# Implementation Labs and Contract Details

These details are part of the requirements and refine the README rules.

## Ledger fixture and invariants

Seed customer Alice/Bob and an explicit system funding account through a balanced opening journal. Never create spendable customer balances by an unbalanced direct UPDATE. Only customer accounts have the no-overdraft rule; identify system accounts and constraints in the design. Use USD only in first tests, then EUR without currency conversion. Sum postings by currency and reconcile against the source of funds.

Provide a deferred constraint trigger or an equally explicit atomic posting strategy that checks balanced journals at commit; ordinary row CHECK constraints cannot aggregate postings. The runtime role cannot alter posted journals. Corrections require reversal authorization and immutable links to the original.

## Approval and event-sourcing contract

Choose and document a high-value threshold (baseline 100000 minor units). Approval token binds actor,source,destination,amount,currency,expiry<=60s and a nonce; consume once in the transfer transaction. A replay of the same transfer idempotency key returns the old result without consuming another approval. JWT alone is insufficient evidence of the extra approval step.

Treat authoritative journal/event writes as one database transaction. Stored event facts include the effective time and monetary amounts needed for deterministic replay. A rebuilding projector must never invoke provider side effects. Expose projection version so clients can distinguish stale reads from missing data.

## Regional failover lab

Use two isolated database/process groups in a reproducible local simulation or dedicated test environment. Describe the lease/epoch authority and prove old-writer rejection with a live stale process. If the fencing authority is unavailable, reject new writes rather than promoting based only on a timeout. Record potential loss from asynchronous replication; compare client-acknowledged IDs against recovered IDs and produce a discrepancy report. This is a learning simulation, not a claim of production financial compliance.
