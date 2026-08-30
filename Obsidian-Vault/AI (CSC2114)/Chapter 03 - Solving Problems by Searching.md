---
chapter: 3 (lecture: Wk4-5, "Search")
title: Solving Problems by Searching
source: "AIMA 4th Ed, Ch.3 (pp.63–109)"
course: "[[00 - Course Map|CSC 2114 - AI]]"
tags: [ai, csc2114, exam-material, search]
---

# Ch.3 — Solving Problems by Searching

## 1. Why this matters for the exam
Classic exam ask: **"trace BFS/DFS/UCS/A* on this graph and give the order nodes are expanded / the path found."** Also: define completeness/optimality/time/space complexity for each algorithm, and prove/explain admissibility & consistency of a heuristic.

---

## 2. Problem formulation (3.1–3.2)
A search problem is defined by 5 things:
1. **Initial state** — where the agent starts
2. **Actions(s)** — the set of actions available in state s
3. **Transition model / Result(s,a)** — the state resulting from doing action a in state s
4. **Goal test** — checks if a state is a goal
5. **Path cost function** — assigns a numeric cost to a path (usually sum of step costs)

The **state space** is the set of all states reachable from the initial state via any sequence of actions. A **solution** is an action sequence from initial state to a goal state; an **optimal solution** has the lowest path cost among all solutions.

**Search tree vs. graph**: the search tree is built by expanding nodes (a node = state + parent + action + path cost + depth); different nodes can represent the same state (leading to redundant work) — a **graph search** avoids this by keeping an "explored"/"visited" set.

---

## 3. Measuring search algorithms — the 4 criteria (always define these in exams)
| Criterion | Meaning |
|---|---|
| **Completeness** | Guaranteed to find a solution if one exists? |
| **Optimality** | Guaranteed to find the lowest-cost solution? |
| **Time complexity** | How many nodes are generated/expanded (usually in terms of branching factor b and depth d)? |
| **Space complexity** | How much memory is used (how many nodes stored at once)? |

---

## 4. Uninformed (blind) search strategies (3.4)
These have no extra information about how "close" a state is to the goal — just the problem definition.

| Algorithm | Strategy | Complete? | Optimal? | Time | Space |
|---|---|---|---|---|---|
| **BFS** (Breadth-First) | Expand shallowest node first (FIFO queue) | Yes | Yes, if step costs equal | O(b^d) | O(b^d) |
| **Uniform-Cost Search (UCS)** | Expand node with lowest path cost g(n) so far (priority queue) | Yes | Yes | O(b^(1+⌊C*/ε⌋)) | same as time |
| **DFS** (Depth-First) | Expand deepest node first (LIFO/stack) | No (can loop forever in infinite/cyclic spaces) | No | O(b^m) | **O(bm)** — its big advantage: linear space |
| **Depth-Limited Search** | DFS with a cutoff depth ℓ | No (if solution deeper than ℓ) | No | O(b^ℓ) | O(bℓ) |
| **Iterative Deepening DFS (IDDFS)** | Repeat depth-limited search with limit = 0,1,2,... | Yes | Yes, if step costs equal | O(b^d) | **O(bd)** |

> **Exam trap**: IDDFS looks wasteful (it redoes shallow levels every iteration) but its time complexity is still O(b^d) because the bottom level dominates the total node count — this surprises people, know it cold.
> **b** = branching factor, **d** = depth of shallowest goal, **m** = max depth of the tree, **C\*** = cost of optimal solution, **ε** = minimum step cost.

---

## 5. Informed (heuristic) search (3.5–3.6)
Use a **heuristic function h(n)** = estimated cost from node n to the nearest goal, to guide the search toward promising nodes.

### Greedy best-first search
Expand the node with the smallest h(n) (ignores cost so far). Fast but **not optimal** and **not complete** in general (can get stuck following a bad estimate).

### A* Search — THE most important algorithm in this chapter
Expand the node with smallest **f(n) = g(n) + h(n)**, where g(n) = cost so far, h(n) = estimated cost to goal.
- **Admissible heuristic**: never overestimates the true cost to the goal (h(n) ≤ h*(n)). Guarantees A* is **optimal** with tree search.
- **Consistent heuristic** (stronger condition): for every edge n→n′, h(n) ≤ cost(n,n′) + h(n′) (triangle inequality). Consistency guarantees optimality **even with graph search** (visited-set pruning), and means f(n) never decreases along any path.
- Every consistent heuristic is admissible, but not vice versa.

**Classic heuristic examples** (know these — very testable):
- 8-puzzle: h₁ = number of misplaced tiles (admissible); h₂ = sum of Manhattan distances of tiles from goal position (admissible, and **dominates** h₁, i.e. h₂(n) ≥ h₁(n) always — dominance means fewer nodes expanded, more efficient).
- Straight-line distance for route-finding (never overestimates true road distance) — classic admissible heuristic.

### A* trace method (how to answer a "trace A*" exam question)
1. Maintain a frontier (priority queue) ordered by f = g + h.
2. Pop the lowest-f node; if it's the goal, stop.
3. Otherwise expand it: for each neighbor, compute g (accumulated) and h (given/looked up), push with f = g+h.
4. Repeat. Always show a table: Node | g | h | f, updated each step.

---

## 6. Heuristic quality
- A heuristic h₂ **dominates** h₁ if h₂(n) ≥ h₁(n) for all n (and both admissible) → h₂ leads A* to expand fewer or equal nodes.
- Heuristics can be generated by solving a **relaxed problem** (a simplified version with fewer constraints, e.g. "ignore the rule that only one tile can move at a time" for the 8-puzzle) — the cost of the relaxed problem's optimal solution is always ≤ the true cost, so it's automatically admissible.

---

## 7. Local search & search in complex environments (Ch.4, brief — often bundled with Ch.3 in lecture)
- **Hill-climbing**: move to the best neighboring state; stops at local maxima, plateaus, or ridges — doesn't guarantee global optimum.
- **Simulated annealing**: like hill-climbing but sometimes accepts worse moves (probability decreasing over time via a "temperature" schedule) to escape local maxima.
- **Genetic algorithms**: maintain a population of candidate solutions, combine ("crossover") and mutate them across generations, keep the fittest.

---

## 8. Quick self-test
1. List the 5 components of a formal search problem.
2. Define completeness and optimality.
3. Why is DFS's space complexity O(bm) instead of O(b^m) like its time complexity?
4. Why is IDDFS's time complexity still O(b^d) despite repeating shallow levels?
5. State the admissibility condition and the consistency condition for a heuristic, and explain the difference.
6. Between h₁ (misplaced tiles) and h₂ (Manhattan distance) for the 8-puzzle, which dominates, and what does that mean for A*'s efficiency?
7. How can you generate an admissible heuristic systematically?
