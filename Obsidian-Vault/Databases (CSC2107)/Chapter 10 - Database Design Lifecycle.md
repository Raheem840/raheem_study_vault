---
chapter: 10
title: Database System Development Lifecycle
source: "Connolly & Begg, Database Systems, 6th Ed, Ch.10"
course: "[[00 - Course Map|CSC 2107 - Databases]]"
tags: [databases, csc2107, exam-material, design-lifecycle]
---

# Ch.10 — Database System Development Lifecycle

## 1. Why this matters
This is the "big picture" project-management view of building a database system — exams love "list the stages of the DB lifecycle in order" and "what happens at stage X." It also sets up the vocabulary (conceptual/logical/physical design) used throughout [[Chapter 12 - Entity-Relationship Modeling|ER modeling]] and [[Chapter 14 - Normalization|normalization]].

---

## 2. The stages, in order (guaranteed exam list — memorize the sequence)
1. **Database planning** — how to carry out the project's stages efficiently; define the **mission statement** (overall purpose) and **mission objectives** (specific tasks the database must support).
2. **System definition** — describe the scope/boundaries of the system, including all its **user views** (the requirements of a particular job role or application area).
3. **Requirements collection and analysis** — gather and analyze information about the organization's data/processing needs, using fact-finding techniques (see below).
4. **Database design** — build the conceptual, then logical, then physical model of the database (the three sub-phases are the heart of the whole course).
5. **DBMS selection** (optional stage — only if no DBMS is already chosen) — evaluate and select an appropriate DBMS product for the design.
6. **Application design** — design the user interface and application programs that use/process the database (transaction design + UI design).
7. **Prototyping** (optional) — build a working model to let users visualize/evaluate the system before full implementation.
8. **Implementation** — physically create the database (run the DDL) and the application programs.
9. **Data conversion and loading** — transfer any existing data into the new database (from old systems/files).
10. **Testing** — run the system with the intent of finding errors, checking it meets requirements.
11. **Operational maintenance** — ongoing monitoring, tuning, and adapting to new requirements after go-live.

> **Exam trap**: this is NOT strictly a rigid waterfall — the book explicitly notes stages often overlap and involve feedback loops (e.g. testing might send you back to application design) — but you should still know the STANDARD ORDER for listing purposes.

---

## 3. Database design — the 3 sub-phases (THE most important part of this chapter, sets up the rest of the course)
| Phase | What it produces | Independent of DBMS? |
|---|---|---|
| **Conceptual design** | A conceptual data model (entities, relationships, attributes) — built via [[Chapter 12 - Entity-Relationship Modeling|ER modeling]] | Yes — fully independent of any specific DBMS or even the storage model (relational vs otherwise) |
| **Logical design** | A logical data model — maps the conceptual model onto a specific TYPE of data model (e.g. the relational model), including [[Chapter 14 - Normalization|normalization]] | Independent of a specific DBMS PRODUCT, but tied to a specific data model TYPE (e.g. relational) |
| **Physical design** | A physical database design — describes how the logical design will be physically implemented (storage structures, indexes, access methods) for a SPECIFIC target DBMS | Fully DBMS-specific |

> **Exam trap**: "conceptual" and "logical" are often confused — conceptual design doesn't even assume you're using a relational database at all; logical design commits to the relational model (or whichever model) but not yet to a specific product like Oracle vs MySQL; physical design is where product-specific syntax/tuning finally enters.

---

## 4. Fact-finding techniques (used during requirements collection — a testable list)
1. **Examining documentation** — existing forms, reports, files, org charts.
2. **Interviewing** — direct conversations with users/stakeholders; can be structured or unstructured.
3. **Observing the enterprise in operation** — watching how work actually happens (often reveals gaps between documented and actual procedures).
4. **Research** — looking at similar systems, trade journals, standard reference sources for ideas.
5. **Questionnaires** — for gathering information from a large number of people efficiently.

> **Exam trap**: no single fact-finding technique is sufficient alone — a good analyst COMBINES several (e.g. interviews to get depth, questionnaires to get breadth) — a common exam question is "justify which combination of techniques you'd use for [scenario]."

---

## 5. Application design (brief — ties transaction/UI design to the data design)
- **Transaction design**: identify the transactions (retrieval, update, mixed) the application will need to run against the database, BEFORE the physical design is finalized — this influences physical design decisions like indexing.
- **User interface design guidelines**: meaningful titles, clear instructions, logical grouping of fields, sensible defaults, appropriate validation feedback, consistent terminology.

---

## 6. CASE tools (Computer-Aided Software Engineering)
Software tools supporting the database design process — diagramming tools, data dictionaries, prototyping/code-generation tools — used across multiple lifecycle stages to improve consistency and reduce manual errors, especially valuable for keeping large/complex designs synchronized as they evolve.

---

## 7. Quick self-test
1. List the 11 stages of the database system development lifecycle in order.
2. Distinguish a mission statement from mission objectives in database planning.
3. What is a "user view" in the context of system definition?
4. Name and describe the 3 sub-phases of database design, and state which are DBMS-independent.
5. List the 5 fact-finding techniques, and explain why combining several is generally better than relying on one.
6. What does transaction design determine, and why must it happen before physical design is finalized?
7. What is the purpose of CASE tools in this lifecycle?
