---
chapter: 8
title: Advanced Counting - Recurrence Relations
source: "Rosen, Discrete Mathematics and Its Applications, 7th Ed, Ch.8"
course: "[[00 - Course Map|CSC 2105 - Discrete Math]]"
tags: [discrete-math, csc2105, exam-material, recurrence-relations]
---

# Ch.8 — Recurrence Relations

## 1. Why this matters
Recurrence relations describe sequences defined in terms of earlier terms (directly connects to [[Chapter 05 - Induction and Recursion]] and to analyzing recursive algorithms' runtime). Classic asks: **"solve this linear recurrence with given initial conditions"**, **"set up a recurrence relation for this counting scenario"**, **"find the recurrence for the runtime of this recursive algorithm."**

---

## 2. What is a recurrence relation?
An equation expressing a term aₙ in terms of one or more previous terms (aₙ₋₁, aₙ₋₂, ...), together with **initial condition(s)** that anchor the sequence (analogous to a base case in recursion).

**Example**: aₙ = aₙ₋₁ + 3, a₀ = 2 → generates 2, 5, 8, 11, ... (arithmetic sequence).

---

## 3. Solving linear homogeneous recurrences with constant coefficients (THE core technique)
A **linear homogeneous recurrence relation of degree k with constant coefficients** has the form:
```
aₙ = c₁aₙ₋₁ + c₂aₙ₋₂ + ... + cₖaₙ₋ₖ
```
("Homogeneous" = no extra standalone term added; "degree k" = depends on the k previous terms.)

### Method: characteristic equation (memorize this procedure)
1. Write the **characteristic equation**: replace aₙ₋ᵢ with r^(k−i), giving a polynomial in r.
   For a degree-2 recurrence aₙ = c₁aₙ₋₁ + c₂aₙ₋₂, the characteristic equation is `r² − c₁r − c₂ = 0`.
2. Solve for the roots r₁, r₂ (factor or use the quadratic formula).
3. **Case A — distinct roots**: general solution is `aₙ = α₁r₁ⁿ + α₂r₂ⁿ`.
   **Case B — repeated root r₀** (degree 2): general solution is `aₙ = α₁r₀ⁿ + α₂n·r₀ⁿ`.
4. Use the given initial conditions to solve for the constants α₁, α₂ (set up a system of equations using a₀, a₁).

### Worked example — Fibonacci-style recurrence
Solve aₙ = aₙ₋₁ + 2aₙ₋₂, with a₀=2, a₁=7.
```
Characteristic equation: r² − r − 2 = 0
Factor: (r−2)(r+1) = 0  →  r₁=2, r₂=−1  (distinct roots)
General solution: aₙ = α₁(2ⁿ) + α₂(−1)ⁿ

Apply initial conditions:
n=0: α₁ + α₂ = 2
n=1: 2α₁ − α₂ = 7
Add the equations: 3α₁ = 9  →  α₁=3, then α₂ = 2−3 = −1

Final closed form: aₙ = 3(2ⁿ) − (−1)ⁿ
```
> **Exam trap**: forgetting the extra factor of `n` in the repeated-root case (Case B) is the single most common mistake — a repeated root needs a linearly independent SECOND solution, which is n·r₀ⁿ, not just another copy of r₀ⁿ.

---

## 4. Setting up recurrence relations from word problems (the other half of the exam skill)
Common scenario types:
- **Compound interest / growth**: aₙ = (1+rate)·aₙ₋₁
- **Tiling/counting problems**: e.g. "ways to tile a 2×n board with 1×2 dominoes" → aₙ = aₙ₋₁ + aₙ₋₂ (splits into: last tile vertical, reducing to n−1 case; or last two tiles horizontal, reducing to n−2 case) — this is literally the Fibonacci recurrence in disguise.
- **Towers of Hanoi**: moving n disks requires moving n−1 disks twice plus one extra move → aₙ = 2aₙ₋₁ + 1, a₁=1 (solving this gives aₙ = 2ⁿ−1).

## 5. Recurrence relations for algorithm runtime (ties directly to [[Chapter 03 - Algorithms]])
**Divide-and-conquer algorithms** naturally produce recurrences: an algorithm that splits a problem of size n into `a` subproblems of size `n/b`, doing `f(n)` extra work to combine results, gives:
```
T(n) = a·T(n/b) + f(n)
```
Example — merge sort: splits into 2 subproblems of half size, with O(n) merge work → T(n) = 2T(n/2) + O(n), which solves to **T(n) = O(n log n)** — this is exactly where the O(n log n) complexity class from Ch.3 actually comes from mathematically, not just as a memorized fact.

---

## 6. Quick self-test
1. Define what makes a recurrence "linear," "homogeneous," and "degree k."
2. Write the characteristic equation for aₙ = 5aₙ₋₁ − 6aₙ₋₂.
3. Solve aₙ = 5aₙ₋₁ − 6aₙ₋₂ with a₀=1, a₁=0 (find roots, general solution, solve for constants).
4. What's different about the general solution form when the characteristic equation has a repeated root?
5. Set up (don't necessarily solve) the recurrence for the Towers of Hanoi problem, and state its initial condition.
6. What recurrence does merge sort satisfy, and what does solving it tell you about merge sort's Big-O?
7. Why is the tiling-a-2×n-board-with-dominoes problem's recurrence the same as Fibonacci's?
