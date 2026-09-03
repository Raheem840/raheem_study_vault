---
chapter: 6
title: Counting
source: "Rosen, Discrete Mathematics and Its Applications, 7th Ed, Ch.6"
course: "[[00 - Course Map|CSC 2105 - Discrete Math]]"
tags: [discrete-math, csc2105, exam-material, counting, combinatorics]
---

# Ch.6 — Counting

## 1. Why this matters
Combinatorics word problems are a guaranteed, heavily-weighted exam section — "how many ways to..." questions. The skill is picking the RIGHT technique/formula for the scenario, not memorizing formulas in isolation.

---

## 2. The two basic counting rules
- **Product rule**: if a task consists of a sequence of k independent steps, with n₁ ways to do step 1, n₂ ways to do step 2, etc., the total number of ways is n₁×n₂×...×nₖ. (Use when tasks happen "AND" in sequence.)
- **Sum rule**: if a task can be done in one of several DISJOINT (mutually exclusive) ways, with n₁ ways for the first way, n₂ for the second, etc., the total is n₁+n₂+...+nₖ. (Use when tasks are alternative, non-overlapping choices — "OR".)

## 3. Inclusion-Exclusion Principle (for overlapping / non-disjoint sets)
```
|A∪B| = |A| + |B| − |A∩B|
|A∪B∪C| = |A|+|B|+|C| − |A∩B|−|A∩C|−|B∩C| + |A∩B∩C|
```
Used whenever the sum rule's "disjoint" requirement is violated — you must subtract the double-counted overlap.

---

## 4. Permutations — ORDER MATTERS
A **permutation** is an ordered arrangement of objects.
- **All n objects, no repetition**: n! (n factorial) arrangements.
- **r objects chosen from n, no repetition, order matters**: `P(n,r) = n! / (n−r)!`

## 5. Combinations — ORDER DOESN'T MATTER
A **combination** is an unordered selection.
```
C(n,r) = n! / [r!(n−r)!]     (also written "n choose r", or (n r))
```
**Relationship**: `P(n,r) = C(n,r) × r!` — a combination counts the selection; multiplying by r! accounts for all the orderings of that selection that a permutation would count separately.

> **Exam trap**: the single most common student error is using P(n,r) when the problem doesn't care about order (e.g. "choose a committee of 3") — always ask first: **does rearranging the selected items create a genuinely different outcome?** If no → combination. If yes → permutation.

### Worked comparison
"How many ways to award gold/silver/bronze medals to 3 of 10 racers?" → **order matters** (different medal = different outcome) → P(10,3) = 10×9×8 = 720.
"How many ways to choose 3 of 10 racers for a relay team (no distinct roles)?" → **order doesn't matter** → C(10,3) = 10!/(3!7!) = 120.

---

## 6. Permutations/combinations with repetition
- **Permutations WITH repetition allowed** (r positions, n choices each, repeats OK): `nʳ`.
- **Combinations WITH repetition allowed** (choosing r items from n TYPES, unlimited supply of each type, order doesn't matter): `C(n+r−1, r)` — the "stars and bars" formula.

### Permutations of a multiset (objects with duplicates)
If you're arranging n objects where there are n₁ identical objects of type 1, n₂ of type 2, etc.:
```
n! / (n₁! × n₂! × ... × nₖ!)
```
Example: arrangements of the letters in "MISSISSIPPI" (11 letters: M=1, I=4, S=4, P=2):
`11! / (1!×4!×4!×2!) = 39,916,800 / (1×24×24×2) = 34,650`

---

## 7. The Pigeonhole Principle (short, conceptual, but a favorite exam trick question)
If you place more than n items into n containers, at least one container must contain more than one item. **Generalized version**: placing N items into k containers means at least one container has ≥ ⌈N/k⌉ items.
> Classic exam framing: "Show that among any 13 people, at least two share a birth month." (13 people, 12 months → by pigeonhole, at least one month has ≥2 people.)

---

## 8. Binomial Theorem (ties combinations to algebra — occasionally tested)
```
(x+y)ⁿ = Σ_{k=0}^{n} C(n,k) xⁿ⁻ᵏ yᵏ
```
The coefficients are exactly the combination values C(n,k) — this is why C(n,k) is also called a "binomial coefficient."

---

## 9. Quick self-test
1. State the product rule and sum rule, and explain when to use each based on "AND" vs "OR" framing.
2. Write the Inclusion-Exclusion formula for |A∪B|.
3. Give the formulas for P(n,r) and C(n,r), and the relationship between them.
4. A lottery draws 6 numbers from 49, order doesn't matter. How many possible outcomes? (Set up the calculation.)
5. How many distinct arrangements are there of the letters in "BALLOON"? (Set up the calculation, noting repeated letters.)
6. State the Pigeonhole Principle and apply it to prove: among any 32 students, at least 3 share a birth day-of-week (7 days).
7. What do the coefficients in the Binomial Theorem represent?
