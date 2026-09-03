---
chapter: 6
title: "SQL: Data Manipulation"
source: "Connolly & Begg, Database Systems, 6th Ed, Ch.6"
course: "[[00 - Course Map|CSC 2107 - Databases]]"
tags: [databases, csc2107, exam-material, sql]
---

# Ch.6 — SQL: Data Manipulation

## 1. Why this matters
This is the practical, hands-on core of the whole course — most practicals/labs and a large chunk of the final exam will be "write a SQL query that does X." Precision matters: exam markers deduct for wrong clause order, wrong join syntax, or forgetting DISTINCT/GROUP BY rules.

---

## 2. Basic query structure (the "shape" every SELECT follows)
```sql
SELECT [DISTINCT] column_list
FROM table_list
[WHERE condition]
[GROUP BY column_list]
[HAVING group_condition]
[ORDER BY column_list [ASC|DESC]];
```
**Logical order of evaluation** (NOT the same as the written order — a very common exam trap): FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY. This is WHY you can't reference a SELECT-list alias in the WHERE clause, but you sometimes can in ORDER BY — WHERE is evaluated before SELECT computes its aliases, but ORDER BY happens after.

---

## 3. Simple queries
```sql
SELECT staffNo, fName, lName FROM Staff;             -- specific columns
SELECT * FROM Staff;                                  -- all columns
SELECT DISTINCT city FROM Branch;                     -- removes duplicate rows
SELECT * FROM Staff WHERE salary > 30000;              -- comparison
SELECT * FROM Staff WHERE salary BETWEEN 20000 AND 40000;
SELECT * FROM Staff WHERE branchNo IN ('B003','B005');
SELECT * FROM Staff WHERE lName LIKE 'S%';             -- pattern match: starts with S
SELECT * FROM Staff WHERE lName IS NULL;               -- NOT "= NULL" — this is a common trap
```
> **Exam trap**: you can NEVER write `WHERE column = NULL` — null isn't a value to compare with `=`; you must use `IS NULL` / `IS NOT NULL`.

---

## 4. Sorting — ORDER BY
```sql
SELECT * FROM Staff ORDER BY salary DESC, lName ASC;
```
Multiple sort keys are applied in the order listed (primary sort key first, ties broken by the next).

---

## 5. Aggregate functions
`COUNT`, `SUM`, `AVG`, `MIN`, `MAX` — operate on a whole column (or group, with GROUP BY) and return a SINGLE value.
```sql
SELECT COUNT(*) FROM Staff;                 -- counts ALL rows, including nulls
SELECT COUNT(salary) FROM Staff;            -- counts only NON-NULL salary values
SELECT AVG(salary) FROM Staff WHERE branchNo='B003';
```
> **Exam trap**: `COUNT(*)` counts rows regardless of nulls; `COUNT(column)` skips rows where that specific column is null — these can give DIFFERENT answers on the same table, and exams test this deliberately.

---

## 6. Grouping — GROUP BY and HAVING
```sql
SELECT branchNo, COUNT(staffNo) AS numStaff
FROM Staff
GROUP BY branchNo
HAVING COUNT(staffNo) > 5;
```
**Golden rule (guaranteed exam trap)**: every column in the SELECT list that is NOT inside an aggregate function MUST appear in the GROUP BY clause — otherwise the query is ambiguous (which row's value would it show for a group with multiple rows?).

`WHERE` filters INDIVIDUAL rows BEFORE grouping; `HAVING` filters GROUPS AFTER aggregation — you cannot use an aggregate function in WHERE (it doesn't exist yet at that stage of evaluation), and this exact distinction is a favorite exam question.

---

## 7. Subqueries
A query nested inside another query, in the WHERE, FROM, or SELECT clause.
```sql
SELECT fName, lName FROM Staff
WHERE salary > (SELECT AVG(salary) FROM Staff);
```
- **Scalar subquery**: returns exactly one value — usable directly with `=`, `>`, etc.
- **Multi-row subquery**: returns multiple values — must be used with `IN`, `ANY`, `ALL`, or `EXISTS`, not a plain `=`/`>`.

### ANY and ALL
```sql
WHERE salary > ANY (SELECT salary FROM Staff WHERE branchNo='B003')  -- greater than at least one
WHERE salary > ALL (SELECT salary FROM Staff WHERE branchNo='B003')  -- greater than every one
```
> **Exam trap**: `> ANY` means "greater than the SMALLEST value in the set" (easiest to satisfy); `> ALL` means "greater than the LARGEST value" (hardest to satisfy) — students frequently get the intuition backwards.

---

## 8. Multi-table queries — JOINs
```sql
-- Implicit join (older style, still commonly tested)
SELECT s.fName, s.lName, b.city
FROM Staff s, Branch b
WHERE s.branchNo = b.branchNo;

-- Explicit JOIN syntax (preferred modern style)
SELECT s.fName, s.lName, b.city
FROM Staff s INNER JOIN Branch b ON s.branchNo = b.branchNo;

-- Outer joins
SELECT s.fName, p.propertyNo
FROM Staff s LEFT OUTER JOIN PropertyForRent p ON s.staffNo = p.staffNo;
```
**Inner join** returns only rows with a match on BOTH sides. **Left outer join** keeps every row from the left table, filling nulls for unmatched right-side columns (ties directly to [[Chapter 05 - Relational Algebra and Calculus|Ch.5's]] algebra outer-join notation).

---

## 9. EXISTS and NOT EXISTS
```sql
SELECT b.* FROM Branch b
WHERE EXISTS (SELECT * FROM Staff s WHERE s.branchNo = b.branchNo);
```
Tests whether the subquery returns ANY rows at all (true/false), rather than comparing actual values — often more efficient than `IN` for large subqueries, and required for certain "for all" style queries (e.g. "find branches with NO staff" uses `NOT EXISTS`).

---

## 10. Combining result tables — UNION, INTERSECT, EXCEPT
```sql
SELECT city FROM Branch
UNION
SELECT city FROM Client;
```
Same union-compatibility rule from [[Chapter 05 - Relational Algebra and Calculus|Ch.5]] applies: both SELECT lists need the same number of columns with compatible types. `UNION` removes duplicates by default (`UNION ALL` keeps them); `INTERSECT` returns rows in both; `EXCEPT` (or `MINUS` in Oracle) returns rows in the first but not the second.

---

## 11. Database updates — INSERT, UPDATE, DELETE
```sql
INSERT INTO Staff (staffNo, fName, lName, salary) VALUES ('SG16','Alan','Brown',25000);

UPDATE Staff SET salary = salary * 1.05 WHERE branchNo = 'B003';

DELETE FROM Staff WHERE staffNo = 'SG16';
```
> **Exam trap**: `UPDATE`/`DELETE` WITHOUT a `WHERE` clause affects EVERY row in the table — a classic "spot the bug" question is a query missing its WHERE clause.

---

## 12. Quick self-test
1. Write out the logical order of evaluation of a SELECT statement's clauses (not the written order).
2. Why can't you write `WHERE column = NULL`? What's the correct syntax?
3. Explain the difference between `COUNT(*)` and `COUNT(column)` with an example where they'd differ.
4. State the "golden rule" for what must appear in GROUP BY.
5. Why can't you use an aggregate function in a WHERE clause, but you can in HAVING?
6. Explain the difference between `> ANY` and `> ALL` in a subquery comparison.
7. Distinguish INNER JOIN from LEFT OUTER JOIN in terms of which rows appear in the result.
8. What happens if you run an UPDATE or DELETE statement without a WHERE clause?
