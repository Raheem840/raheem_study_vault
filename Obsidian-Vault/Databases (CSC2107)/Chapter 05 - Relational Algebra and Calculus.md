---
chapter: 5
title: Relational Algebra and Relational Calculus
source: "Connolly & Begg, Database Systems, 6th Ed, Ch.5"
course: "[[00 - Course Map|CSC 2107 - Databases]]"
tags: [databases, csc2107, exam-material, relational-algebra]
---

# Ch.5 — Relational Algebra and Relational Calculus

## 1. Why this matters
Relational algebra is the FORMAL, mathematical version of what SQL does informally — exams test it because it forces you to understand exactly what each SQL clause is doing underneath. Classic asks: **"express this query in relational algebra"**, **"what does this algebra expression compute, in English"**, **"distinguish an inner join from an outer join using algebra notation."**

---

## 2. Relational Algebra — unary operations (operate on ONE relation)
| Operation | Symbol | What it does | SQL equivalent |
|---|---|---|---|
| **Selection** | σ (sigma) | Selects ROWS matching a condition | WHERE clause |
| **Projection** | π (pi) | Selects COLUMNS (and removes duplicate resulting rows) | SELECT column list |

```
σ salary>30000 (Staff)        -- rows where salary > 30000
π fName, lName (Staff)        -- just the fName and lName columns
```
> **Exam trap**: Selection picks ROWS (like WHERE); Projection picks COLUMNS (like the SELECT list) — students very commonly swap these two names.

---

## 3. Relational Algebra — set operations (operate on TWO relations; both must be **union-compatible** — same number of attributes, matching domains)
| Operation | Symbol | Meaning |
|---|---|---|
| Union | ∪ | Rows in EITHER relation (duplicates removed) |
| Intersection | ∩ | Rows in BOTH relations |
| Difference | − | Rows in the first relation but NOT the second |
| Cartesian product | × | Every row of R combined with every row of S (all combinations) — degree adds, cardinality multiplies |

> **Exam trap**: union-compatibility is REQUIRED for union/intersection/difference (same degree, matching domains attribute-by-attribute) — you cannot union two relations with different columns; Cartesian product has NO such restriction, since it just pairs up rows regardless of matching structure.

---

## 4. Join operations — THE most tested section (know all of these precisely)
| Join type | Symbol/name | Meaning |
|---|---|---|
| **Theta join** | R ⋈θ S | General join: combine rows from R and S where a given condition θ holds (θ can be any comparison, not just equality) |
| **Equijoin** | | A theta join where the condition is specifically equality (=) |
| **Natural join** | R ⋈ S | An equijoin on ALL common attribute names, with the duplicate common column removed from the result (the "clean" join most people mean by default) |
| **Outer join** | ⟕ ⟖ ⟗ | Like a natural/equijoin, but KEEPS unmatched rows from one or both sides, filling missing values with null |
| **Semijoin** | ⋉ | Returns only the rows (and columns) of R that WOULD have matched in a join with S — but only R's attributes appear in the result |

### Outer join variants (guaranteed exam distinction)
- **Left outer join**: keeps ALL rows from the LEFT relation, matched rows from the right (nulls where no match).
- **Right outer join**: keeps ALL rows from the RIGHT relation, matched rows from the left.
- **Full outer join**: keeps ALL rows from BOTH relations, nulls filled in wherever there's no match on either side.

**Natural join is just a Cartesian product + selection + projection, done as one combined operation** — this connects the join operators back to the basic operations, and is a classic "derive the join from basic operations" exam question:
```
R ⋈ S  ≡  π (all attributes, common column once) (σ (R.commonAttr = S.commonAttr) (R × S))
```

---

## 5. Division operation
`R ÷ S` returns tuples from R's remaining attributes that are associated with EVERY tuple in S — the classic use case is "find all X that relate to ALL Y" queries (e.g. "find students who have taken EVERY course in the Computer Science department").

---

## 6. Aggregation and grouping
Relational algebra extensions for SUM, COUNT, AVG, MAX, MIN and GROUP BY-style grouping — these mirror SQL's aggregate functions and GROUP BY clause directly (covered concretely in [[Chapter 06 - SQL Data Manipulation|Ch.6]]).

---

## 7. Relational Calculus — the DECLARATIVE alternative to algebra
Where algebra specifies HOW to get the answer (step-by-step operations), calculus specifies WHAT you want (a formal logic-based description) — this connects directly to Ch.2's procedural-vs-non-procedural DML distinction. Relational algebra and relational calculus are **provably equivalent in expressive power** (Codd's theorem) — anything expressible in one is expressible in the other, they're just different notations/philosophies.

- **Tuple relational calculus**: variables range over TUPLES. `{t | Staff(t) ∧ t.salary > 30000}` — "the set of all tuples t such that t is in Staff and t's salary exceeds 30000."
- **Domain relational calculus**: variables range over individual DOMAIN VALUES (attribute values) rather than whole tuples — more fine-grained, closer to how Query-By-Example (QBE) interfaces work.

---

## 8. Quick self-test
1. Distinguish Selection (σ) from Projection (π) — which picks rows, which picks columns?
2. What does "union-compatible" mean, and which operations require it?
3. Define natural join, and explain how it's derived from Cartesian product + selection + projection.
4. Distinguish left outer join, right outer join, and full outer join.
5. What is a semijoin, and how does its output differ from a regular join's output?
6. Describe a scenario where the division operation is the natural fit, and explain why.
7. What does Codd's theorem say about the relationship between relational algebra and relational calculus?
