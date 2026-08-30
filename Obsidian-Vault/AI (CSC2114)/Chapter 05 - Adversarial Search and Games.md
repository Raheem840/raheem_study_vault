---
chapter: 5 (lecture: Wk8, "Adversarial games")
title: Adversarial Search and Games
source: "AIMA 4th Ed, Ch.5.1–5.4 (pp.146–182)"
course: "[[00 - Course Map|CSC 2114 - AI]]"
tags: [ai, csc2114, exam-material, games]
---

# Ch.5 — Adversarial Search and Games

## 1. Why this matters for the exam
Classic ask: **"draw/trace a minimax game tree and compute the values"**, **"apply alpha-beta pruning and show which branches get cut and why"**, **"explain why perfect minimax is infeasible for real games like chess and what we do instead."**

---

## 2. Game setup as a search problem (5.1)
A two-player, zero-sum game (one player's gain = the other's loss) is defined by:
- **Initial state**
- **To-Move(s)** — whose turn it is in state s
- **Actions(s)** — legal moves
- **Result(s,a)** — resulting state
- **Is-Terminal(s)** — is the game over?
- **Utility(s, p)** — the numeric outcome for player p at a terminal state (e.g. +1 win, −1 loss, 0 draw)

Players are conventionally called **MAX** (trying to maximize the utility) and **MIN** (trying to minimize it).

---

## 3. Minimax algorithm (5.2) — THE core algorithm
Assume the opponent plays optimally (always making the move worst for you). Compute a value for every node bottom-up:
- At a **terminal node**: value = Utility(s)
- At a **MAX node**: value = max of children's values
- At a **MIN node**: value = min of children's values

The root's chosen action is the one leading to the child with the best minimax value for the player to move.

**Complexity**: O(b^m) time (b = branching factor, m = max depth) — same as exhaustive DFS, and O(bm) space, since it's depth-first.

> **Exam trace method**: draw the tree, fill in terminal utilities first (given), then propagate upward: MIN nodes take the min of their children, MAX nodes take the max, alternating level by level until you reach the root.

---

## 4. Alpha-Beta Pruning (5.3) — makes minimax practical
Same result as minimax, but prunes branches that **cannot** influence the final decision — cutting search time roughly in half on average (best case: reduces effective branching factor to √b).

- **α (alpha)**: the best value MAX can guarantee so far along the current path (starts at −∞).
- **β (beta)**: the best value MIN can guarantee so far along the current path (starts at +∞).
- **Pruning rule**: at a MAX node, if a child's value ≥ β, stop exploring further children (**β-cutoff**) — MIN would never let play reach here. At a MIN node, if a child's value ≤ α, stop (**α-cutoff**) — MAX would never let play reach here.
- Pass updated α/β values down to children as you go; a node's own α/β window narrows as siblings are explored.

> **Exam trap**: alpha-beta pruning **never changes the final minimax decision** — it only skips work that provably wouldn't change the answer. If asked "does it give a different result from minimax?" — the answer is **no**.

> **Move ordering matters**: exploring the best moves first at each node produces more cutoffs (closer to the best-case √b branching factor); bad ordering gives no benefit over plain minimax.

---

## 5. Why we can't do full minimax on real games
Games like Chess/Go have enormous branching factors and depths — full minimax to terminal states is computationally infeasible. Solution: **cut off search at a limited depth** and use an **evaluation function** Eval(s) to estimate how good a non-terminal state is (instead of a true utility), e.g. material count in chess. This is called **heuristic minimax** / depth-limited minimax with static evaluation.
- A good evaluation function should agree with the true utility at terminal states and be fast to compute.
- **Quiescence search**: extend the search a bit further at "unstable" positions (e.g. right after a capture) so the evaluation isn't made at a misleading moment.
- **Horizon effect**: a danger just beyond the search depth limit gets missed/underestimated because the cutoff happens right before it becomes visible.

---

## 6. Stochastic games (5.5, brief) — games with chance
For games involving randomness (e.g. dice), minimax is extended to **Expectimax**: add **chance nodes** between MAX/MIN nodes, whose value is the **expected value** (probability-weighted average) over the dice/random outcomes, rather than a max or min.
```
Expected-Value(chance node) = Σ_outcomes  P(outcome) × value(outcome)
```

---

## 7. Imperfect information games (5.6, brief)
Games like poker involve **hidden information** (opponent's cards). Pure minimax doesn't directly apply since you don't know the true game state. Modern approaches use techniques from game theory (e.g. computing strategies that are robust against any opponent strategy — related to Nash equilibrium) rather than a single deterministic game tree.

---

## 8. Quick self-test
1. Define MAX and MIN's objectives in a two-player zero-sum game.
2. Write the minimax value rule for a MAX node and a MIN node.
3. State the alpha-beta pruning rule for a beta-cutoff and an alpha-cutoff.
4. Does alpha-beta pruning ever change which move is chosen compared to plain minimax? Why/why not?
5. Why is move ordering important for alpha-beta's efficiency?
6. What problem does an evaluation function solve, and what is the horizon effect?
7. How does Expectimax differ from Minimax, and when is it used?
