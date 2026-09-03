---
chapter: 12
title: Entity–Relationship Modeling
source: "Connolly & Begg, Database Systems, 6th Ed, Ch.12"
course: "[[00 - Course Map|CSC 2107 - Databases]]"
tags: [databases, csc2107, exam-material, er-modeling]
---

# Ch.12 — Entity–Relationship Modeling

## 1. Why this matters
ER modeling is THE core conceptual design skill of this course — you WILL be asked to draw (or interpret) an ER diagram from a word-problem scenario. This is [[Chapter 10 - Database Design Lifecycle|Ch.10's]] "conceptual design" phase made concrete.

---

## 2. Entity types
An **entity type** is a group of objects with the same properties, identified as having independent existence (e.g. Staff, Branch, PropertyForRent). An **entity occurrence** (or instance) is one uniquely identifiable object of that type (e.g. one specific staff member).
> **Exam trap**: entity TYPE (the category, like "Staff") vs entity OCCURRENCE (one specific staff member, like "John Smith") — diagrams show TYPES; the database stores OCCURRENCES.

## 3. Relationship types
A **relationship type** is a meaningful association among entity types (e.g. Staff *Manages* Branch). A **relationship occurrence** is one specific, uniquely identifiable association between specific entity occurrences.

### Degree of a relationship type
- **Unary (degree 1)**: relates an entity to ITSELF — also called a **recursive relationship** (e.g. Staff *Supervises* Staff — a staff member supervises other staff members).
- **Binary (degree 2)**: relates two DIFFERENT entity types — by far the most common (e.g. Staff *Manages* Branch).
- **Ternary (degree 3)**: relates three entity types simultaneously — can't always be decomposed into simpler binary relationships without losing information.

## 4. Attributes
| Type | Meaning | Example |
|---|---|---|
| Simple | Cannot be subdivided further | staffNo |
| Composite | Made up of smaller sub-parts | address (= street, city, postcode) |
| Single-valued | One value per entity occurrence | dateOfBirth |
| Multi-valued | Can have MULTIPLE values per occurrence | phoneNumbers (a staff member might have several) |
| Derived | Computed from other attribute(s), not stored directly | age (derived from dateOfBirth) |

**Keys**: the same key concepts from [[Chapter 04 - The Relational Model|Ch.4]] apply here — every entity type needs at least one candidate key (some attribute or combination that uniquely identifies each occurrence).

---

## 5. Strong vs. weak entity types (guaranteed exam distinction)
- **Strong entity type**: existence does NOT depend on another entity type; has its own primary key independent of any other entity.
- **Weak entity type**: existence DEPENDS on some other (owner) entity type — it has NO candidate key of its OWN; it's identified only through its relationship to the owner entity, using a **partial key** combined with the owner's key.

**Example**: a `PropertyRentalReview` might depend entirely on `PropertyForRent` existing first — if the property is deleted, the review has no meaning on its own.

---

## 6. Attributes on relationships
A relationship type itself can have attributes (e.g. the relationship *Staff Manages Branch* might have an attribute `dateStarted` recording when that particular staff member started managing that particular branch) — this attribute belongs to neither entity alone, only to the association between them.

---

## 7. Structural constraints — THE most heavily tested section (multiplicity)
**Multiplicity** describes the number of possible relationship occurrences an entity can participate in.

### Cardinality (the "shape" of the relationship)
| Type | Meaning | Example |
|---|---|---|
| **One-to-one (1:1)** | Each entity on one side relates to AT MOST one entity on the other | Staff *manages* Branch (one staff member manages one branch, one branch has one manager) |
| **One-to-many (1:\*)** | One entity on one side can relate to MANY on the other, but each on the many side relates to only one on the "one" side | Branch *has* Staff (one branch has many staff, each staff belongs to one branch) |
| **Many-to-many (\*:\*)** | Entities on BOTH sides can relate to multiple entities on the other side | Staff *oversees* PropertyForRent in some models where multiple staff can oversee multiple properties |

### Cardinality vs. Participation — the classic exam-trap distinction
- **Cardinality**: the MAXIMUM number of relationship occurrences an entity can be involved in (the 1:1, 1:*, *:* classification above).
- **Participation**: whether EVERY occurrence of an entity type MUST participate in the relationship (**mandatory**, drawn with a solid line / minimum of 1) or NOT (**optional**, drawn with a dashed line / minimum of 0).

> **Exam trap**: cardinality (max) and participation (min) are TWO SEPARATE properties, often written together as `(min, max)` notation — e.g. `(0,1)` means optional participation with a max of one; `(1,*)` means mandatory participation with no upper limit. A common mistake is describing only the cardinality and forgetting to also specify participation, or vice versa.

---

## 8. Problems with ER models (2 named "traps" — very testable)
- **Fan trap**: occurs when a model has one entity connected to TWO other entities via 1:* relationships, but there's actually NO direct relationship between those two "many" entities — the model incorrectly implies a connection can be traced through the first entity that doesn't really represent valid combinations of data. Fix: introduce a direct relationship between the two entities if a real association exists, or restructure the model.
- **Chasm trap**: occurs when a model suggests a relationship exists between entities, but the PATH between them isn't guaranteed to exist for every occurrence (an optional participation somewhere along the path breaks the chain) — leads to LOSING information about entities that legitimately have no connecting path. Fix: identify the missing/optional relationship and add a direct relationship where needed.

---

## 9. Quick self-test
1. Distinguish entity type from entity occurrence.
2. Define unary, binary, and ternary relationships, with an example of each.
3. Distinguish simple, composite, single-valued, multi-valued, and derived attributes.
4. Define strong vs. weak entity type, and explain what a "partial key" is.
5. Can a relationship itself have attributes? Give an example.
6. Distinguish cardinality from participation — which describes the maximum, which describes mandatory vs optional?
7. Define a fan trap and a chasm trap, and explain the fix for each.
