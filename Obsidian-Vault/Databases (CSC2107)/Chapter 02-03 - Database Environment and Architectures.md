---
chapter: "2-3"
title: Database Environment and Architectures
source: "Connolly & Begg, Database Systems, 6th Ed, Ch.2-3"
course: "[[00 - Course Map|CSC 2107 - Databases]]"
tags: [databases, csc2107, exam-material, architecture]
---

# Ch.2–3 — Database Environment and Architectures

## 1. Why this matters
The **three-level ANSI-SPARC architecture** and **data independence** are near-guaranteed exam topics — they explain WHY databases are more flexible than file systems at a structural level (building directly on Ch.1's "data dependence" limitation).

---

## 2. The Three-Level ANSI-SPARC Architecture (THE core diagram of this chapter — memorize all 3 levels)
| Level | Also called | Describes | Who sees it |
|---|---|---|---|
| **External level** | View level | Each user/user-group's own customized view of the relevant part of the database | End-users (multiple different external views can exist simultaneously) |
| **Conceptual level** | Community/logical level | The WHOLE database's structure — all entities, relationships, constraints — independent of any specific application or storage detail | DBA / database designers |
| **Internal level** | Physical/storage level | How data is ACTUALLY stored on disk — file structures, indexes, storage allocation | DBMS/storage engine |

**Schemas**: each level has its own **schema** (a description of the structure at that level) — external schemas (one per user view), ONE conceptual schema, ONE internal schema.

### Data independence — the whole POINT of this architecture (guaranteed exam definition)
- **Logical data independence**: the ability to change the CONCEPTUAL schema without affecting EXTERNAL schemas/application programs (e.g. adding a new entity type doesn't break existing views).
- **Physical data independence**: the ability to change the INTERNAL schema (e.g. reorganize storage, add an index, switch storage devices) without affecting the CONCEPTUAL schema or any external schemas.

> **Exam trap**: logical data independence is considered HARDER to achieve than physical data independence, because application programs are more tightly tied to the logical structure of the data they process than to how it's physically stored. This "which is harder and why" framing is a common exam question.

**Mappings**: the DBMS maintains mappings BETWEEN levels (external↔conceptual, conceptual↔internal) so a query written against an external view can be translated all the way down to actual disk access — this mapping machinery is exactly what makes data independence possible.

---

## 3. Database languages
- **DDL (Data Definition Language)** — defines the conceptual/internal schema (and sometimes external schemas): CREATE, ALTER, DROP statements.
- **DML (Data Manipulation Language)** — for querying and updating data: SELECT, INSERT, UPDATE, DELETE.
  - **Procedural DML**: the user specifies WHAT data is needed AND HOW to get it (step by step) — older, navigational style.
  - **Non-procedural (declarative) DML**: the user specifies WHAT data is needed, and the DBMS figures out HOW — this is SQL's style, and is generally easier to use and more efficient (the DBMS's query optimizer picks the access strategy).
- **4GLs (Fourth-Generation Languages)**: even higher-level tools (forms generators, report generators, application generators) built on top of a DML, requiring less code than a full DML query for common tasks.

---

## 4. Data models — 3 categories
1. **Object-based (high-level/conceptual) models**: Entity-Relationship (ER) model, object-oriented model — used for CONCEPTUAL/logical design, close to how humans naturally think about the domain.
2. **Record-based (logical) models**: relational, network, hierarchical models — closer to implementation, describe data as fixed-format records.
3. **Physical (low-level) models**: describe how data is stored at the lowest level — bits, bytes, file organization, indexing structures.

> **Exam trap**: the RELATIONAL model is a record-based (logical) model, NOT the same category as the ER model (which is object-based/conceptual) — students often conflate "relational model" and "ER model" since ER diagrams are usually translated INTO relational schemas, but they belong to different categories in this classification and serve different design phases.

---

## 5. Multi-user DBMS architectures (Ch.3)
- **Teleprocessing**: one central mainframe does ALL processing; "dumb" terminals just display output — earliest architecture, no client-side processing at all.
- **File-server architecture**: the file server just stores/retrieves files; ALL data processing happens on each individual client workstation — high network traffic (whole files, not just query results, move across the network).
- **Two-tier client-server**: the client handles the user interface + application logic; the server (running the DBMS) handles data storage and query processing — client sends a query, server sends back just the result set, drastically reducing network traffic vs file-server.
- **Three-tier client-server**: adds a middle **application/business logic tier** between the client (presentation only) and the database server (data only) — better separation of concerns, easier to scale and maintain, the dominant pattern for modern web applications (client = browser, middle tier = web/app server, data tier = DBMS).
- **N-tier**: further splits the middle tier into multiple specialized layers/services.

### Why three-tier is generally preferred over two-tier (a classic compare/contrast exam question)
- **Thinner clients**: business logic lives on the middle tier, not duplicated across every client.
- **Easier maintenance**: updating business rules means updating the middle tier once, not redeploying every client.
- **Better scalability**: the middle tier can be scaled/load-balanced independently of the database.
- **Improved security**: clients never talk to the database directly.

---

## 6. Functions of a DBMS (a checklist of what "the software" must actually provide — good source of "list X functions" questions)
Data storage/retrieval/update, a catalog accessible to users (metadata about the data itself), transaction support, concurrency control services, recovery services, authorization services, support for data communication, integrity services, services to promote data independence, utility services (import/export, monitoring, performance tuning tools).

---

## 7. Quick self-test
1. Name the three ANSI-SPARC levels and what each one describes.
2. Define logical data independence and physical data independence, and explain which is generally harder to achieve.
3. Distinguish procedural from non-procedural DML, and state which category SQL belongs to.
4. Which category (object-based, record-based, or physical) does the relational model belong to — and is that the same category as the ER model?
5. List the four multi-user DBMS architectures in the order they historically appeared.
6. Give three reasons three-tier client-server architecture is generally preferred over two-tier.
7. Name four functions a DBMS must provide, beyond just storing and retrieving data.
