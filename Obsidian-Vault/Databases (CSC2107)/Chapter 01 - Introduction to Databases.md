---
chapter: 1
title: Introduction to Databases
source: "Connolly & Begg, Database Systems, 6th Ed, Ch.1"
course: "[[00 - Course Map|CSC 2107 - Databases]]"
tags: [databases, csc2107, exam-material]
---

# Ch.1 — Introduction to Databases

## 1. Why this matters
Sets up the vocabulary (database, DBMS, schema) and the file-based-vs-database contrast that almost every exam opens with. Classic asks: **"list the limitations of file-based systems that databases solve"**, **"define database vs DBMS vs application program"**, **"list the roles in a database environment."**

---

## 2. The file-based approach and why it failed at scale
Before databases, each application had its OWN separate data files (e.g. Payroll dept has its own employee file, Personnel dept has ANOTHER separate employee file).

### Limitations of file-based systems (guaranteed exam list — memorize all 5)
1. **Separation and isolation of data** — each program manages its own files; data spread across the organization isn't easily combined/cross-referenced.
2. **Duplication of data** — the same data (e.g. an employee's address) is stored redundantly across multiple files, wasting space and risking inconsistency.
3. **Data dependence** — the physical file structure/format is baked into the application program's code; changing the file format requires changing the program.
4. **Incompatible file formats** — files created by different programs/languages often can't be directly processed together.
5. **Fixed queries / proliferation of application programs** — file-based systems are designed around a fixed set of pre-planned queries/reports; any NEW query needs a new program written from scratch.

---

## 3. The database approach — what it fixes
A **database** is a shared collection of logically related data (and its description), designed to meet the information needs of an organization.
A **DBMS (Database Management System)** is software that enables users to define, create, maintain, and control access to the database.
The **database** + **DBMS** + associated **application programs** together form the **database system**.

> **Exam trap**: "database" and "DBMS" are NOT the same thing — the database is the DATA itself (plus its description/metadata); the DBMS is the SOFTWARE that manages it. Students frequently conflate these.

### Components of the DBMS environment (5 components — testable)
1. **Hardware** — the computers/storage the DBMS and applications run on
2. **Software** — the DBMS itself, operating system, application programs, network software
3. **Data** — the actual data AND the metadata (data about the data — the schema)
4. **Procedures** — instructions/rules for using/running the database system
5. **People** — everyone involved (see roles below)

---

## 4. Roles in the database environment (guaranteed exam list)
| Role | Responsibility |
|---|---|
| **Data Administrator (DA)** | Manages the data RESOURCE itself — policy-level decisions, standards |
| **Database Administrator (DBA)** | Manages the physical realization of the database — technical implementation, security, performance, backups |
| **Database Designers** | Design the database structure (further split into **logical** designers, who model entities/relationships/attributes independent of any specific DBMS, and **physical** designers, who map the logical design onto a specific DBMS's storage structures) |
| **Application Developers** | Write the programs that interact with the database via the DBMS |
| **End-Users** | Use the finished application to query/update data — split into **naive users** (use pre-written menu/form-based apps) and **sophisticated users** (write their own ad-hoc queries) |

---

## 5. Brief history (know the rough eras, not exact years)
1960s: hierarchical and network database models emerge (early, rigid, navigational access). 1970: **E.F. Codd** publishes the seminal paper proposing the **relational model** — data as simple tables with a strong mathematical (set theory) foundation. 1980s onward: relational DBMSs (RDBMS) become commercially dominant (Oracle, DB2, SQL Server, MySQL). Later: object-oriented, NoSQL, and NewSQL systems emerge for specialized needs (though the relational model remains the dominant default even today).

---

## 6. Advantages and disadvantages of DBMSs (both sides are testable — don't only memorize advantages)

### Advantages (partial list, most-cited)
- **Control of data redundancy** — data stored once, referenced everywhere (not eliminated entirely, but minimized and controlled)
- **Data consistency** — reducing redundancy reduces the risk of inconsistent copies
- **Improved data sharing / integrity** — centralized constraints enforce validity rules
- **Improved security** — centralized access control
- **Improved data accessibility** — via query languages like SQL, without writing custom programs
- **Economy of scale** — combining resources/data for all departments into one system can reduce overall cost
- **Improved backup/recovery** — centralized, managed by the DBMS

### Disadvantages
- **Complexity** — DBMSs are complex software, requiring skilled staff
- **Cost** — of the DBMS itself, hardware, and specialist staff
- **Performance overhead** — the DBMS's generality can add overhead vs. a hand-tuned file system for one specific task
- **Higher impact of failure** — centralization means a DBMS failure can affect the whole organization at once (single point of failure risk)

> **Exam trap**: "control of redundancy" ≠ "elimination of redundancy" — the database approach controls/minimizes redundancy for good reasons (e.g. performance via denormalization later in the course), it does not claim to eliminate it entirely.

---

## 7. Quick self-test
1. List the 5 limitations of file-based systems.
2. Define "database" and "DBMS," and explain why they are not the same thing.
3. List the 5 components of the DBMS environment.
4. Distinguish the DA from the DBA role.
5. Distinguish a naive end-user from a sophisticated end-user.
6. Name 3 advantages and 3 disadvantages of the database approach.
7. Why is "control of redundancy" the more accurate phrase than "elimination of redundancy"?
