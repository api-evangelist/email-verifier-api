---
name: Verify a single email address
description: >-
  Call Email Verifier API once for one address and turn the response into a correct
  send / do-not-send / retry decision, without mistaking an HTTP 200 for deliverability.
api: openapi/email-verifier-api-verification-api-openapi.yml
operations:
  - verifyEmailPost
  - verifyEmailGet
generated: '2026-08-13'
method: generated
source: openapi/email-verifier-api-verification-api-openapi.yml
---

# Verify a single email address

## Before you start

- Base URL is `https://emailverifierapi.com/v2`. There is one path: `/`.
- The API key goes in the **`apiKey` query parameter**. There is no header form. Treat the
  URL as sensitive — it lands in access logs.
- Use **`verifyEmailPost`**, not `verifyEmailGet`, for anything automated. Both return the
  same document, but POST carries the address in an `application/x-www-form-urlencoded` body
  so it does not appear in URLs, proxy logs, or Referer headers. Reserve `verifyEmailGet` for
  ad-hoc checks and no-code tools.

## Steps

1. **Normalize the address** before sending it: trim whitespace, lowercase the domain, and
   reject anything with no `@`. This costs nothing and avoids spending a credit on input you
   could have rejected locally.

2. **Check your local cache.** The API has no idempotency key and no de-duplication. If you
   have verified this exact normalized address recently, reuse that result — a repeat call for
   a mailbox-level outcome is billed again.

3. **Call `verifyEmailPost`** with `apiKey` in the query string and `email` in the form body.
   Leave `xml` unset; JSON is the default and the only sensible machine format. Do **not** send
   an `Accept` header expecting negotiation — this API ignores it and switches format only on
   `?xml=true`.

4. **Branch on `status`, never on the HTTP status code.** A 200 OK is returned for every
   verdict, including undeliverable ones. This is the single most common integration error
   against this API.

   | `status`    | What it means                                  | What to do                                            |
   |-------------|------------------------------------------------|-------------------------------------------------------|
   | `passed`    | Mailbox accepts mail                           | Safe to send                                          |
   | `failed`    | Mailbox, domain, or MX is invalid              | Do not send; suppress the address                     |
   | `unknown`   | Catch-all or greylisting; indeterminate        | Do not treat as valid; see step 6                     |
   | `transient` | Temporary failure during verification          | Retry after a delay (see step 7)                      |

5. **Then read `event`** for the reason, and log it — it is what makes a suppression decision
   auditable later:
   - `mailboxExists` (billed) — confirmed.
   - `mailboxDoesNotExist`, `mailboxIsFull` (billed) — hard failures.
   - `invalidSyntax`, `domainDoesNotExist`, `mxServerDoesNotExist` (free) — the address was
     never viable; these cost no credit.
   - `isCatchall`, `isGreylisting` (free) — indeterminate, not failure.
   - `transientError` (free) — retry.

6. **Handle `emailSuggested` before you discard a failure.** When a domain typo is detected the
   response carries a corrected address (`gmial.com` → `gmail.com`). At a signup form, prompt
   the user with the suggestion rather than rejecting them. In a batch, treat it as a candidate
   for re-verification, not as a verified address — the suggestion has not itself been checked.

7. **Apply the intelligence flags according to your use case**, not as a blanket filter. They
   are independent of `status`; an address can be `passed` and still be one you should not mail:
   - `isDisposable` — burner provider. Block at signup; suppress in marketing.
   - `isRoleAccount` — `info@`, `sales@`, `support@`. Usually fine transactionally, poor for
     cold outreach.
   - `possibleSpamtrap`, `isComplainer` — reputation hazards. Suppress from marketing sends
     regardless of `status`.
   - `isFreeService` — consumer mailbox. A B2B-vs-B2C signal, not a quality signal.
   - `isGibberish`, `isOffensive` — fraud and abuse signals at registration.

8. **Retry only `transient` and 429/503**, with exponential backoff and jitter. No
   `Retry-After` or `RateLimit-*` header is published, so choose your own schedule. Never retry
   a `failed` verdict — the answer will not change and a Paid event bills again.

## Errors

All errors reuse the same `{status, event, details}` envelope as a success. There is no
`application/problem+json`.

- `401` — missing or invalid `apiKey`. Not retryable; fix the credential.
- `402` — out of credits. Not retryable until the balance is topped up.
- `429` — rate limited. Back off.
- `503` — service unavailable. Back off.

## Budgeting

Every successful response carries `remaining`, the credit balance after the call — note it is
serialized as a **string**, so parse it rather than comparing it. Read it on every call, and
stop dispatching work below a floor you choose, because the only signal that you are out of
credits is a 402 on the next request. `execution` gives the wall-clock seconds the verification
took and is useful for tuning your client timeout.
