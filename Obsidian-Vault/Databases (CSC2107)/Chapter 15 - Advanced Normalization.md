---
chapter: 15
title: Advanced Normalization
source: "Connolly & Begg, Database Systems, 6th Ed, Ch.15"
course: "[[00 - Course Map|CSC 2107 - Databases]]"
tags: [databases, csc2107, exam-material, normalization, bcnf]
---

# Ch.15 — Advanced Normalization

## 1. Why this matters
Builds directly on [[Chapter 14 - Normalization|Ch.14]] — 3NF has an edge case it doesn't catch, fixed by **BCNF**. Most undergrad exams stop at BCNF (4NF/5NF are usually mentioned only conceptually) — but know all three since "list the normal forms in order" is common.

---

## 2. Inference rules for functional dependencies (Armstrong's Axioms — brief, occasionally tested by name)
Given a set of FDs, these rules let you derive/prove ADDITIONAL FDs that must also logically hold:
- **Reflexivity**: if B is a subset of A, then A → B (trivial).
- **Augmentation**: if A → B, then AC → BC (adding the same attribute to both sides preserves the dependency).
- **Transitivity**: if A → B and B → C, then A → C (this is literally what 3NF checks for and eliminates).
Additional derived rules (union, decomposition, pseudotransitivity) follow from these three.

**Minimal set of FDs**: a set of functional dependencies with no redundant FDs and no redundant attributes on the left-hand side of any FD — used to simplify the FD set before doing normalization analysis.

---

## 3. Boyce-Codd Normal Form (BCNF) — stricter than 3NF
**Definition**: a relation is in BCNF if, for every functional dependency `A → B` in the relation, A is a **candidate key** (a "superkey," specifically minimal one) of the relation.

**Why BCNF exists / what 3NF misses**: 3NF's definition only restricts dependencies of NON-key attributes on the primary key — but it's possible for a relation to be in 3NF while STILL having anomalies, specifically when there are MULTIPLE overlapping candidate keys and a non-trivial FD exists FROM a non-key attribute (or part of a composite key) that isn't itself a full candidate key. BCNF closes this loophole by requiring EVERY determinant (not just the primary key) to be a candidate key.

> **Exam trap**: every relation in BCNF is automatically in 3NF, but NOT every 3NF relation is in BCNF — BCNF is strictly stronger. The classic textbook example involves a relation with overlapping composite candidate keys (e.g. a student-tutoring scenario where {student, subject} and {student, tutor} are both candidate keys, but tutor → subject also holds) — this satisfies 3NF's technical definition but still has update anomalies, which BCNF's stricter test catches.

### Practical approach to converting 3NF to BCNF
Find a determinant that ISN'T a candidate key, and split the relation into two: one relation with just that determinant and what it determines, and another with the determinant (as a foreign key) plus the remaining attributes.

---

## 4. Review of normalization up to BCNF (the whole progression — memorize this table)
| Normal Form | Eliminates |
|---|---|
| 1NF | Repeating groups / non-atomic values |
| 2NF | Partial dependencies (non-key attribute depends on only PART of a composite key) |
| 3NF | Transitive dependencies (non-key attribute depends on ANOTHER non-key attribute) |
| BCNF | Any remaining anomaly from a determinant that isn't a candidate key (closes 3NF's loophole) |

---

## 5. Fourth Normal Form (4NF) — deals with multi-valued dependencies
**Multi-valued dependency (MVD)**: written `A →→ B`, means for a given value of A, there is a SET of values of B, INDEPENDENT of any other attribute C in the relation. This happens when a relation tries to represent TWO independent, unrelated multi-valued facts about the same entity in one table (e.g. an Employee's Skills and their Children, both multi-valued but completely unrelated to each other, jammed into one table causes spurious combinations).

**Definition of 4NF**: a relation is in 4NF if it's in BCNF, and contains no non-trivial multi-valued dependencies (other than dependencies where the determinant is a candidate key of the WHOLE relation).
**Fix**: split the two independent multi-valued facts into two SEPARATE relations.

---

## 6. Fifth Normal Form (5NF) — deals with join dependencies
**Lossless-join dependency**: a relation can be split into multiple smaller relations such that JOINING them back together perfectly reconstructs the original relation, with NO spurious extra rows and no lost information.
**Definition of 5NF**: a relation is in 5NF (also called **Project-Join Normal Form**) if every join dependency in it is implied by its candidate keys — essentially, the relation cannot be decomposed any further into smaller relations without losing information or gaining spurious join results. This is the theoretical "final" normal form, rarely reached deliberately in ordinary practical design (mostly a theoretical ceiling).

---

## 7. Quick self-test
1. State Armstrong's three basic inference rules (reflexivity, augmentation, transitivity).
2. Define BCNF, and explain precisely what it requires beyond 3NF.
3. Why is every BCNF relation automatically in 3NF, but not vice versa?
4. Describe the practical approach to converting a 3NF relation that violates BCNF into BCNF.
5. Complete the table: what does each of 1NF, 2NF, 3NF, and BCNF specifically eliminate?
6. Define a multi-valued dependency, and describe the classic scenario 4NF fixes.
7. What does 5NF's "lossless-join dependency" concept require?
