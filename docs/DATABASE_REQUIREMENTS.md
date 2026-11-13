# Database Requirements

## 1. Ownership and schema

Authoritative PostgreSQL tables accounts(id,owner_subject,currency,status,version), transfers(id,source_id FK,destination_id FK,amount_minor BIGINT CHECK(amount_minor>0),currency,status), journals(id,transfer_id UNIQUE,posted_at,reverses_journal_id UNIQUE NULL), postings(journal_id FK,account_id FK,currency,amount_minor,ordinal, PRIMARY KEY(journal_id,ordinal)), events(aggregate_id,aggregate_version,event_id UNIQUE,schema_version,payload JSONB,occurred_at, UNIQUE(aggregate_id,aggregate_version)), idempotency_records(subject,operation,key,hash,response,expires_at), outbox,inbox,writer_epochs and approval_tokens. Balance per journal requires a transaction-aware/deferred validation strategy; a simple per-row CHECK cannot sum other rows. Separate projection schema stores balances,history,checkpoint and snapshot metadata. Backups, WAL retention and event retention must support declared RPO and replay horizon. Distinguish signed posting amounts from positive transfer amounts and check BIGINT overflow in Java and SQL.

## 2. Required artifacts

- ERD with cardinality, PK, FK, nullability and ownership.
- Versioned forward migrations for every constraint/index; no automatic schema update in final configuration.
- Seed fixtures for normal, duplicate, missing-reference and boundary cases.
- Explicit transaction isolation, lock order, timeout and retry policy.
- EXPLAIN (ANALYZE, BUFFERS) for the busiest query before and after an index; include row counts and hardware.
- Upgrade test from a prior schema fixture, not only create-from-empty.
- Backup/restore commands for a disposable destination and SQL integrity checks.

## 3. Testing rules

Run actual PostgreSQL semantics. Use separate connections/processes for concurrent transactions and deterministic barriers; bound all waits. A service-level existence check is not a uniqueness guarantee. If the app relies on a constraint, test direct SQL violating it. Explain any SELECT FOR UPDATE, SKIP LOCKED, RLS, serializable retry or projection checkpoint used here.

## 4. Migration discipline

An applied migration is immutable. Use expand/backfill/contract for incompatible changes, and state how old/new application versions coexist. Separate migration privileges from runtime privileges. Document retention and restore behavior for derived data, deduplication records and secrets. Do not ship real user data in fixtures.

See [Implementation Labs and Contract Details](IMPLEMENTATION_LABS.md) for concrete fixtures, payload semantics and boundary experiments. These are required, not optional extras.
