---
name: Clean a marketing list before a send
description: >-
  Verify a list of addresses against Email Verifier API without a batch endpoint — how to
  fan out safely, what to suppress, what to keep, and how to stay inside a credit budget.
api: openapi/email-verifier-api-verification-api-openapi.yml
operations:
  - verifyEmailPost
generated: '2026-08-13'
method: generated
source: openapi/email-verifier-api-verification-api-openapi.yml
---

# Clean a marketing list before a send

## What this API does and does not give you

There is **no batch operation**. The v2 API has exactly two operations, both single-address,
and no job, upload, or result-polling surface. Cleaning a list means calling
`verifyEmailPost` once per address and assembling the outcome yourself. Nothing is persisted
server-side — if you do not store the result, it is gone, and re-checking costs credits again.

There is also no published rate limit and no rate-limit response header. Choose a
conservative concurrency (start low, watch for `429`) rather than assuming headroom.

## Steps

1. **Deduplicate and normalize first.** Lowercase domains, trim whitespace, drop malformed
   entries locally. On a real list this typically removes several percent of the rows before a
   single credit is spent.

2. **Subtract what you already know.** Merge against your existing verification store and skip
   any address verified recently with a terminal verdict (`passed` or a hard `failed`). Only
   `unknown` and `transient` rows are worth re-checking. There is no idempotency key and no
   server-side de-duplication, so this cache is the only thing preventing double billing.

3. **Budget the run.** Only mailbox-level outcomes are billed: `mailboxExists`,
   `mailboxDoesNotExist`, `mailboxIsFull`. Syntax, DNS, MX, catch-all and greylisting outcomes
   are free. Your worst case is therefore one credit per remaining address; your actual cost
   will be lower in proportion to how much junk the list carries. Confirm the balance covers
   the worst case before starting — `remaining` on any single response gives you the current
   figure.

4. **Fan out with bounded concurrency.** Call `verifyEmailPost` per address. Keep a small
   worker pool, and read `remaining` from each response to track the burn rate in real time.
   Halt the run if `remaining` drops below your reserve rather than discovering the wall as a
   stream of `402`s.

5. **Bucket every result** as you go. Do not collapse to a boolean:

   | Bucket        | Condition                                                       | Action                      |
   |---------------|-----------------------------------------------------------------|-----------------------------|
   | Send          | `status: passed`, no reputation flag                             | Keep in the send            |
   | Suppress-hard | `status: failed`                                                 | Remove permanently          |
   | Suppress-risk | any `status`, with `possibleSpamtrap` or `isComplainer` true     | Remove from marketing sends |
   | Review        | `status: unknown` (`isCatchall`, `isGreylisting`)                | See step 6                  |
   | Requeue       | `status: transient`                                              | Retry later, same run or next |
   | Typo          | `emailSuggested` is non-null                                     | See step 7                  |

6. **Decide a catch-all policy up front.** `isCatchall` means the destination accepts every
   address, so existence is unprovable — the verdict is genuinely `unknown`, not a soft pass.
   Two defensible policies: exclude them from cold sends and include them for re-engagement of
   known-good historical contacts; or include them but segment them so their bounces do not
   contaminate your main send's reputation metrics. Pick one and record it — this bucket is
   usually the largest source of argument after a send.

7. **Feed typos back through the loop.** When `emailSuggested` is populated, the suggestion has
   **not** been verified. Add it to the next pass as a new address; do not promote it into the
   send list on the strength of the suggestion alone.

8. **Apply the B2B/B2C and role signals to segmentation, not deletion.** `isFreeService` marks a
   consumer mailbox and `isRoleAccount` marks a departmental alias. For cold B2B outreach both
   are usually exclusions; for a transactional or account-notification list neither is.
   `isDisposable` is an exclusion in essentially every list context.

9. **Write the run down.** Store per address: `status`, `event`, the seven flags, `mxLocation`,
   and the timestamp. That record is what lets you re-run the list cheaply next quarter, and it
   is the evidence trail behind any suppression a stakeholder later questions.

## Errors during a run

- `429` — you found the unpublished ceiling. Reduce concurrency, back off exponentially with
  jitter, resume. No `Retry-After` is returned, so pick your own interval.
- `402` — credits exhausted mid-run. Checkpoint your progress by address so the run resumes
  rather than restarts; restarting re-bills every address already verified.
- `503` — back off and resume.
- `401` — stop the run entirely; every remaining call will fail identically.

## After the send

Reconcile actual bounces against the `passed` bucket. A `passed` address that hard-bounces is
the only real accuracy measurement you will get, and the rate at which it happens is what tells
you whether the catch-all policy you chose in step 6 was right.
