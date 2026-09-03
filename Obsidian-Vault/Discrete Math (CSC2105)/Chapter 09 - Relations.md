---
chapter: 9
title: Relations
source: "Rosen, Discrete Mathematics and Its Applications, 7th Ed, Ch.9"
course: "[[00 - Course Map|CSC 2105 - Discrete Math]]"
tags: [discrete-math, csc2105, exam-material, relations]
---

# Ch.9 — Relations

## 1. Why this matters
Relations formalize "how elements of sets connect to each other" — the foundation for databases (this ties directly to your [[00 - Course Map|other course, Databases CSC2107]] relational model!) and graph theory. Classic asks: **"determine if this relation is reflexive/symmetric/antisymmetric/transitive"**, **"is this relation an equivalence relation / partial order — justify"**, **"find the equivalence classes."**

---

## 2. What is a relation?
A **relation** R from set A to set B is a subset of A×B — a collection of ordered pairs (a,b) where a∈A, b∈B and "a is related to b" (written a R b). A relation ON a set A (from A to itself) is a subset of A×A.

**Representations**: as a set of ordered pairs, as a matrix (1/0 entries), or as a **directed graph** (arrow from a to b if a R b) — the graph representation makes the properties below very visual and intuitive.

---

## 3. The four key properties of a relation on a set (THE core of this chapter — memorize precisely)
| Property | Definition | Directed graph intuition |
|---|---|---|
| **Reflexive** | ∀a∈A, (a,a) ∈ R | Every node has a self-loop |
| **Symmetric** | ∀a,b, (a,b)∈R ⟹ (b,a)∈R | Every arrow has a matching arrow going back |
| **Antisymmetric** | ∀a,b, if (a,b)∈R and (b,a)∈R, then a=b | No two DISTINCT nodes have arrows both ways |
| **Transitive** | ∀a,b,c, if (a,b)∈R and (b,c)∈R, then (a,c)∈R | Any 2-step path implies a direct shortcut arrow |

> **Exam trap #1**: symmetric and antisymmetric are NOT opposites — a relation can be BOTH (e.g. equality: (a,a) pairs only) or NEITHER. Don't assume "not symmetric" means "antisymmetric."
> **Exam trap #2**: to disprove a property, you only need ONE counterexample pair; to prove it, you must show it holds for ALL pairs (or argue generally, not just check a few).

### Worked example
Let A = {1,2,3}, R = {(1,1), (1,2), (2,1), (2,2), (3,3)}.
- Reflexive? Check (1,1),(2,2),(3,3) all present → **Yes**.
- Symmetric? (1,2)∈R and (2,1)∈R also present → check all pairs, holds → **Yes**.
- Antisymmetric? (1,2) and (2,1) are both in R with 1≠2 → **No** (this single pair breaks it).
- Transitive? (1,2)∈R, (2,1)∈R, need (1,1)∈R — yes it is. Check all combinations similarly → **Yes** here.

---

## 4. Equivalence relations
A relation that is **reflexive, symmetric, AND transitive**. Equivalence relations partition a set into **equivalence classes** — [a] = {x ∈ A : x R a} — every element of A belongs to EXACTLY ONE equivalence class, and the classes are disjoint and cover all of A (this partition property is the whole point of an equivalence relation).

**Classic example**: "a ≡ b (mod 5)" on the integers is an equivalence relation; its equivalence classes are the 5 residue classes {...,−5,0,5,10,...}, {...,−4,1,6,11,...}, etc.

---

## 5. Partial orders
A relation that is **reflexive, antisymmetric, AND transitive** — models a "ranking" or "ordering" where not every pair needs to be comparable (unlike a total/linear order, where every pair IS comparable).

**Classic example**: "divides" (a|b) on positive integers is a partial order — reflexive (a|a), antisymmetric (a|b and b|a ⟹ a=b), transitive — but 3 and 5 are incomparable (neither divides the other), so it's only PARTIAL, not total.

**Hasse diagrams**: a simplified visual of a partial order — omit self-loops (reflexivity implied), omit arrows implied by transitivity, draw "greater" elements higher, connect only direct (covering) relationships.

---

## 6. Combining and composing relations
- **Composition**: if R is a relation from A to B and S is from B to C, `S∘R` is the relation from A to C where (a,c) ∈ S∘R if there exists b∈B with (a,b)∈R and (b,c)∈S — "chain" through the intermediate set.
- **Transitive closure**: the smallest transitive relation containing R — found by adding all pairs reachable via any chain of R-steps (computed via repeated composition, or via graph reachability — this is exactly what Warshall's algorithm computes efficiently).

---

## 7. Quick self-test
1. State the formal definitions of reflexive, symmetric, antisymmetric, and transitive.
2. Why are symmetric and antisymmetric not opposites? Give an example of a relation that is both, and one that is neither.
3. What THREE properties define an equivalence relation, and what structural result do they guarantee about the set?
4. What are the equivalence classes of "a ≡ b (mod 3)" on the integers?
5. What THREE properties define a partial order, and how does a partial order differ from a total order?
6. Is "≤" on the real numbers a partial order or total order? Justify.
7. Define relation composition S∘R, and explain what "transitive closure" computes.
