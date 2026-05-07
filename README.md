# Email Verifier API (email-verifier-api)

Real-time email verification API offering a 16-point verification engine that combines
syntax checking, DNS / MX lookups, SMTP handshakes, mailbox existence probing, catch-all
and greylisting detection, disposable address detection, role-account flagging, spam-trap
and complainer detection, gibberish and offensive-language scanning, B2B / B2C
classification, typo correction, and SMTP-provider identification.

- **Provider site:** https://emailverifierapi.com/
- **Documentation:** https://emailverifierapi.com/api-docs/
- **Pricing:** https://emailverifierapi.com/pricing/
- **Sign up:** https://emailverifierapi.com/register/
- **Free tool:** https://emailverifierapi.com/free-email-verifier/
- **Inbox issue:** [#887](https://github.com/api-evangelist/inbox/issues/887)

## API Surface

Single endpoint at `https://emailverifierapi.com/v2/` accepting GET or POST. API key is
passed as the `apiKey` query parameter. Responses are JSON by default, or XML when
`xml=true` is appended. Each successful response carries the customer's `remaining`
credit balance and `execution` time in seconds.

## Artifacts

| Artifact | Path |
|---|---|
| API index | `apis.yml` |
| OpenAPI 3.1 spec | `openapi/email-verifier-api-openapi.yml` |
| JSON Schema | `json-schema/email-verifier-api-verification-result-schema.json` |
| JSON Structure | `json-structure/email-verifier-api-verification-result-structure.json` |
| JSON-LD context | `json-ld/email-verifier-api-context.jsonld` |
| Examples | `examples/` |
| Spectral ruleset | `rules/email-verifier-api-rules.yml` |
| Naftiko capability | `capabilities/email-verification.yaml` |
| Vocabulary | `vocabulary/email-verifier-api-vocabulary.yml` |
| Plans (API Commons 0.1) | `plans/email-verifier-api-plans-pricing.yml` |
| Rate Limits (API Commons 0.1) | `rate-limits/email-verifier-api-rate-limits.yml` |
| FinOps (FOCUS 1.3) | `finops/email-verifier-api-finops.yml` |

## Verification Outcomes

| Status | Event | Billed |
|---|---|---|
| `passed` | `mailboxExists` | Paid |
| `failed` | `mailboxDoesNotExist` | Paid |
| `failed` | `mailboxIsFull` | Paid |
| `failed` | `domainDoesNotExist` | Free |
| `failed` | `mxServerDoesNotExist` | Free |
| `failed` | `invalidSyntax` | Free |
| `unknown` | `isCatchall` | Free |
| `unknown` | `isGreylisting` | Free |
| `transient` | `transientError` | Free |

## Pricing Snapshot

Pay-as-you-go credit packs that never expire. Free tier of 100 credits at signup; no
credit card required. Per-email price scales from $0.0080 at 1,000 credits down to
$0.0013 at 10,000,000 credits. Enterprise volume contracts available.

## Tags

`Email Verification`, `Deliverability`, `SMTP Check`, `Bounce Prevention`,
`Lead Validation`, `Disposable Detection`, `Spam Trap Detection`,
`Catch-All Detection`, `Greylisting`, `Role Account Detection`, `Typo Suggestion`,
`B2B Lead Scoring`.
