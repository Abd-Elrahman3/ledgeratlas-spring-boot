# Acceptance Tests

Every row is mandatory. M1 includes provided examples in starters; later test skeletons must become real behavior tests. Test names below are descriptions, not a hidden API.

| Milestone | Rule | Scenario and expected result |
| --- | --- | --- |
| M1 | BR-101 | A 500-unit transfer produces -500/+500; mixed currency, zero, negative and overflow values fail unchanged. |
| M1 | BR-102 | Reversing twice is rejected by uniqueness; original journal and audit trail remain readable. |
| M2 | BR-201 | 100 concurrent 100-unit transfers from balance 1,000 produce exactly 10 successes and no negative committed balance. |
| M2 | BR-202 | Two writers with expected version 7 cannot both append version 8; rebuilding without a snapshot yields identical state. |
| M3 | BR-301 | Crash after commit before HTTP response; retry returns the same transfer/journal and original status. |
| M3 | BR-302 | Pause projector: command remains durable while reads report lag; no stale projection authorizes a debit. |
| D4 | BR-401 | IDOR, replayed approval and stale approval fail; failed attempts reveal no other user’s account balance. |
| D4 | BR-402 | A consumer restart processes duplicates without double-crediting the read model. |
| D5 | BR-501 | Seed one missing projection and one corrupt test fixture; reconciliation distinguishes rebuildable lag from authoritative inconsistency. |
| D5 | BR-502 | Replay the same stream twice into empty stores yields equal balances and counts, including old schema versions. |
| D6 | BR-601 | Partition regions, attempt promotion, and prove the old writer cannot commit after the new epoch; no split-brain balances. |
| D6 | BR-602 | Kill writer region, promote only after fencing, measure recovery and enumerate any missing acknowledged transfers. |
| D7 | BR-701 | Publish load report and error-budget policy; alerts distinguish dependency outage from invariant violation. |
| D7 | BR-702 | An independent reviewer can reproduce race, replay and regional-failure drills and verify journal balance from SQL alone. |

## Failure drills

1. Race debits against one account and verify total conservation.
2. Replay all events with an old schema and no snapshots.
3. Fence the old writer before promoting a region during a partition.

## Evidence standard

Record setup, exact command, controlled failure point, durable before/after state, expected result and actual result. A test that only asserts an exception or mocks all persistence cannot prove a database invariant. Use bounded timeouts and deterministic coordination; avoid sleeps as a race strategy. Record a seed for randomized tests. Do not turn an unavailable Docker dependency into a silently skipped passing test.

For D6, deliberately introduce one representative bug and verify a test detects it. For D7, record machine configuration, dataset, concurrency, p50/p95/p99, throughput, errors and resource usage. Restore tests use a different database from the source.
