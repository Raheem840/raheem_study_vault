---
chapter: "12-13 (lecture: Wk11-14, 'Markov & Bayesian networks')"
title: Quantifying Uncertainty & Probabilistic Reasoning (Bayesian Networks)
source: "AIMA 4th Ed, Ch.12 (pp.385–411) + Ch.13.1–13.4 (pp.412–450)"
course: "[[00 - Course Map|CSC 2114 - AI]]"
tags: [ai, csc2114, exam-material, probability, bayesian-networks]
---

# Ch.12–13 — Probability & Bayesian Networks

## 1. Why this matters for the exam
Classic ask: **"apply Bayes' rule to a diagnostic-style word problem"**, **"build a Bayesian network from a described scenario and write its joint distribution factorization"**, **"perform exact inference by enumeration on a small network."**

---

## 2. Why probability, not just logic? (12.1)
Pure logic requires certainty (true/false); real agents must act under **uncertainty** — incomplete information, noisy sensors, unpredictable environments. Probability theory gives a principled way to represent and combine degrees of belief.

---

## 3. Basic probability notation (12.2–12.3)
- **Prior (unconditional) probability**: P(A) — belief before any evidence.
- **Conditional probability**: P(A|B) = P(A∧B) / P(B) — belief in A given B is known.
- **Joint probability distribution**: P(X₁, X₂, ..., Xₙ) — probability of every combination of variable values; contains everything you could possibly compute about the domain (but grows exponentially in size).
- **Inference by enumeration**: to answer any query, sum the relevant rows out of the full joint distribution:
  `P(X | e) = α · Σ_y P(X, y, e)` where α normalizes so probabilities sum to 1, y ranges over hidden (unobserved) variables.

### Key rules (memorize)
- **Product rule**: P(A∧B) = P(A|B)P(B) = P(B|A)P(A)
- **Sum rule / marginalization**: P(A) = Σ_b P(A, B=b)
- **Independence**: A and B are independent if P(A∧B) = P(A)P(B), equivalently P(A|B) = P(A).
- **Conditional independence**: A and B are conditionally independent given C if P(A|B,C) = P(A|C).

---

## 4. Bayes' Rule (12.5) — THE core formula
```
P(A|B) = [ P(B|A) · P(A) ] / P(B)
```
- Used heavily for **diagnostic reasoning**: given an effect (symptom/evidence), infer the probability of a cause.
- P(A) = prior; P(B|A) = likelihood; P(B) = evidence (normalizer); P(A|B) = posterior.

### Worked example (classic disease-test style — practice this pattern)
Disease prevalence P(D) = 0.01. Test sensitivity P(+|D) = 0.99 (true positive rate). False positive rate P(+|¬D) = 0.05.
```
P(D|+) = P(+|D)P(D) / [ P(+|D)P(D) + P(+|¬D)P(¬D) ]
       = (0.99 × 0.01) / [ (0.99×0.01) + (0.05×0.99) ]
       = 0.0099 / (0.0099 + 0.0495)
       = 0.0099 / 0.0594 ≈ 0.1667  (≈16.7%)
```
> **Exam trap**: people expect a positive 99%-sensitive test to mean "you're almost certainly sick" — but with a low base rate, the posterior can still be low. Always plug into the full formula, don't guess.

### Naive Bayes model (12.6)
Assumes all evidence variables are **conditionally independent given the class**:
```
P(Class | e₁,...,eₙ) = α · P(Class) · Π_i P(eᵢ | Class)
```
Simple, scales well, works surprisingly well in practice (e.g. spam filters) even though the independence assumption is rarely exactly true.

---

## 5. Bayesian Networks (13.1–13.2) — representing a full joint distribution compactly
A **Bayesian network** is a directed acyclic graph (DAG) where:
- Each **node** = a random variable
- Each **edge** X→Y means X is a direct influence ("parent") of Y
- Each node has a **Conditional Probability Table (CPT)**: P(node | its parents)

### The chain rule for Bayesian networks (memorize)
```
P(X₁, ..., Xₙ) = Π_i P(Xᵢ | Parents(Xᵢ))
```
This factorization is why Bayes nets are compact: instead of one huge joint table (2ⁿ entries for n binary variables), you only need a CPT per node conditioned on its (usually few) parents.

### Building a network from a scenario (exam skill)
1. Identify the variables.
2. Decide causal/influence direction (parent → child, "cause → effect" ordering usually gives the most compact, intuitive network).
3. Draw edges for direct dependencies only.
4. Fill in each node's CPT: P(node = true | each combination of its parents' values).

---

## 6. Conditional independence & d-separation (13.2)
A Bayes net encodes: **each node is conditionally independent of its non-descendants, given its parents.**
- **d-separation** is the graphical test for whether two variables are conditionally independent given a set of evidence variables — check every path between them; a path is "blocked" if it goes through a) a chain or fork with the middle node in the evidence set, or b) a **collider** (both edges point into it) whose descendants are *not* in the evidence set. If ALL paths are blocked, the two variables are conditionally independent given that evidence.

---

## 7. Exact inference in Bayesian networks (13.3)
- **Inference by enumeration**: sum out all hidden variables from the full joint (reconstructed via the chain rule above) to compute the query — correct but exponential in the worst case.
- **Variable elimination**: smarter version — eliminate (sum out) hidden variables one at a time, caching intermediate "factors" instead of recomputing the whole joint. Much more efficient in practice, especially on sparse/tree-like networks, though still exponential in the worst case for densely connected networks.

## 8. Approximate inference (13.4, brief)
For large networks, exact inference is infeasible; use **sampling methods**:
- **Direct/Prior sampling**: sample each variable in topological order according to its CPT; repeat many times; estimate probabilities by counting.
- **Rejection sampling**: like direct sampling, but throw away samples inconsistent with the observed evidence — wasteful if evidence is rare.
- **Likelihood weighting**: fix evidence variables to their observed values, sample the rest, and weight each sample by how likely the fixed evidence was — avoids waste, but weights can become very uneven.

---

## 9. Quick self-test
1. State Bayes' rule and label prior, likelihood, evidence, posterior.
2. Why can a 99%-sensitive medical test still give a low P(disease | positive) result? (Explain with the base-rate idea.)
3. What independence assumption does Naive Bayes make?
4. Write the Bayesian network chain-rule factorization of a joint distribution.
5. What does an edge X→Y mean in a Bayesian network?
6. Explain d-separation's blocking conditions in your own words.
7. Contrast inference by enumeration and variable elimination.
8. Name one approximate inference method and briefly describe how it works.
