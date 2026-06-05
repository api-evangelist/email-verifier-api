# Email Verifier API (email-verifier-api)

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
