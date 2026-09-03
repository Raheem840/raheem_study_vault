---
chapter: 7
title: "SQL: Data Definition"
source: "Connolly & Begg, Database Systems, 6th Ed, Ch.7"
course: "[[00 - Course Map|CSC 2107 - Databases]]"
tags: [databases, csc2107, exam-material, sql]
---

# Ch.7 — SQL: Data Definition

## 1. Why this matters
This chapter is where the relational model's abstract integrity rules (Ch.4) and normalization's output (Ch.14–15) turn into actual, runnable `CREATE TABLE` statements — including how PRIMARY KEY, FOREIGN KEY, NOT NULL, and CHECK constraints are declared in real SQL syntax.

---

## 2. Creating a table (CREATE TABLE) — the most-tested SQL statement in this chapter
```sql
CREATE TABLE Staff (
    staffNo    VARCHAR(5)     NOT NULL,
    fName      VARCHAR(15)    NOT NULL,
    lName      VARCHAR(15)    NOT NULL,
    salary     DECIMAL(7,2)   NOT NULL CHECK (salary > 0),
    branchNo   VARCHAR(4)     NOT NULL,
    PRIMARY KEY (staffNo),
    FOREIGN KEY (branchNo) REFERENCES Branch(branchNo)
        ON DELETE CASCADE
        ON UPDATE CASCADE
);
```
### Key clauses to know precisely
- **PRIMARY KEY** — implicitly enforces NOT NULL and uniqueness (entity integrity from Ch.4, enforced automatically).
- **FOREIGN KEY ... REFERENCES** — enforces referential integrity (Ch.4); the referenced column must be a primary/candidate key in the target table.
- **NOT NULL** — a column-level constraint disallowing nulls.
- **CHECK (condition)** — enforces a general/business-rule constraint (Ch.4's "general constraints").
- **UNIQUE** — enforces uniqueness on a non-primary-key column (an alternate key from Ch.4).

### Referential actions — ON DELETE / ON UPDATE (guaranteed exam list)
| Action | Effect when the referenced row is deleted/updated |
|---|---|
| **CASCADE** | Automatically delete/update the matching rows in the referencing (child) table too |
| **SET NULL** | Set the foreign key column(s) in referencing rows to NULL |
| **SET DEFAULT** | Set the foreign key to its defined default value |
| **NO ACTION / RESTRICT** | Reject the delete/update if matching referencing rows exist (the default, safest option) |

> **Exam trap**: `ON DELETE CASCADE` is dangerous if misapplied — it can trigger a chain of deletions across multiple tables; exams often ask you to justify WHICH referential action fits a given business scenario (e.g. deleting a Branch should probably NOT cascade-delete all its Staff — RESTRICT is usually more appropriate there).

---

## 3. ISO SQL scalar data types (know the major categories, not every exact type)
| Category | Examples |
|---|---|
| Character | CHAR(n) — fixed length, VARCHAR(n) — variable length |
| Numeric (exact) | INTEGER, SMALLINT, DECIMAL(p,s), NUMERIC(p,s) |
| Numeric (approximate) | FLOAT, REAL, DOUBLE PRECISION |
| Date/time | DATE, TIME, TIMESTAMP |
| Boolean | BOOLEAN (true/false/unknown) |

> **Exam trap**: CHAR(n) always pads with trailing spaces to exactly n characters; VARCHAR(n) stores only the actual characters used, up to a maximum of n — this affects both storage and sometimes comparison behaviour, and is a common short-answer question.

---

## 4. Altering and removing tables
```sql
ALTER TABLE Staff ADD COLUMN dateOfBirth DATE;
ALTER TABLE Staff DROP COLUMN dateOfBirth;
DROP TABLE Staff;                    -- removes the table AND its data entirely
```
> **Exam trap**: `DROP TABLE` typically fails if other tables have foreign keys referencing it (unless those are dropped/altered first) — this connects directly back to referential integrity.

---

## 5. Indexes
```sql
CREATE INDEX idx_lname ON Staff(lName);
DROP INDEX idx_lname;
```
An index speeds up retrieval (and sorting) on the indexed column(s), at the cost of extra storage and slightly slower writes (every INSERT/UPDATE/DELETE must also update the index) — a classic "why not just index everything" exam reasoning question. Primary keys are usually automatically indexed by the DBMS.

---

## 6. Views (DDL side — creating them; conceptual purpose covered in [[Chapter 04 - The Relational Model|Ch.4]])
```sql
CREATE VIEW LondonStaff AS
    SELECT staffNo, fName, lName, salary
    FROM Staff s, Branch b
    WHERE s.branchNo = b.branchNo AND b.city = 'London';

DROP VIEW LondonStaff;
```
**WITH CHECK OPTION**: when updating through a view, ensures the update doesn't produce a row that would then fall OUTSIDE the view's own WHERE condition (e.g. can't use the view above to change a staff member's branch to one outside London, since the resulting row wouldn't satisfy the view's own filter anymore).

### Advantages of views (guaranteed list)
Data security (hide sensitive columns/rows), simplifying complex queries for end-users, logical data independence, allowing the same data to be seen differently by different users, structural/referential integrity when combined appropriately.

### Disadvantages
Update restrictions (not all views are updatable — see Ch.4), potential performance overhead (the underlying query re-runs each time the view is accessed, unless materialized), and structural limitations imposed by the DBMS.

---

## 7. Transactions and constraint timing (brief, expanded fully in the Transaction Management chapter)
- **Immediate constraint checking**: violations are caught as soon as the statement executes.
- **Deferred constraint checking**: some constraints (especially certain foreign keys involved in circular references) can be checked only at COMMIT time, allowing temporarily "inconsistent" intermediate states within a single transaction.

---

## 8. Discretionary access control — GRANT and REVOKE
```sql
GRANT SELECT, INSERT ON Staff TO userX;
GRANT ALL PRIVILEGES ON Staff TO userY WITH GRANT OPTION;
REVOKE INSERT ON Staff FROM userX;
```
`WITH GRANT OPTION` lets the recipient user pass those same privileges on to OTHER users — without it, a user can use the privilege but not grant it onward. This connects directly to [[00 - Course Map|the Security & Administration chapter]] later in the course.

---

## 9. Quick self-test
1. Write a CREATE TABLE statement for a simple two-column table with a primary key and a NOT NULL constraint.
2. List the 4 referential actions for ON DELETE, and explain when CASCADE would be inappropriate.
3. Distinguish CHAR(n) from VARCHAR(n).
4. Why does DROP TABLE sometimes fail, and what's the underlying reason (connect to Ch.4)?
5. What's the tradeoff of creating an index on a column?
6. What does WITH CHECK OPTION do when updating through a view?
7. What does WITH GRANT OPTION allow a user to do, that they couldn't do without it?
