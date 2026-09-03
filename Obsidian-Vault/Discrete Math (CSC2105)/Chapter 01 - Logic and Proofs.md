---
chapter: 1
title: Logic and Proofs
source: "Rosen, Discrete Mathematics and Its Applications, 7th Ed, Ch.1"
course: "[[00 - Course Map|CSC 2105 - Discrete Math]]"
tags: [discrete-math, csc2105, exam-material, logic]
---

# Ch.1 — Logic and Proofs

## 1. Why this matters for the exam
Everything else in discrete math (and half of CS theory) rests on this chapter. Classic asks: **"construct a truth table for [expression]"**, **"determine if this argument is valid"**, **"write the converse/inverse/contrapositive"**, **"prove this statement by [method]."**

---

## 2. Propositions and logical connectives
A **proposition** is a declarative sentence that is either true or false, but not both (e.g. "Kampala is the capital of Uganda" — true; "x + 1 = 5" is NOT a proposition on its own since it depends on x, unless x is fixed).

| Connective | Symbol | Name | True when... |
|---|---|---|---|
| Negation | ¬p | NOT | p is false |
| Conjunction | p ∧ q | AND | both p and q are true |
| Disjunction | p ∨ q | OR (inclusive) | at least one of p, q is true |
| Exclusive or | p ⊕ q | XOR | exactly one of p, q is true (not both) |
| Conditional | p → q | IF...THEN | **only false when p is true and q is false** — otherwise true |
| Biconditional | p ↔ q | IFF | p and q have the SAME truth value |

> **Exam trap #1**: `p → q` is TRUE whenever p is FALSE, regardless of q ("vacuously true"). Students constantly get this wrong — memorize the truth table, don't reason from English intuition.
> **Exam trap #2**: inclusive OR (∨) allows both true; exclusive OR (⊕) does not. Default "or" in math/logic is always inclusive unless stated otherwise.

### Truth table for p → q (memorize cold)
| p | q | p → q |
|---|---|---|
| T | T | T |
| T | F | **F** |
| F | T | T |
| F | F | T |

---

## 3. Conditional statement variations (guaranteed exam material)
Given `p → q`:
| Name | Form | Logically equivalent to original? |
|---|---|---|
| Converse | q → p | **No** |
| Inverse | ¬p → ¬q | **No** |
| Contrapositive | ¬q → ¬p | **Yes — always equivalent to p → q** |

> **Exam trap**: converse and inverse are logically equivalent to EACH OTHER (not to the original), while the contrapositive is equivalent to the ORIGINAL. This is the single most tested fact in this section.

---

## 4. Logical equivalence and key laws (De Morgan's Laws — very high yield)
Two propositions are **logically equivalent** (written p ≡ q) if they have identical truth tables.

**De Morgan's Laws** (memorize both directions):
```
¬(p ∧ q) ≡ ¬p ∨ ¬q
¬(p ∨ q) ≡ ¬p ∧ ¬q
```
Intuition: "NOT (A and B)" means "NOT A, or NOT B" (at least one must fail); "NOT (A or B)" means "NOT A, and NOT B" (both must fail).

**Other must-know equivalences:**
- Identity: p ∧ T ≡ p ; p ∨ F ≡ p
- Domination: p ∨ T ≡ T ; p ∧ F ≡ F
- Idempotent: p ∧ p ≡ p ; p ∨ p ≡ p
- Double negation: ¬(¬p) ≡ p
- Conditional as disjunction: **p → q ≡ ¬p ∨ q** (extremely useful for converting conditionals into other forms)
- Distributive: p ∧ (q ∨ r) ≡ (p ∧ q) ∨ (p ∧ r)

---

## 5. Predicates and quantifiers
A **predicate** P(x) is a statement whose truth depends on the variable x (e.g. "x > 5"). It becomes a proposition once x is bound by a **quantifier** or given a specific value.

| Quantifier | Symbol | Meaning | True when... |
|---|---|---|---|
| Universal | ∀x P(x) | "for all x" | P(x) is true for EVERY x in the domain |
| Existential | ∃x P(x) | "there exists x" | P(x) is true for AT LEAST ONE x |

**Negating quantifiers (very testable — a generalized De Morgan's Law):**
```
¬(∀x P(x)) ≡ ∃x ¬P(x)
¬(∃x P(x)) ≡ ∀x ¬P(x)
```
("Not everyone did X" means "someone did NOT do X"; "no one did X" means "everyone did NOT do X.")

**Nested quantifiers — order matters!**
`∀x ∃y P(x,y)` ≠ `∃y ∀x P(x,y)` in general. The first says "for every x, SOME y (possibly different for each x) makes P true"; the second says "there's ONE y that works for ALL x simultaneously" — a much stronger claim.

---

## 6. Rules of inference (for building valid arguments)
| Rule | Form | Reads as |
|---|---|---|
| Modus Ponens | p→q, p ⊢ q | If p implies q, and p is true, then q is true |
| Modus Tollens | p→q, ¬q ⊢ ¬p | If p implies q, and q is false, then p is false |
| Hypothetical Syllogism | p→q, q→r ⊢ p→r | Chain conditionals together |
| Disjunctive Syllogism | p∨q, ¬p ⊢ q | If one of two options is ruled out, the other holds |
| Addition | p ⊢ p∨q | If p is true, "p or anything" is true |
| Simplification | p∧q ⊢ p | If both are true, each individually is true |

An **argument is valid** if the conclusion logically follows from the premises using rules like these — validity is about the FORM of the argument, not whether the premises/conclusion are actually true in reality.

---

## 7. Proof methods (know when to use each)
- **Direct proof**: assume p is true, show q must follow, therefore p → q.
- **Proof by contraposition**: to prove p → q, instead prove the (equivalent) contrapositive ¬q → ¬p — useful when the direct route is awkward.
- **Proof by contradiction**: assume the statement is FALSE (i.e. assume ¬conclusion, along with the premises), derive a logical contradiction (something both true and false), therefore the original statement must be true.
- **Proof by cases**: split into exhaustive cases (e.g. "n is even" or "n is odd"), prove the statement holds in each case separately.
- **Proof by (mathematical) induction**: covered in depth in Rosen Ch.5 — prove a base case, then prove that truth for case k implies truth for case k+1.

### Worked example — classic contradiction proof
**Claim**: √2 is irrational.
**Proof**: Suppose (for contradiction) √2 IS rational, so √2 = a/b for integers a, b with no common factors (fully reduced). Then 2 = a²/b², so a² = 2b² — meaning a² is even, so a itself is even (write a = 2k). Substituting: (2k)² = 2b² → 4k² = 2b² → b² = 2k² — so b² is even, so b is even too. But then both a and b are even, contradicting our assumption that a/b was fully reduced (no common factor). Contradiction ⟹ √2 is irrational. ∎

---

## 8. Quick self-test
1. Write the truth table for p → q from memory.
2. Given "If it rains, the ground is wet," write the converse, inverse, and contrapositive — which is guaranteed logically equivalent to the original?
3. State both De Morgan's Laws.
4. Negate: "∀x, x is a student who has passed the exam." Express the negation using ∃.
5. Explain, with an example, why `∀x ∃y P(x,y)` is not the same as `∃y ∀x P(x,y)`.
6. Name the 4 proof methods described above and give one sentence on when each is most useful.
7. Reproduce the proof that √2 is irrational, in your own words, without looking.
