---
chapter: 17 (lecture: Wk6-7, "MDPs")
title: Making Complex Decisions — Markov Decision Processes
source: "AIMA 4th Ed, Ch.17.1–17.3 (pp.562–595)"
course: "[[00 - Course Map|CSC 2114 - AI]]"
tags: [ai, csc2114, exam-material, mdp]
---

# Ch.17 — Markov Decision Processes (MDPs)

## 1. Why this matters for the exam
Classic ask: **"define an MDP formally"**, **"write the Bellman equation and explain each term"**, **"trace one or two iterations of Value Iteration by hand on a small grid."**

---

## 2. What is an MDP, and why "Markov"?
An MDP models **sequential decision-making under uncertainty**: the agent doesn't just make one decision, it acts repeatedly over time, and its actions don't always have certain outcomes.

**Markov property**: the effect of an action taken in a state depends only on that state (not on the history of how the agent got there). This keeps the model computationally tractable.

### Formal definition — 5 components (memorize):
1. **States S** — the set of all possible situations
2. **Actions A(s)** — actions available in state s
3. **Transition model P(s'|s,a)** — probability of reaching state s' given action a in state s (captures uncertainty/stochastic outcomes)
4. **Reward function R(s), R(s,a), or R(s,a,s')** — the immediate numeric payoff
5. **Discount factor γ (gamma), 0 ≤ γ ≤ 1** — how much future rewards are worth relative to immediate ones (γ close to 0 = short-sighted; γ close to 1 = far-sighted)

---

## 3. Policies and utility
- A **policy π(s)** is a mapping from every state to an action — a complete "what to do here" plan (unlike a plan, which is only a fixed sequence).
- An **optimal policy π\*** maximizes the expected sum of (discounted) rewards over time.
- **Utility of a state history**: `U([s₀,s₁,s₂,...]) = R(s₀) + γR(s₁) + γ²R(s₂) + ...` — the discounted sum. Discounting guarantees this sum converges (is finite) even for infinite sequences, as long as γ < 1 and rewards are bounded.

---

## 4. The Bellman Equation — THE core formula of this chapter
The utility of a state = its immediate reward + the discounted expected utility of the best next action:
```
U(s) = R(s) + γ · max_a  Σ_s'  P(s'|s,a) · U(s')
```
Read it as: "current reward, plus the best you can expect to get by acting optimally from here on, discounted."

The **optimal policy** is then simply:
```
π*(s) = argmax_a  Σ_s'  P(s'|s,a) · U(s')
```
i.e., in each state, pick the action that leads (in expectation) to the highest-utility successor states.

---

## 5. Value Iteration (17.2) — the main algorithm to trace by hand
Idea: iteratively update every state's utility estimate using the Bellman equation, until the values stop changing much (converge).

**Algorithm:**
1. Initialize U(s) = 0 for all states (or any starting guess).
2. Repeat: for every state s, compute
   `U'(s) ← R(s) + γ · max_a Σ_s' P(s'|s,a) U(s)`  (using the *previous* iteration's U values)
3. Stop when the maximum change in U across all states is below a small threshold (convergence).
4. Once converged, extract the policy via `π*(s) = argmax_a Σ_s' P(s'|s,a) U(s')`.

> **Exam trace tip**: for a small deterministic-transition grid world, P(s'|s,a) collapses to "1 for the intended next cell, 0 elsewhere" (or split with a small probability of "slipping" sideways in the classic AIMA 4×3 grid example) — always write out U(s) for every state each iteration in a table, one row per iteration.

---

## 6. Policy Iteration (17.3) — the alternative algorithm
Instead of iterating utilities directly, alternate between two steps:
1. **Policy evaluation**: given the current (fixed) policy π, solve for U^π(s) — the utility of following π forever (this is now a *linear* system, no max operator, since the action is fixed by π).
2. **Policy improvement**: for each state, check if switching to a different action (one step, then following π after) would increase utility. If so, update the policy.
3. Repeat until the policy no longer changes (stable) → that's the optimal policy.

**Value Iteration vs Policy Iteration**: Value iteration does many cheap Bellman updates; policy iteration does fewer but more expensive steps (solving a linear system per iteration). Both converge to the same optimal policy.

---

## 7. Worked concept check (do this kind of calculation yourself)
Grid world, one state s with reward R(s) = −0.04, γ = 1, two neighbouring states with current utility estimates U(sA)=0.5 and U(sB)=0.7, and action "move" leads to sA with prob 0.8 and sB with prob 0.2:
```
U'(s) = R(s) + γ · [0.8×U(sA) + 0.2×U(sB)]
      = −0.04 + 1×[0.8×0.5 + 0.2×0.7]
      = −0.04 + [0.40 + 0.14]
      = −0.04 + 0.54 = 0.50
```
If there were a second action to compare, you'd compute its expected value the same way and take the max — that max value becomes U'(s), and the action that achieved it becomes the (candidate) policy at s.

---

## 8. Quick self-test
1. Give the 5 formal components of an MDP.
2. What does the discount factor γ control, and why is discounting needed for infinite-horizon problems?
3. Write the Bellman equation and explain every symbol.
4. Describe Value Iteration's stopping condition.
5. Contrast policy evaluation and policy improvement in Policy Iteration.
6. Why is policy iteration's evaluation step a "linear system" while value iteration is not?
