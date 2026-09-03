---
chapter: 5
title: Induction and Recursion
source: "Rosen, Discrete Mathematics and Its Applications, 7th Ed, Ch.5"
course: "[[00 - Course Map|CSC 2105 - Discrete Math]]"
tags: [discrete-math, csc2105, exam-material, induction, recursion]
---

# Ch.5 — Induction and Recursion

## 1. Why this matters
Mathematical induction is THE standard proof technique for statements about all natural numbers — and it's a guaranteed exam question, usually "prove [formula] by induction." Also covers recursive definitions and recursive algorithms.

---

## 2. Mathematical induction — the structure (memorize this template exactly)
To prove a statement P(n) is true for all integers n ≥ n₀:
1. **Base case**: show P(n₀) is true (directly, by substitution/computation).
2. **Inductive step**: assume P(k) is true for some arbitrary k ≥ n₀ (this assumption is the **inductive hypothesis**), then PROVE P(k+1) follows from it.
3. **Conclusion**: by the principle of mathematical induction, P(n) is true for all n ≥ n₀.

### Worked example — the classic sum formula
**Claim**: for all n ≥ 1, 1+2+3+...+n = n(n+1)/2.

**Base case** (n=1): LHS = 1. RHS = 1(2)/2 = 1. Equal. ✓

**Inductive step**: assume 1+2+...+k = k(k+1)/2 (inductive hypothesis). Show 1+2+...+k+(k+1) = (k+1)(k+2)/2.
```
1+2+...+k+(k+1) = [k(k+1)/2] + (k+1)         (using the inductive hypothesis)
                = (k+1)[k/2 + 1]
                = (k+1)(k+2)/2                  ✓ matches the target formula
```
By induction, the formula holds for all n ≥ 1. ∎

> **Exam trap**: the inductive step must explicitly USE the inductive hypothesis (P(k) assumed true) to prove P(k+1) — a "proof" that doesn't reference the assumption isn't a valid inductive proof, it's just restating the claim.

---

## 3. Strong induction — when regular induction isn't enough
**Strong induction**: instead of assuming just P(k), assume P(n₀), P(n₀+1), ..., P(k) are ALL true (everything up to k), then prove P(k+1) follows. Needed when P(k+1) depends on more than just the immediately preceding case — e.g. proving every integer > 1 has a prime factorization often needs strong induction, since you might need to reference a much smaller case, not just k.

> **Exam trap**: regular and strong induction are logically EQUIVALENT in power (anything provable with one is provable with the other) — strong induction is just more convenient when the inductive step naturally needs more than the immediate predecessor.

---

## 4. Recursive definitions
Define an object in terms of smaller instances of itself, with a **base case** to stop the recursion.

**Example — factorial**:
```
f(0) = 1                    (base case)
f(n) = n × f(n−1), n ≥ 1    (recursive case)
```

**Example — Fibonacci**:
```
F(0) = 0, F(1) = 1           (base cases — two needed here)
F(n) = F(n−1) + F(n−2), n≥2  (recursive case)
```

**Recursively defined sets**: define via (1) a base case listing initial elements, and (2) recursive rules for constructing new elements from existing ones — e.g. the set S of all strings of balanced parentheses: base case "" ∈ S; recursive rule: if w ∈ S, then "(w)" ∈ S, and if w₁,w₂ ∈ S, then w₁w₂ ∈ S.

---

## 5. Structural induction
A proof technique for recursively defined structures (trees, recursively defined sets) — mirrors the recursive definition itself: prove the property holds for the base case(s), then prove that IF it holds for the smaller sub-structure(s), it holds for the structure built from them via the recursive rule.

---

## 6. Recursive algorithms
An algorithm that calls itself on a smaller instance of the same problem, always making progress toward a base case.
```
function factorial(n):
    if n == 0: return 1        # base case
    else: return n * factorial(n-1)   # recursive case
```
**Correctness of recursive algorithms is typically proved by (strong) induction** on the size of the input — this directly connects this section back to the proof techniques above.

> **Exam trap**: every recursive definition/algorithm needs a base case that DOESN'T recurse, or it never terminates (infinite recursion) — examiners often ask you to identify a missing or incorrect base case in a buggy recursive definition.

---

## 7. Quick self-test
1. State the 3-part structure of a proof by mathematical induction.
2. Prove by induction that 1+3+5+...+(2n−1) = n² for all n ≥ 1 (write the full base case + inductive step).
3. What distinguishes strong induction from regular induction, and are they equally powerful?
4. Give the recursive definition of the Fibonacci sequence, including base cases.
5. What is structural induction, and when would you use it instead of ordinary induction on integers?
6. Why must every recursive definition include a base case?
7. What proof technique is typically used to prove a recursive algorithm is correct?
