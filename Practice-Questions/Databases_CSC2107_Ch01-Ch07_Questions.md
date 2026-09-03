# CSC 2107 — Practice Questions: Ch.1 Intro, Ch.2-3 Environment/Architecture, Ch.4 Relational Model, Ch.5 Algebra, Ch.6-7 SQL

Closed-book. Cover the answer key until you've committed to an answer.

## Intro & Environment (Ch.1–3)
1. A university currently keeps separate spreadsheets for admissions, fees, and library records for the same students. Identify which 2 of the 5 file-based limitations this scenario most clearly demonstrates, and explain why.
2. Explain, using the three ANSI-SPARC levels, what happens when a DBA adds a new column to a table that no existing application uses yet — which level(s) change, and which stay the same?
3. Justify, using at least 2 reasons, why a modern web application would use three-tier rather than two-tier architecture.

## Relational Model & Algebra (Ch.4–5)
4. Given a relation Employee(empNo, name, deptNo, ssn) where empNo and ssn are both unique identifiers on their own, identify all candidate keys, and explain why deptNo alone is not one.
5. Write the relational algebra expression to find the names of all employees in department 'D01', given Employee(empNo, name, deptNo).
6. A table Enrollment(studentID, courseID) records which students take which courses. Using relational algebra, describe (in words, referencing the division operation) how you'd find students enrolled in EVERY course offered by the Computer Science department.

## SQL (Ch.6–7)
7. Write a SQL query to find department numbers with more than 3 employees, using Employee(empNo, name, deptNo, salary).
8. Spot the bug: `UPDATE Employee SET salary = salary * 1.1;` — what does this do, and how would you fix it to give a raise only to department 'D01'?
9. Write a CREATE TABLE statement for a Department table (deptNo primary key, deptName not null) and an Employee table with a foreign key to Department that prevents deleting a department while employees still reference it.
10. Explain why `SELECT deptNo, name FROM Employee GROUP BY deptNo;` is invalid SQL, and how to fix it if you actually want one representative name per department.

---

## ANSWER KEY

**1.** Primarily **separation and isolation of data** (each department's data is siloed, hard to cross-reference a student across admissions/fees/library) and **duplication of data** (the student's name, ID, etc. are likely re-entered in all three systems, risking inconsistency).

**2.** Only the **conceptual level** (schema) and the **internal level** (physical storage, to accommodate the new column) change. Existing **external level** schemas (views used by current applications) are unaffected since they don't reference the new column — this is a direct demonstration of **logical data independence**: the conceptual schema changed without breaking existing external schemas/applications.

**3.** (1) Thinner clients / easier maintenance — business logic lives in the middle tier, so updating rules doesn't require redeploying every client (browser). (2) Better scalability — the middle (application) tier can be scaled/load-balanced independently of the database tier, which two-tier can't do as cleanly. (Also acceptable: improved security, since browsers never talk to the database directly.)

**4.** Candidate keys: **{empNo}** and **{ssn}** (each independently uniquely identifies a tuple). deptNo alone is NOT a candidate key because multiple employees share the same department — it doesn't uniquely identify a tuple.

**5.** `π name (σ deptNo='D01' (Employee))`

**6.** Enrollment ÷ (the set of course IDs offered by the Computer Science department) — this computes the student IDs associated with EVERY course in that division set, which is exactly the division operation's defining use case: "find X related to ALL of Y."

**7.**
```sql
SELECT deptNo, COUNT(*) AS numEmployees
FROM Employee
GROUP BY deptNo
HAVING COUNT(*) > 3;
```

**8.** As written, it gives EVERY employee in the entire table a 10% raise, since there's no WHERE clause. Fix:
```sql
UPDATE Employee SET salary = salary * 1.1 WHERE deptNo = 'D01';
```

**9.**
```sql
CREATE TABLE Department (
    deptNo    VARCHAR(4)  NOT NULL,
    deptName  VARCHAR(30) NOT NULL,
    PRIMARY KEY (deptNo)
);

CREATE TABLE Employee (
    empNo   VARCHAR(5)  NOT NULL,
    name    VARCHAR(30) NOT NULL,
    deptNo  VARCHAR(4)  NOT NULL,
    salary  DECIMAL(7,2),
    PRIMARY KEY (empNo),
    FOREIGN KEY (deptNo) REFERENCES Department(deptNo)
        ON DELETE NO ACTION
);
```
`ON DELETE NO ACTION` (or simply omitting an action, which defaults to restrict-like behaviour in most systems) ensures a Department row can't be deleted while Employee rows still reference it.

**10.** It's invalid because `name` is neither an aggregate function nor listed in the GROUP BY clause — per the golden rule, every non-aggregated SELECT column must appear in GROUP BY, since with multiple employees per department the DBMS wouldn't know WHICH employee's name to show for that group. Fix: either add `name` to `GROUP BY deptNo, name` (which effectively changes the grouping granularity to department+name combinations, likely not what's intended), or if you genuinely want one representative name per department, use an aggregate like `MIN(name)` or restructure the query's intent entirely (e.g. it may not be a meaningful query at all without deciding what "representative" means).
