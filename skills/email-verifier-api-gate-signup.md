---
name: Gate a signup form with real-time verification
description: >-
  Verify an address inline at registration — blocking disposable, gibberish and invalid
  addresses, recovering typos, and failing open when the verifier is unavailable.
api: openapi/email-verifier-api-verification-api-openapi.yml
operations:
  - verifyEmailPost
generated: '2026-08-13'
method: generated
source: openapi/email-verifier-api-verification-api-openapi.yml
---

# Gate a signup form with real-time verification

## Where the call belongs

Call from your **server**, on form submit, using `verifyEmailPost`. Never call from the
browser: the API key is a query parameter, so a client-side call publishes your credential to
every visitor. There is no public/publishable key variant.

Typical verification time is well under a second (`execution` reports the actual figure per
call), which is inside the budget for a synchronous submit handler — but see step 5 for what
to do when it is not.

## Steps

1. **Do the free checks locally first.** Syntax validation, your own disposable-domain
   denylist, and a check against addresses already registered. Anything you reject locally
   costs nothing; the API's own `invalidSyntax` and `domainDoesNotExist` events are free too,
   but the round trip is not free in latency.

2. **Call `verifyEmailPost`** with `apiKey` in the query string and `email` in the form body.

3. **Handle `emailSuggested` before anything else.** If it is non-null, the user mistyped a
   common domain. Do not reject and do not silently correct — return the suggestion to the form
   and ask: *"Did you mean john@gmail.com?"* This recovers a real registration that a hard
   rejection would have lost, and it is the highest-value single behaviour in this flow.

4. **Apply your registration policy** to `status` and the flags:

   | Signal                                       | Suggested action at signup                  |
   |----------------------------------------------|---------------------------------------------|
   | `status: failed` + `invalidSyntax`           | Reject inline: "That address isn't valid."  |
   | `status: failed` + `domainDoesNotExist` / `mxServerDoesNotExist` | Reject inline: the domain cannot receive mail |
   | `status: failed` + `mailboxDoesNotExist`     | Reject inline, offer the typo suggestion if present |
   | `isDisposable: true`                         | Reject if you have a burner-account problem; otherwise flag the account |
   | `isGibberish` or `isOffensive`               | Flag for review — fraud/abuse signal, not necessarily a rejection |
   | `possibleSpamtrap` or `isComplainer`         | Allow registration, but suppress from marketing mail from day one |
   | `isRoleAccount: true`                        | Usually allow — `support@` is a legitimate signup for B2B products |
   | `status: unknown` (`isCatchall`, `isGreylisting`) | **Allow.** Existence is unprovable; rejecting punishes legitimate corporate domains |
   | `status: passed`                             | Allow                                       |

5. **Fail open on infrastructure errors.** If the call returns `429`, `503`, `transient`, or
   times out, **let the registration through** and mark the address unverified for an
   asynchronous re-check. Blocking signups because a third-party verifier is degraded costs far
   more than admitting a few bad addresses. Set a hard client timeout — a second or two — and
   treat exceeding it the same way.

6. **Fail open on `402` as well, and alert.** An exhausted credit balance is an operational
   problem, not a user problem. Watch `remaining` on every response and alarm well before zero;
   a signup form that silently starts rejecting everyone because the balance ran out is a
   worst-case outage.

7. **Store the verdict on the user record** — `status`, `event`, and the flags. It is what lets
   you decide later whether to send to that user, and it saves re-verifying the same address on
   every subsequent touchpoint.

## What not to do

- Do not call the API on every keystroke or on field blur. It is billed per mailbox-level
  outcome; a debounce on blur will still burn credits on partial addresses. Verify on submit.
- Do not treat a 200 OK as a valid address. Every verdict, including `failed`, arrives as 200.
- Do not reject `unknown`. Catch-all is the normal configuration for a great many corporate
  mail domains, and rejecting it excludes exactly the B2B users you most want.
- Do not re-verify an address you already verified for this user. There is no idempotency key
  and no de-duplication; the second call is billed.
