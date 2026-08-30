---
chapter: 6 (lecture: Wk9-10, "CSPs")
title: Constraint Satisfaction Problems
source: "AIMA 4th Ed, Ch.6 (pp.185–207)"
course: "[[00 - Course Map|CSC 2114 - AI]]"
tags: [ai, csc2114, exam-material, csp]
---

# Ch.6 — Constraint Satisfaction Problems (CSPs)

## 1. Why this matters for the exam
Classic ask: **"formulate [map coloring / N-Queens / Sudoku] as a CSP"**, **"trace backtracking search with MRV/degree/LCV heuristics"**, **"run AC-3 and show which values get removed."**

---

## 2. Formal definition (6.1)
A CSP is defined by three things:
1. **Variables** X = {X₁, ..., Xₙ}
2. **Domains** D = {D₁, ..., Dₙ} — the set of possible values for each variable
3. **Constraints** C — restrict which combinations of values are allowed

A **state** = an assignment of values to some/all variables. A **complete assignment** gives every variable a value; a **consistent assignment** violates no constraints. A **solution** is a complete AND consistent assignment.

**Classic example — Map Coloring**: variables = regions, domain = {colors}, constraint = adjacent regions must differ in color.

---

## 3. Why formulate a problem as a CSP instead of general search?
CSPs expose the problem's **structure** (specifically-shaped constraints between specific variables), which lets us use much more powerful general-purpose techniques than plain search: constraint propagation, and heuristics based on the constraint structure itself, not just problem-specific tricks.

---

## 4. Backtracking search (6.3) — the baseline algorithm
Depth-first search that assigns one variable at a time, and **backtracks** (undoes the last assignment and tries a different value) as soon as a constraint is violated — this is called **early failure detection**, far better than generating a full assignment before checking constraints.

### The 3 heuristics that make backtracking efficient (all testable individually)
1. **Minimum Remaining Values (MRV)** — "most constrained variable": pick the unassigned variable with the **fewest legal values left** in its domain. Intuition: fail fast — if a variable is going to run out of options, find out now rather than after wasting time on easier variables.
2. **Degree heuristic** — used to break MRV ties: pick the variable involved in the **most constraints** with other unassigned variables. Reduces future branching.
3. **Least Constraining Value (LCV)** — when choosing a *value* for a variable, prefer the value that **rules out the fewest choices** for neighboring variables. Intuition: keep options open for the rest of the search (opposite goal from the variable-ordering heuristics, which want to fail fast — LCV wants to succeed smoothly).

> **Exam trap**: MRV/degree are about **choosing which variable** to assign next; LCV is about **choosing which value** to try for it. Don't mix them up.

---

## 5. Constraint propagation (6.2) — reducing domains BEFORE/DURING search
Instead of only checking constraints after a full assignment attempt, propagate their implications early to shrink domains.

- **Node consistency**: a variable is node-consistent if every value in its domain satisfies all unary constraints on it.
- **Arc consistency**: an arc X→Y is consistent if for every value in X's domain, there's *some* value in Y's domain that satisfies the constraint between them. If not, remove the offending value from X's domain.
- **AC-3 algorithm**: maintains a queue of arcs; repeatedly picks an arc, applies arc consistency (removing values if needed); if a domain changes, **re-add all arcs pointing into that variable** to the queue (since removing a value might now make previously-consistent arcs inconsistent). Continue until the queue is empty (consistent) or some domain becomes empty (no solution).
  - Worst-case complexity: O(n²d³) for n variables, max domain size d.

> **Exam trace tip for AC-3**: draw a table of arcs; for each arc (Xᵢ, Xⱼ) check every value of Xᵢ against Xⱼ's current domain; cross out unsupported values; if you cross anything out, re-queue arcs (Xₖ, Xᵢ) for every neighbour Xₖ of Xᵢ.

---

## 6. Backtracking + inference combined
Real implementations interleave: assign a variable → propagate constraints (e.g. re-run arc consistency on affected arcs, sometimes called **MAC — Maintaining Arc Consistency**) → if any domain becomes empty, backtrack immediately instead of continuing deeper.

---

## 7. Local search for CSPs (6.4)
- **Min-conflicts heuristic**: start with a complete (but possibly inconsistent) assignment; repeatedly pick a conflicted variable and reassign it to the value that minimizes the number of constraint violations. Surprisingly effective for large CSPs like N-Queens (can solve million-queens problems fast) — a form of local search / hill-climbing over the space of complete assignments.

---

## 8. Exploiting problem structure (6.5)
- **Independent subproblems**: if the constraint graph splits into disconnected components, solve each separately — cost is additive rather than multiplicative in size.
- **Tree-structured CSPs**: if the constraint graph is a tree (no cycles), the CSP can be solved in **linear time** (O(nd²)) via a two-pass algorithm (directional arc consistency, then direct assignment) — no backtracking needed at all.
- **Cutset conditioning**: for a near-tree graph, fix values for a small set of variables (the "cycle cutset") to break all cycles, turning the rest into a tree-structured (fast) problem — repeat for each possible cutset assignment.

---

## 9. Quick self-test
1. Give the 3 formal components of a CSP.
2. What is "early failure detection" in backtracking search, and why does it help?
3. Distinguish MRV, degree heuristic, and LCV — which choose a variable and which chooses a value?
4. Describe the AC-3 algorithm's core loop and its worst-case time complexity.
5. Why does removing a value from Xᵢ's domain require re-adding arcs (Xₖ, Xᵢ) to the AC-3 queue?
6. What is the min-conflicts heuristic, and what type of search is it an example of?
7. Why can a tree-structured CSP be solved in linear time with no backtracking?
