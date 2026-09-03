---
chapter: 4
title: The Relational Model
source: "Connolly & Begg, Database Systems, 6th Ed, Ch.4"
course: "[[00 - Course Map|CSC 2107 - Databases]]"
tags: [databases, csc2107, exam-material, relational-model]
---

# Ch.4 — The Relational Model

## 1. Why this matters
This is the formal vocabulary underneath every SQL statement you'll write in Ch.6/7 — **relation, tuple, attribute, domain, key, integrity constraint**. Exams love precise definitions here, and precisely testing "primary key vs candidate key vs foreign key" distinctions.

---

## 2. Core terminology (memorize the formal-term ↔ everyday-term mapping)
| Formal term | Everyday term |
|---|---|
| Relation | Table |
| Tuple | Row / record |
| Attribute | Column / field |
| Domain | The set of allowable values for an attribute |
| Degree | Number of attributes (columns) in a relation |
| Cardinality | Number of tuples (rows) in a relation |

**Relational database**: a collection of normalized relations with distinct relation names.

---

## 3. Properties of a relation (testable list — a "table" isn't automatically a valid relation unless it satisfies these)
1. Each relation has a unique name.
2. Each cell contains exactly one (atomic/indivisible) value — no repeating groups or multi-valued cells (this is literally First Normal Form, previewed here and formalized in [[Chapter 14 - Normalization|Ch.14]]).
3. Each attribute has a distinct name (within that relation).
4. The VALUES of an attribute are all drawn from the same domain.
5. The order of attributes has no significance (a relation is a SET of attributes, not a sequence).
6. Each tuple is distinct — no duplicate rows.
7. The order of tuples has no significance (a relation is a SET of tuples).

> **Exam trap**: property 6 (no duplicate tuples) is why every relation MUST have a key — without a unique identifier, you couldn't guarantee uniqueness of rows in principle.

---

## 4. Relational keys — THE most-tested part of this chapter
| Key type | Definition |
|---|---|
| **Superkey** | A set of one or more attributes that, taken together, uniquely identifies a tuple |
| **Candidate key** | A superkey with NO redundant attributes — a MINIMAL superkey (remove any attribute and it stops being unique) |
| **Primary key** | The candidate key CHOSEN to be the main identifier for the relation (there may be several candidate keys; exactly one is picked as primary) |
| **Alternate key** | Any candidate key NOT chosen as the primary key |
| **Composite key** | A candidate/primary key made of MORE than one attribute |
| **Foreign key** | An attribute (or set of attributes) in one relation that matches the CANDIDATE key of some relation (possibly the same relation, for a recursive relationship) — this is the mechanism that LINKS relations together |

> **Exam trap**: every candidate key is a superkey, but not every superkey is a candidate key (a superkey can have redundant attributes; a candidate key cannot — it must be minimal). This minimality distinction is asked constantly.

---

## 5. Integrity constraints (guaranteed exam definitions)
- **Nulls**: represent a value that is currently unknown or not applicable — NOT the same as zero or blank space; null is a special marker meaning "no value recorded."
- **Entity integrity**: in a base relation, no attribute of the PRIMARY KEY may be null. (If part of the primary key could be null, you couldn't guarantee unique identification — defeats the whole purpose of a primary key.)
- **Referential integrity**: if a foreign key exists, its value must either match a candidate key value in the referenced relation, OR be entirely null. (You can't reference a row that doesn't exist.)
- **General constraints**: additional business rules specific to the application (e.g. "salary must be positive," "an employee's manager must be in the same department") — enforced via CHECK constraints, triggers, etc.

> **Exam trap**: entity integrity applies ONLY to the primary key (not to every attribute) — a non-key attribute CAN be null; referential integrity's "or null" clause is also frequently forgotten in exam answers (a foreign key can legitimately be null if the relationship is optional).

---

## 6. Views
A **view** is a "virtual" or "derived" relation — it doesn't store data itself, but is dynamically computed from one or more base relations via a stored query, and behaves like a relation from the user's perspective (it can typically be queried like a normal table).

**Purposes of views**: hide columns/rows a user shouldn't see (security), simplify complex queries for end-users, provide logical data independence (an external schema can be built from views even if the underlying conceptual schema changes somewhat).

**Updatability**: not all views can be updated — generally, a view is updatable only if the DBMS can unambiguously map the update back to the underlying base table(s) (e.g. views involving joins, aggregates, or GROUP BY are typically NOT updatable).

---

## 7. Quick self-test
1. Map these formal terms to everyday terms: relation, tuple, attribute, degree, cardinality.
2. List the 7 properties a valid relation must satisfy.
3. Define superkey, candidate key, primary key, and foreign key precisely, and explain the minimality distinction between superkey and candidate key.
4. State entity integrity and referential integrity precisely, including the "null" exception for each.
5. Why is null not the same as zero or blank?
6. What is a view, and why might a view NOT be updatable?
7. Name two purposes of using a view.
