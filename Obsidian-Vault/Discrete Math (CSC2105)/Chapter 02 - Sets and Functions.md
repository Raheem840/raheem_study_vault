---
chapter: 2
title: Sets, Functions, and Sequences
source: "Rosen, Discrete Mathematics and Its Applications, 7th Ed, Ch.2"
course: "[[00 - Course Map|CSC 2105 - Discrete Math]]"
tags: [discrete-math, csc2105, exam-material, sets, functions]
---

# Ch.2 — Sets, Functions, and Sequences

## 1. Why this matters
Sets and functions are the vocabulary of essentially all later chapters (relations, counting, graphs). Classic asks: **"compute A∪B, A∩B, A−B, A⊕B given these sets"**, **"is this function injective/surjective/bijective — prove it"**, **"find |A×B| and |P(A)|."**

---

## 2. Set basics
A **set** is an unordered collection of distinct elements. `x ∈ A` means x is a member of A; `x ∉ A` means it isn't.
- **Subset**: A ⊆ B means every element of A is also in B.
- **Proper subset**: A ⊂ B means A ⊆ B AND A ≠ B.
- **Empty set** ∅: the set with no elements — a subset of every set.
- **Power set** P(A): the set of ALL subsets of A (including ∅ and A itself). **|P(A)| = 2^|A|** — very testable formula.
- **Cardinality** |A|: the number of elements in A.

## 3. Set operations (know the definitions AND the Venn diagram intuition)
| Operation | Symbol | Definition |
|---|---|---|
| Union | A ∪ B | Elements in A OR B (or both) |
| Intersection | A ∩ B | Elements in BOTH A and B |
| Difference | A − B | Elements in A but NOT in B |
| Complement | A̅ (or Aᶜ) | Elements in the universal set U but NOT in A |
| Symmetric difference | A ⊕ B | Elements in exactly one of A, B (i.e. (A−B) ∪ (B−A)) |
| Cartesian product | A × B | All ordered pairs (a,b) with a∈A, b∈B; **|A×B| = |A|·|B|** |

**Set identities mirror the logical equivalences from Ch.1** (this is not a coincidence — set operations ARE logical operations on membership):
```
De Morgan's for sets:  (A∪B)̅ = A̅ ∩ B̅        (A∩B)̅ = A̅ ∪ B̅
Distributive:  A∩(B∪C) = (A∩B)∪(A∩C)
```
> **Exam trap**: A ⊕ B is NOT the same as A ∪ B — it explicitly EXCLUDES the overlap. Draw the Venn diagram if unsure.

---

## 4. Functions
A **function** f: A → B assigns EXACTLY ONE element of B (the **codomain**) to each element of A (the **domain**). The **range** (or image) is the actual SET of values f produces — the range is a subset of the codomain, not necessarily equal to it.

### The three key properties (THE most tested part of this chapter)
| Property | Definition | Test |
|---|---|---|
| **Injective (one-to-one)** | Different inputs always give different outputs | f(a₁)=f(a₂) ⟹ a₁=a₂ |
| **Surjective (onto)** | Every element of the codomain is hit by SOME input | ∀b∈B, ∃a∈A such that f(a)=b |
| **Bijective** | Both injective AND surjective | One-to-one correspondence — has a well-defined inverse function |

**Worked example**: f: ℝ → ℝ, f(x) = x².
- NOT injective: f(2) = f(−2) = 4, but 2 ≠ −2.
- NOT surjective onto ℝ: no x gives f(x) = −1 (negative outputs are never produced).
- So f is neither injective nor surjective here — but if you restrict the domain to x ≥ 0 AND the codomain to y ≥ 0, it becomes bijective.

> **Exam trap**: injective/surjective/bijective depend on the STATED domain and codomain, not just the formula — the same formula can be injective on one domain and not another (as shown above). Always check the given domain/codomain before answering.

### Inverse functions
f⁻¹ exists (as a proper function) **only if f is bijective**. If f is only injective (not surjective), you can define an inverse on the range, but not on the whole stated codomain.

### Function composition
(f∘g)(x) = f(g(x)) — apply g first, then f. Composition of two bijections is a bijection.

---

## 5. Sequences and summations (brief, ties into later counting/recurrence chapters)
A **sequence** is a function from a subset of integers (usually ℕ) to a set — written a₁, a₂, a₃, ... or {aₙ}.
**Summation notation**: `Σ_{i=1}^{n} aᵢ` means a₁+a₂+...+aₙ.

**Useful closed-form sums to memorize** (come up constantly in later chapters):
```
Σ_{i=1}^{n} i = n(n+1)/2
Σ_{i=0}^{n} 2^i = 2^(n+1) − 1     (geometric series, ratio 2)
Σ_{i=0}^{n} r^i = (r^(n+1) − 1)/(r − 1),  r ≠ 1   (general geometric series)
```

---

## 6. Quick self-test
1. Given A={1,2,3}, B={2,3,4}, compute A∪B, A∩B, A−B, A⊕B.
2. State the formula for |P(A)| given |A|=n, and compute it for n=4.
3. State De Morgan's Law for sets (complement of a union).
4. Define injective, surjective, and bijective precisely.
5. Give a function that is injective but not surjective, and one that is surjective but not injective (state domain/codomain for each).
6. Why does f⁻¹ only exist as a true function when f is bijective?
7. Compute Σ_{i=1}^{10} i using the closed-form formula.
