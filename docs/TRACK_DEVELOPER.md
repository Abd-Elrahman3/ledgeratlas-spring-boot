# Developer Track (D4–D7)

Rules are mandatory and cumulative. README section 5 is authoritative. Testing is part of each milestone; the AQA guide is complementary, not an alternative that skips implementation.

## D4 — API contracts and authorization

**Branch:** `feature/d4-api`

**Goal:** Secure transfer boundaries; Publish immutable events safely.

**You will practise:** Architecture from scratch: double-entry ledger, event sourcing, CQRS, serializable transactions, durable workflow, fencing, consistency models, reconciliation, multi-region recovery.

| Done | Rule | What to implement |
| --- | --- | --- |
| [ ] | BR-401 | Secure transfer boundaries — IDOR, replayed approval and stale approval fail; failed attempts reveal no other user’s account balance. |
| [ ] | BR-402 | Publish immutable events safely — A consumer restart processes duplicates without double-crediting the read model. |
**Required tests:** both rule examples, at least one boundary and one failure/rollback scenario from [Acceptance Tests](ACCEPTANCE_TESTS.md).

**Done when:** these tests and every earlier milestone pass; `docs/evidence/D4.md` contains reproducible evidence and an explanation of the invariant.

**Trap:** implementing only the HTTP happy path, or checking a rule in Java while leaving the database race unprotected.

## D5 — Advanced behavior and resource limits

**Branch:** `feature/d5-advanced`

**Goal:** Reconcile ledger invariants; Rebuild and verify projections.

**You will practise:** Architecture from scratch: double-entry ledger, event sourcing, CQRS, serializable transactions, durable workflow, fencing, consistency models, reconciliation, multi-region recovery.

| Done | Rule | What to implement |
| --- | --- | --- |
| [ ] | BR-501 | Reconcile ledger invariants — Seed one missing projection and one corrupt test fixture; reconciliation distinguishes rebuildable lag from authoritative inconsistency. |
| [ ] | BR-502 | Rebuild and verify projections — Replay the same stream twice into empty stores yields equal balances and counts, including old schema versions. |
**Required tests:** both rule examples, at least one boundary and one failure/rollback scenario from [Acceptance Tests](ACCEPTANCE_TESTS.md).

**Done when:** these tests and every earlier milestone pass; `docs/evidence/D5.md` contains reproducible evidence and an explanation of the invariant.

**Trap:** implementing only the HTTP happy path, or checking a rule in Java while leaving the database race unprotected.

## D6 — Failure testing and recovery

**Branch:** `feature/d6-failures`

**Goal:** Prevent two regional writers; Practice disaster recovery.

**You will practise:** Architecture from scratch: double-entry ledger, event sourcing, CQRS, serializable transactions, durable workflow, fencing, consistency models, reconciliation, multi-region recovery.

| Done | Rule | What to implement |
| --- | --- | --- |
| [ ] | BR-601 | Prevent two regional writers — Partition regions, attempt promotion, and prove the old writer cannot commit after the new epoch; no split-brain balances. |
| [ ] | BR-602 | Practice disaster recovery — Kill writer region, promote only after fencing, measure recovery and enumerate any missing acknowledged transfers. |
**Required tests:** both rule examples, at least one boundary and one failure/rollback scenario from [Acceptance Tests](ACCEPTANCE_TESTS.md).

**Done when:** these tests and every earlier milestone pass; `docs/evidence/D6.md` contains reproducible evidence and an explanation of the invariant.

**Trap:** implementing only the HTTP happy path, or checking a rule in Java while leaving the database race unprotected.

## D7 — Operations and design defense

**Branch:** `feature/d7-operations`

**Goal:** Meet measurable service objectives; Defend every guarantee.

**You will practise:** Architecture from scratch: double-entry ledger, event sourcing, CQRS, serializable transactions, durable workflow, fencing, consistency models, reconciliation, multi-region recovery.

| Done | Rule | What to implement |
| --- | --- | --- |
| [ ] | BR-701 | Meet measurable service objectives — Publish load report and error-budget policy; alerts distinguish dependency outage from invariant violation. |
| [ ] | BR-702 | Defend every guarantee — An independent reviewer can reproduce race, replay and regional-failure drills and verify journal balance from SQL alone. |
**Required tests:** both rule examples, at least one boundary and one failure/rollback scenario from [Acceptance Tests](ACCEPTANCE_TESTS.md).

**Done when:** these tests and every earlier milestone pass; `docs/evidence/D7.md` contains reproducible evidence and an explanation of the invariant.

**Trap:** implementing only the HTTP happy path, or checking a rule in Java while leaving the database race unprotected.
