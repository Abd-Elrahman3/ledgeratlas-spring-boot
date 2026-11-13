# API Test Specification

The authoritative contract is [API Requirements](../API_REQUIREMENTS.md). Build a test matrix for all 7 operations.

For each: test successful response/status/headers, malformed payload, boundary values, missing resource, duplicate request, authorization denial and relevant unavailable dependency. Check database side effects after rejection. Use exact request bytes for signatures, real JWT validation tests for issuer/audience, and protocol-specific assertions for GraphQL/gRPC/streaming.

| Rule | Required sample |
| --- | --- |
| BR-401 | IDOR, replayed approval and stale approval fail; failed attempts reveal no other user’s account balance. |
| BR-402 | A consumer restart processes duplicates without double-crediting the read model. |
| BR-501 | Seed one missing projection and one corrupt test fixture; reconciliation distinguishes rebuildable lag from authoritative inconsistency. |
| BR-502 | Replay the same stream twice into empty stores yields equal balances and counts, including old schema versions. |

Do not call real carrier/payment/customer services. Build deterministic local provider fixtures and record request counts and idempotency keys.
