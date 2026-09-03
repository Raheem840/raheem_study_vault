---
course: CSC 2107 Database Management Systems
lecturer: Emmanuel Lule
room: LLT 4A / LLT 6A
day: Thursday 11:00–13:00, Friday 12:00–14:00
textbook: "Connolly & Begg, Database Systems: A Practical Approach to Design, Implementation, and Management, 6th Ed"
semester: 2026/2027 Semester I
---

# CSC 2107 — Database Management Systems

> No official Makerere-specific course outline was found, so scope is set to the standard undergraduate DB course core: everything a typical semester-long intro DB course actually examines. The full textbook runs to 34 chapters (distributed DBs, object DBs, web/XML, data warehousing, OLAP, data mining) — those are graduate/elective-level topics in most curricula and are flagged as optional below rather than silently dropped. Say the word and I'll build any of the optional ones too.
>
> **Core scope is now fully complete — all 13 topics covered.**

## Core scope (what a typical exam actually covers)
| # | Topic | Book Chapter(s) | Note |
|---|---|---|---|
| 1 | Introduction to Databases | Ch. 1 | [[Chapter 01 - Introduction to Databases]] |
| 2 | Database Environment & Architectures | Ch. 2–3 | [[Chapter 02-03 - Database Environment and Architectures]] |
| 3 | The Relational Model | Ch. 4 | [[Chapter 04 - The Relational Model]] |
| 4 | Relational Algebra and Calculus | Ch. 5 | [[Chapter 05 - Relational Algebra and Calculus]] |
| 5 | SQL: Data Manipulation | Ch. 6 | [[Chapter 06 - SQL Data Manipulation]] |
| 6 | SQL: Data Definition | Ch. 7 | [[Chapter 07 - SQL Data Definition]] |
| 7 | Database Design Lifecycle | Ch. 10 | [[Chapter 10 - Database Design Lifecycle]] |
| 8 | Entity–Relationship Modeling | Ch. 12 | [[Chapter 12 - Entity-Relationship Modeling]] |
| 9 | Enhanced ER Modeling | Ch. 13 | [[Chapter 13 - Enhanced ER Modeling]] |
| 10 | Normalization | Ch. 14 | [[Chapter 14 - Normalization]] |
| 11 | Advanced Normalization (BCNF, 4NF, 5NF) | Ch. 15 | [[Chapter 15 - Advanced Normalization]] |
| 12 | Transaction Management (ACID, concurrency, recovery) | Ch. 22 | [[Chapter 22 - Transaction Management]] |
| 13 | Security and Administration | Ch. 20 | [[Chapter 20 - Security and Administration]] |

## How the chapters connect (cross-links, not just a flat list)
- Ch.10's "conceptual design" phase = Ch.12's ER Modeling; "logical design" = Ch.14/15's Normalization applied to the ER output.
- Ch.13 (EER) extends Ch.12 — read them back to back.
- Ch.15 builds directly on Ch.14 — BCNF fixes 3NF's blind spot.
- Ch.4's key definitions (candidate/primary/foreign key) are used constantly in Ch.12 (ER keys), Ch.14/15 (normalization), and Ch.7 (SQL constraints).
- Ch.5's algebra (joins, outer joins) is the formal version of Ch.6's SQL JOIN syntax — read together if a join question feels unclear.
- Ch.20's countermeasures (views, GRANT/REVOKE) are the conceptual wrapper around Ch.7's actual SQL syntax.
- Ch.22's recovery techniques connect to Ch.20's backup/recovery security control.
- Ch.9 (Relations) from your [[00 - Course Map|Discrete Math course]] is the mathematical foundation underneath this entire relational model.

## Optional / advanced (full textbook has these — flag if your course actually covers them)
Advanced SQL & Object-Relational (Ch.8–9), full DB design methodology steps (Ch.16–19), Query Processing (Ch.23), Distributed DBMSs (Ch.24–26), Object-Oriented DBMSs (Ch.27–28), Web & XML (Ch.29–30), Data Warehousing/OLAP/Data Mining (Ch.31–34).

## Links
- [[Chapter 01 - Introduction to Databases]]
- [[Chapter 02-03 - Database Environment and Architectures]]
- [[Chapter 04 - The Relational Model]]
- [[Chapter 05 - Relational Algebra and Calculus]]
- [[Chapter 06 - SQL Data Manipulation]]
- [[Chapter 07 - SQL Data Definition]]
- [[Chapter 10 - Database Design Lifecycle]]
- [[Chapter 12 - Entity-Relationship Modeling]]
- [[Chapter 13 - Enhanced ER Modeling]]
- [[Chapter 14 - Normalization]]
- [[Chapter 15 - Advanced Normalization]]
- [[Chapter 22 - Transaction Management]]
- [[Chapter 20 - Security and Administration]]
