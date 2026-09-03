---
chapter: 22
title: Transaction Management
source: "Connolly & Begg, Database Systems, 6th Ed, Ch.22.1–22.3"
course: "[[00 - Course Map|CSC 2107 - Databases]]"
tags: [databases, csc2107, exam-material, transactions, concurrency]
---

# Ch.22 — Transaction Management

## 1. Why this matters
Transactions and the **ACID properties** are guaranteed exam vocabulary — plus concurrency control (locking) and recovery are where DB theory connects to real-world reliability guarantees. Classic asks: **"define the ACID properties"**, **"explain a lost-update / dirty-read problem with an example"**, **"trace 2PL / explain deadlock."**

---

## 2. What is a transaction?
A **transaction** is a logical unit of work — an action or series of actions carried out by a single user/program, which reads/updates the contents of the database, and takes the database from one CONSISTENT state to another. A transaction ends with either a `COMMIT` (making all changes permanent) or a `ROLLBACK`/`ABORT` (undoing all changes, as if nothing happened).

### ACID Properties (THE single most guaranteed exam question in this chapter — know each precisely)
| Property | Meaning |
|---|---|
| **Atomicity** | A transaction is "all or nothing" — either ALL its operations complete, or NONE do (no partial execution left in the database) |
| **Consistency** | A transaction takes the database from one valid (constraint-satisfying) state to another valid state — it never leaves the database in a state that violates integrity constraints |
| **Isolation** | The effects of a transaction are not visible to other concurrently-running transactions until it commits — concurrent transactions behave as if they ran one at a time (serially), even though they may actually interleave |
| **Durability** | Once a transaction commits, its changes are PERMANENT, surviving even a subsequent system crash |

> **Exam trap**: Consistency here is about the DATABASE's integrity constraints (business rules, keys), NOT about "reading the same value twice" (that's what Isolation governs) — these two are commonly confused in short-answer definitions.

---

## 3. Concurrency control — why it's needed
Without control, multiple transactions running "simultaneously" (interleaved) on shared data can cause problems (guaranteed exam list — know each with a concrete tiny example):

| Problem | What happens |
|---|---|
| **Lost update** | Two transactions both read the same value, both compute an update based on that read value, and the SECOND one's write overwrites the FIRST one's write entirely — the first update is silently lost |
| **Uncommitted dependency (dirty read)** | A transaction reads a value that was written by ANOTHER transaction that hasn't committed yet — if that other transaction later rolls back, the first transaction used a value that never "really" existed |
| **Inconsistent analysis** | A transaction reads several values while ANOTHER transaction is in the middle of updating some of them, producing a summary/total that never actually existed at any single point in time |

---

## 4. Serializability and recoverability
- **Serializability**: the gold-standard correctness criterion for concurrent execution — an interleaved (concurrent) execution of transactions is **serializable** if its final result is EQUIVALENT to some SERIAL (one-at-a-time, non-interleaved) execution of the same transactions. This is what "correct" concurrency control is actually trying to guarantee — you don't need transactions to literally run one at a time, just to produce a result AS IF they had.
- **Recoverable schedule**: a schedule where, if transaction T2 reads data written by T1, then T1's COMMIT must happen BEFORE T2's commit — this ensures that if T1 has to be rolled back, we never end up needing to also roll back an already-committed T2 (which would violate durability).

---

## 5. Locking methods — the main mechanism for enforcing serializability
- **Shared lock (S-lock / read lock)**: allows the transaction to READ an item; multiple transactions can hold a shared lock on the SAME item simultaneously.
- **Exclusive lock (X-lock / write lock)**: allows the transaction to READ and WRITE an item; only ONE transaction can hold an exclusive lock on an item, and no other transaction can hold ANY lock (shared or exclusive) on it at the same time.

### Two-Phase Locking (2PL) — guaranteed exam algorithm
Every transaction has two distinct phases:
1. **Growing phase**: the transaction can ACQUIRE locks, but cannot release any.
2. **Shrinking phase**: the transaction can RELEASE locks, but cannot acquire any new ones.
**Guarantee**: 2PL guarantees serializability (this is a proven theoretical result — a key fact to state on exams).
**Strict 2PL** (the variant almost always used in practice): all EXCLUSIVE locks are held until the transaction actually commits or aborts (not released early even in the shrinking phase) — this additionally avoids cascading rollbacks (where rolling back one transaction would force rolling back others that read its uncommitted data).

---

## 6. Deadlock
Occurs when two (or more) transactions are each waiting for a lock held by the OTHER, so NEITHER can ever proceed — a circular wait.
**Detection**: the DBMS periodically checks for a cycle in a "wait-for graph" (transaction A waits for a lock held by B, B waits for a lock held by A → cycle → deadlock).
**Resolution**: the DBMS picks a "victim" transaction (often the one that's done the least work, or is cheapest to restart) and forcibly ROLLS IT BACK, releasing its locks so the other transaction(s) can proceed.
**Prevention** (alternative to detection): approaches like requiring transactions to request all locks upfront, or ordering lock requests, prevent the circular-wait condition from ever arising in the first place — at the cost of reduced concurrency.

---

## 7. Timestamping methods (an alternative to locking, brief)
Instead of locks, each transaction is given a unique **timestamp** when it starts. Conflicting operations are ordered/resolved based on comparing timestamps (an older transaction's operations are generally prioritized over a younger one's, on the same data item) — this achieves serializability WITHOUT using locks at all, trading lock-contention overhead for the cost of managing/comparing timestamps and potentially restarting transactions.

---

## 8. Database recovery — the need for it
Recovery restores the database to a CONSISTENT state after a failure (system crash, media failure, transaction failure).
- **Recovery facilities**: a **log** (journal) recording every database update (before/after images), **checkpoints** (periodic snapshots reducing how much log must be replayed after a crash), and backup copies.
- **Recovery techniques**: use the log to either **REDO** (reapply) the effects of committed transactions that might not have been physically written to disk yet, or **UNDO** (roll back) the effects of transactions that were in progress but not committed at the time of failure.

---

## 9. Quick self-test
1. Define the four ACID properties precisely, and clarify the difference between Consistency and Isolation.
2. Describe the lost update problem with a small concrete example (two transactions, starting value, two updates).
3. Define serializability, and explain why it doesn't require transactions to literally run one at a time.
4. Distinguish a shared lock from an exclusive lock.
5. Describe the two phases of Two-Phase Locking, and state what 2PL guarantees.
6. What's the difference between plain 2PL and Strict 2PL, and what problem does Strict 2PL additionally avoid?
7. Define deadlock, and describe how a DBMS detects and resolves it.
8. What are the two general recovery actions (using the log) after a crash, and when is each used?
