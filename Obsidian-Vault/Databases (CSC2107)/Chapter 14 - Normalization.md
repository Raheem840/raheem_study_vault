---
chapter: 14
title: Normalization
source: "Connolly & Begg, Database Systems, 6th Ed, Ch.14"
course: "[[00 - Course Map|CSC 2107 - Databases]]"
tags: [databases, csc2107, exam-material, normalization]
---

# Ch.14 — Normalization

## 1. Why this matters
Normalization is THE single most heavily examined topic in most DB courses. You WILL be asked to normalize a given unnormalized table to 1NF → 2NF → 3NF, showing your working at each step, AND to identify functional dependencies from a scenario.

---

## 2. Why normalize? — Data redundancy and update anomalies (the MOTIVATION, always asked first)
A poorly structured (unnormalized) table mixing unrelated facts causes 3 types of **update anomalies**:
- **Insertion anomaly**: can't add certain data without also having OTHER, unrelated data available (e.g. can't record a new course exists until at least one student has enrolled in it, if course and enrollment info are jammed into one table).
- **Deletion anomaly**: deleting one fact accidentally destroys OTHER, unrelated information (e.g. deleting the last student enrolled in a course accidentally deletes all record that the course exists).
- **Modification (update) anomaly**: a fact stored redundantly in multiple rows must be updated in EVERY row consistently, or the data becomes contradictory (e.g. a course's title, if stored once per enrollment row, might get updated in some rows and not others).

Normalization systematically eliminates these by ensuring each table represents ONE clear, well-defined theme/entity.

---

## 3. Functional Dependencies (FDs) — the mathematical foundation of normalization
`A → B` ("A functionally determines B") means: for any two tuples, if they agree on their A value, they MUST also agree on their B value — knowing A's value uniquely determines B's value.

**Determinant**: the attribute(s) on the LEFT side of a functional dependency (A, in `A → B`).

### Characteristics of FDs (testable)
- **Full functional dependency**: B is fully dependent on the WHOLE of a composite key A — removing ANY attribute from A would mean A no longer determines B.
- **Partial dependency**: B depends on only PART of a composite key A (not the whole thing) — this is exactly what 2NF eliminates.
- **Transitive dependency**: A → B and B → C, therefore A → C indirectly, THROUGH B (not directly) — this is exactly what 3NF eliminates.

**Identifying FDs and the primary key**: given a scenario/table, you identify which attributes determine which others by reasoning about the REAL-WORLD meaning of the data (e.g. "does staffNo determine staffName? Yes — each staff number belongs to exactly one name"), then the primary key is the minimal set of attribute(s) that functionally determines ALL other attributes in the table.

---

## 4. The normalization process — 1NF → 2NF → 3NF (memorize the definitions AND the "what you actually do" at each step)

### First Normal Form (1NF)
**Definition**: a relation in which the intersection of every row and column contains ONE, and only one, atomic value — no repeating groups, no multi-valued attributes.
**What you do**: if a table has a repeating group (e.g. a client with MULTIPLE property viewings crammed into one row with repeated columns like viewDate1, viewDate2...), you "flatten" it by removing the repeating group into a SEPARATE table with a composite key (the original key + the repeating attribute's identifier).

### Second Normal Form (2NF)
**Definition**: a relation that is in 1NF AND every non-primary-key attribute is FULLY functionally dependent on the ENTIRE primary key (no partial dependencies).
**What you do**: applies only when the primary key is COMPOSITE (multiple attributes). Identify attributes that depend on only PART of the key, and move them into a separate table keyed by just that part.
> **Exam trap**: 2NF is automatically satisfied if the primary key is a SINGLE attribute (there's no "part" of a single-attribute key to partially depend on) — a relation with a simple (non-composite) key that's already in 1NF is automatically in 2NF too.

### Third Normal Form (3NF)
**Definition**: a relation that is in 2NF AND no non-primary-key attribute is TRANSITIVELY dependent on the primary key.
**What you do**: identify chains like PK → X → Y (where a non-key attribute X determines another non-key attribute Y), and move X and Y into their own separate table, keyed by X, with a foreign key back to the original table.

### Worked example (the classic style of exam question)
Given: `StaffBranch(staffNo, staffName, branchNo, branchAddress)`, where staffNo → staffName, staffNo → branchNo, and branchNo → branchAddress.
- This is in 1NF and 2NF already (single-attribute key staffNo, so no partial dependency issue).
- But it has a TRANSITIVE dependency: staffNo → branchNo → branchAddress (branchAddress depends on staffNo only THROUGH branchNo, not directly).
- **3NF fix**: split into `Staff(staffNo, staffName, branchNo)` and `Branch(branchNo, branchAddress)`, with branchNo as a foreign key in Staff referencing Branch.

---

## 5. General (formal) definitions of 2NF and 3NF
- **2NF**: a relation is in 2NF if it's in 1NF and every non-primary-key attribute is fully functionally dependent on the primary key (not just part of it).
- **3NF**: a relation is in 3NF if it's in 1NF and 2NF, and no non-primary-key attribute is transitively dependent on the primary key.

> **Exam trap**: both 2NF and 3NF definitions explicitly build on the PREVIOUS normal form ("in 1NF and..." / "in 1NF and 2NF and...") — you cannot claim a table is in 3NF without it also satisfying 1NF and 2NF; these are CUMULATIVE, not independent checks.

---

## 6. Quick self-test
1. Define the three update anomalies with a one-sentence example each.
2. Define functional dependency, and what a "determinant" is.
3. Distinguish full, partial, and transitive functional dependency.
4. State the formal definition of 1NF.
5. State the formal definition of 2NF, and explain why a table with a single-attribute key automatically satisfies it.
6. State the formal definition of 3NF, and identify the transitive dependency in StaffBranch(staffNo, staffName, branchNo, branchAddress) — show the resulting 3NF decomposition.
7. Why are 2NF and 3NF cumulative rather than independent checks?
