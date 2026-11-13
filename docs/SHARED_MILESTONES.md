# Shared Milestones (M1–M3)

Rules are mandatory and cumulative. README section 5 is authoritative. Testing is part of each milestone; the AQA guide is complementary, not an alternative that skips implementation.

## M1 — Domain and invariants

**Branch:** `feature/m1-domain`

**Goal:** Balance every journal; Keep immutable history.

**You will practise:** Architecture from scratch: double-entry ledger, event sourcing, CQRS, serializable transactions, durable workflow, fencing, consistency models, reconciliation, multi-region recovery.

| Done | Rule | What to implement |
| --- | --- | --- |
| [ ] | BR-101 | Balance every journal — A 500-unit transfer produces -500/+500; mixed currency, zero, negative and overflow values fail unchanged. |
| [ ] | BR-102 | Keep immutable history — Reversing twice is rejected by uniqueness; original journal and audit trail remain readable. |
**Required tests:** both rule examples, at least one boundary and one failure/rollback scenario from [Acceptance Tests](ACCEPTANCE_TESTS.md).

**Done when:** these tests and every earlier milestone pass; `docs/evidence/M1.md` contains reproducible evidence and an explanation of the invariant.

**Trap:** implementing only the HTTP happy path, or checking a rule in Java while leaving the database race unprotected.

## M2 — Database and persistence

**Branch:** `feature/m2-database`

**Goal:** Commit atomically under contention; Enforce stream concurrency.

**You will practise:** Architecture from scratch: double-entry ledger, event sourcing, CQRS, serializable transactions, durable workflow, fencing, consistency models, reconciliation, multi-region recovery.

| Done | Rule | What to implement |
| --- | --- | --- |
| [ ] | BR-201 | Commit atomically under contention — 100 concurrent 100-unit transfers from balance 1,000 produce exactly 10 successes and no negative committed balance. |
| [ ] | BR-202 | Enforce stream concurrency — Two writers with expected version 7 cannot both append version 8; rebuilding without a snapshot yields identical state. |
**Required tests:** both rule examples, at least one boundary and one failure/rollback scenario from [Acceptance Tests](ACCEPTANCE_TESTS.md).

**Done when:** these tests and every earlier milestone pass; `docs/evidence/M2.md` contains reproducible evidence and an explanation of the invariant.

**Trap:** implementing only the HTTP happy path, or checking a rule in Java while leaving the database race unprotected.

## M3 — Transactional use cases

**Branch:** `feature/m3-transactions`

**Goal:** Implement idempotent commands; Separate CQRS read models.

**You will practise:** Architecture from scratch: double-entry ledger, event sourcing, CQRS, serializable transactions, durable workflow, fencing, consistency models, reconciliation, multi-region recovery.

| Done | Rule | What to implement |
| --- | --- | --- |
| [ ] | BR-301 | Implement idempotent commands — Crash after commit before HTTP response; retry returns the same transfer/journal and original status. |
| [ ] | BR-302 | Separate CQRS read models — Pause projector: command remains durable while reads report lag; no stale projection authorizes a debit. |
**Required tests:** both rule examples, at least one boundary and one failure/rollback scenario from [Acceptance Tests](ACCEPTANCE_TESTS.md).

**Done when:** these tests and every earlier milestone pass; `docs/evidence/M3.md` contains reproducible evidence and an explanation of the invariant.

**Trap:** implementing only the HTTP happy path, or checking a rule in Java while leaving the database race unprotected.
