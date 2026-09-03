# CSC 2107 — Practice Questions: Ch.10 Design Lifecycle, Ch.12-13 ER/EER, Ch.14-15 Normalization, Ch.22 Transactions, Ch.20 Security

Closed-book. Cover the answer key until you've committed to an answer.

## Design Lifecycle (Ch.10)
1. Put these in correct lifecycle order: Testing, Database Design, Requirements Collection, Implementation, Database Planning.
2. Explain why conceptual design is described as independent of the "data model type," while logical design is not.

## ER / EER Modeling (Ch.12–13)
3. A university has Students and Courses, and a Student *Enrolls In* a Course, recording an `enrollmentDate`. Model this in words: what's the relationship's degree, and where does `enrollmentDate` belong?
4. A Library has Books and Members. A Book can be borrowed by many Members over time (not simultaneously), and a Member can borrow many Books. Classify this relationship's cardinality, and identify whether participation is mandatory or optional for each side (a book might sit un-borrowed; a member might never borrow anything).
5. Model "Vehicle" as a superclass with "Car" and "Motorbike" subclasses, where every vehicle must be classified as exactly one of the two. State the specialization's participation and disjointness constraints.
6. Explain, with a small example, how a fan trap could arise if a Department has many Employees and many Projects, but Employees aren't actually linked to specific Projects directly.

## Normalization (Ch.14–15)
7. Given `Order(orderNo, customerNo, customerName, productNo, productName, quantity)` where orderNo+productNo together form the key (an order can include multiple products), identify the partial dependencies and normalize to 2NF.
8. Continuing from Q7, if `customerName` depends only on `customerNo` (not on the full order), identify this as a further normalization issue and resolve to 3NF.
9. Explain why a relation could satisfy 3NF's technical definition yet still be vulnerable to update anomalies — what does BCNF additionally check?

## Transaction Management (Ch.22)
10. Two transactions, T1 and T2, both read Account balance = 100. T1 computes balance+50=150 and writes it. T2 (using its own earlier read of 100) computes balance+30=130 and writes it AFTER T1. What's the final balance, what SHOULD it be, and which anomaly is this?
11. Explain why Strict 2PL is preferred over plain 2PL in most real DBMS implementations.

## Security (Ch.20)
12. A company experiences a hard-drive failure that destroys the primary database server's disk. Which specific countermeasure(s) from the chapter would have mitigated this, and why?

---

## ANSWER KEY

**1.** Database Planning → Requirements Collection → Database Design → Implementation → Testing.

**2.** Conceptual design only models entities, relationships, and attributes at a purely abstract level — it doesn't even commit to whether the eventual implementation will be relational, object-oriented, or something else. Logical design DOES commit to a specific data model TYPE (e.g. the relational model, applying normalization), even though it still doesn't commit to a specific DBMS PRODUCT (that's physical design's job).

**3.** Binary relationship (degree 2, between Student and Course). `enrollmentDate` belongs to the RELATIONSHIP itself (Enrolls In), not to either Student or Course individually, since it describes a property of that specific association.

**4.** Many-to-many (*:*) between Book and Member (considering borrowing history over time, not simultaneous borrowing). Participation is OPTIONAL on both sides: a Book might never be borrowed (optional from Book's side), and a Member might never borrow anything (optional from Member's side).

**5.** Mandatory (total) participation — every Vehicle must be classified as one of the subclasses — AND disjoint — a vehicle is either a Car or a Motorbike, not both.

**6.** If Department 1:* Employee and Department 1:* Project, but there's no direct Employee-to-Project relationship, the model might misleadingly suggest you can trace "which employees work on which projects" via the shared Department — but this would incorrectly imply every employee in a department is connected to every project in that department, which likely isn't true. The fix is adding a direct Employee-Project relationship (e.g. "Works On") if that real-world connection exists.

**7.** Partial dependencies: `customerNo`, `customerName` depend only on `orderNo` (not the full orderNo+productNo key); `productName` depends only on `productNo` (not the full key). 2NF fix: split into `Order(orderNo, customerNo, customerName)`, `Product(productNo, productName)`, and `OrderLine(orderNo, productNo, quantity)` (quantity IS fully dependent on the whole composite key, so it stays with the junction table).

**8.** `customerName` depending on `customerNo` (not directly on `orderNo`, the Order table's key) is a TRANSITIVE dependency once Order's key is just `orderNo`. 3NF fix: further split into `Order(orderNo, customerNo)` and `Customer(customerNo, customerName)`, with customerNo as a foreign key in Order.

**9.** This happens when a relation has MULTIPLE overlapping candidate keys, and a non-trivial functional dependency exists from an attribute (or part of a composite key) that is itself NOT a full candidate key — 3NF's definition technically only restricts dependencies of non-key attributes ON the primary key, missing this case. BCNF additionally requires that EVERY determinant in the relation (not just the primary key) must be a candidate key, closing this loophole.

**10.** Final balance = 130 (T2's write, since it happened after T1's and overwrote it). It SHOULD be 180 (100+50+30, both updates applied). This is the **lost update** problem — T1's +50 update is silently lost because T2 computed its update from the same stale read (100) rather than from T1's already-written 150.

**11.** Strict 2PL holds ALL exclusive locks until the transaction actually commits or aborts (not releasing them early during the shrinking phase). This specifically avoids **cascading rollbacks** — plain 2PL could release a write-lock before commit, letting another transaction read that (still uncommitted) data; if the first transaction then aborts, the second transaction (and anything that read from IT) would also need to be rolled back, potentially cascading through many transactions. Strict 2PL prevents this entire chain by never letting anyone see uncommitted writes.

**12.** **Backup and recovery**, and potentially **RAID** (specifically a redundant level like RAID 1 or RAID 5, NOT RAID 0). Backup and recovery would allow restoring the database to its last consistent state from a backup copy plus replaying the transaction log; RAID (with actual redundancy) could have prevented data loss entirely by surviving a single disk's failure through mirroring or parity, without even needing to fall back to a backup.
