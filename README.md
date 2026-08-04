# Email Verifier API (email-verifier-api)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Email Verifier API is a real-time email verification service offering a 16-point engine
that validates deliverability through syntax checking, DNS / MX lookups, real-time SMTP
handshakes, mailbox existence probing, catch-all and greylisting detection, disposable
address detection, role-account flagging, spam-trap and complainer detection, gibberish
and offensive-language scanning, B2B / B2C classification, typo correction, and SMTP
provider identification. The service is delivered as a single REST endpoint that
accepts GET or POST requests, returns JSON or XML, and meters usage against a
credit-pack balance that never expires. The product targets growth teams, ESPs, and
lead-generation operators that need to eliminate hard bounces and protect sender
reputation before they send.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/email-verifier-api/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/email-verifier-api/refs/heads/main/apis.yml)

## Scope

- **Type:** Index

## Tags

- Email Verification
- Deliverability
- SMTP Check
- Bounce Prevention
- Lead Validation
- Disposable Detection
- Spam Trap Detection
- Catch-All Detection
- Greylisting
- Role Account Detection
- Typo Suggestion
- B2B Lead Scoring

## Timestamps

- **Created:** 2026-05-06
- **Modified:** 2026-05-19

## APIs

### Email Verifier API Verification

Real-time email-address verification endpoint. Single resource at
https://emailverifierapi.com/v2/ accepting GET or POST. Returns a top-level
deliverability status (passed / failed / unknown / transient), a granular event
code (mailboxExists, mailboxDoesNotExist, mailboxIsFull, domainDoesNotExist,
mxServerDoesNotExist, invalidSyntax, isCatchall, isGreylisting, transientError),
and a set of intelligence flags (isDisposable, isRoleAccount, isFreeService,
possibleSpamtrap, isComplainer, isOffensive, isGibberish), along with the MX
server IP and ISO country, a typo-corrected address suggestion, the remaining
credit balance, and the wall-clock execution time. Authentication is by API key
passed as the `apiKey` query parameter.

- **Human URL:** [https://emailverifierapi.com/api-docs/](https://emailverifierapi.com/api-docs/)
- **Base URL:** `https://emailverifierapi.com/v2/`

#### Tags

- Email Verification
- Deliverability
- SMTP Check
- Bounce Prevention

#### Properties

- [Documentation](https://emailverifierapi.com/api-docs/)
- [Sign Up](https://emailverifierapi.com/register/)
- [Pricing](https://emailverifierapi.com/pricing/)
- [Terms of Service](https://emailverifierapi.com/terms-of-service/)
- [Privacy Policy](https://emailverifierapi.com/privacy-policy/)
- [Login](https://emailverifierapi.com/login/)
- [Blog](https://emailverifierapi.com/blog/)
- [Integrations](https://emailverifierapi.com/integrations/)
- [Free Tool](https://emailverifierapi.com/free-email-verifier/)
- [Directory](https://emailverifierapi.com/verify-company-emails/)
- [OpenAPI](https://raw.githubusercontent.com/api-evangelist/email-verifier-api/refs/heads/main/openapi/email-verifier-api-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [JSON Schema](https://raw.githubusercontent.com/api-evangelist/email-verifier-api/refs/heads/main/json-schema/email-verifier-api-verification-result-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Structure](https://raw.githubusercontent.com/api-evangelist/email-verifier-api/refs/heads/main/json-structure/email-verifier-api-verification-result-structure.json)
- [JSON-LD](https://raw.githubusercontent.com/api-evangelist/email-verifier-api/refs/heads/main/json-ld/email-verifier-api-context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)
- [Spectral Rules](https://raw.githubusercontent.com/api-evangelist/email-verifier-api/refs/heads/main/rules/email-verifier-api-rules.yml)
- [Vocabulary](https://raw.githubusercontent.com/api-evangelist/email-verifier-api/refs/heads/main/vocabulary/email-verifier-api-vocabulary.yml)
- [Plans](https://raw.githubusercontent.com/api-evangelist/email-verifier-api/refs/heads/main/plans/email-verifier-api-plans-pricing.yml)
- [Rate Limits](https://raw.githubusercontent.com/api-evangelist/email-verifier-api/refs/heads/main/rate-limits/email-verifier-api-rate-limits.yml)
- [Fin Ops](https://raw.githubusercontent.com/api-evangelist/email-verifier-api/refs/heads/main/finops/email-verifier-api-finops.yml)
- [Postman Collection](collections/email-verifier-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/email-verifier-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [Documentation](https://emailverifierapi.com/api-docs/)
- [Sign Up](https://emailverifierapi.com/register/)
- [Login](https://emailverifierapi.com/login/)
- [Pricing](https://emailverifierapi.com/pricing/)
- [Blog](https://emailverifierapi.com/blog/)
- [Terms of Service](https://emailverifierapi.com/terms-of-service/)
- [Privacy Policy](https://emailverifierapi.com/privacy-policy/)
- [Integrations](https://emailverifierapi.com/integrations/)
- [Free Tool](https://emailverifierapi.com/free-email-verifier/)
- [Directory](https://emailverifierapi.com/verify-company-emails/)
- [Support](mailto:support@emailverifierapi.com)
- [Authentication](undefined)
- [Features](undefined)
- [Use Cases](undefined)
- [Integrations](undefined)
- [SDK](undefined)
- [L L Ms Txt](https://emailverifierapi.com/llms.txt)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
