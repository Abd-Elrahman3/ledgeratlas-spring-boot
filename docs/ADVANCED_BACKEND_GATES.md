# Advanced Backend Gates

CRUD alone does not complete this project. Every README rule is required, including the following demonstrations.

| Gate | Required demonstration |
| --- | --- |
| D5 | Seed one missing projection and one corrupt test fixture; reconciliation distinguishes rebuildable lag from authoritative inconsistency. |
| D5 | Replay the same stream twice into empty stores yields equal balances and counts, including old schema versions. |
| D6 | Partition regions, attempt promotion, and prove the old writer cannot commit after the new epoch; no split-brain balances. |
| D6 | Kill writer region, promote only after fencing, measure recovery and enumerate any missing acknowledged transfers. |
| D7 | Publish load report and error-budget policy; alerts distinguish dependency outage from invariant violation. |
| D7 | An independent reviewer can reproduce race, replay and regional-failure drills and verify journal balance from SQL alone. |

## Explain before advancing

1. Which invariant belongs in the domain, database, transport and worker respectively?
2. Where is the transaction boundary, and what happens if the process dies immediately after commit?
3. Which operations can safely retry, and how are duplicates or uncertain results resolved?
4. What is bounded: threads, connections, queues, buffers, retries and query sizes?
5. How is authorization enforced below the controller and across caches/messages?
6. Which metric tells you this system is failing before users report it?
7. How do you restore data and prove correctness after recovery?

Project-specific concepts to defend: Architecture from scratch: double-entry ledger, event sourcing, CQRS, serializable transactions, durable workflow, fencing, consistency models, reconciliation, multi-region recovery.

Record commands and results in evidence files. An owner can alter CI; the gates are an honest learning contract, not tamper-proof certification. Mark incomplete behavior openly.
