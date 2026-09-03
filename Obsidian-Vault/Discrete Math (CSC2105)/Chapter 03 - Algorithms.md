---
chapter: 3
title: Algorithms
source: "Rosen, Discrete Mathematics and Its Applications, 7th Ed, Ch.3"
course: "[[00 - Course Map|CSC 2105 - Discrete Math]]"
tags: [discrete-math, csc2105, exam-material, algorithms, complexity]
---

# Ch.3 — Algorithms

## 1. Why this matters
This chapter is about the mathematical language for describing and analyzing algorithms — mainly **Big-O notation**. Classic asks: **"give the Big-O of this algorithm/pseudocode"**, **"trace this search/sort algorithm"**, **"order these growth functions from slowest to fastest growing."**

---

## 2. What is an algorithm?
A finite, well-defined sequence of steps that solves a problem or performs a computation, with 5 required properties (testable list): **input**, **output**, **definiteness** (each step precisely defined), **correctness** (produces the right output for every valid input), **finiteness** (terminates after finitely many steps).

## 3. Searching algorithms (classic pair, always compared together)
- **Linear search**: check each element in order until found or the list ends. Worst case: check every element.
- **Binary search**: only works on a SORTED list; repeatedly compare the target to the middle element, discard half the remaining list each time. Far faster than linear search, but requires sorted input.

## 4. Sorting algorithms — know at least these 2 for tracing by hand
- **Bubble sort**: repeatedly compare adjacent pairs, swap if out of order, "bubbling" the largest element to the end each full pass.
- **Insertion sort**: build a sorted portion one element at a time, inserting each new element into its correct position among the already-sorted part.

---

## 5. Big-O notation — THE core topic of this chapter
`f(x) is O(g(x))` means: there exist positive constants C and k such that `|f(x)| ≤ C·|g(x)|` for all x > k. In plain terms: f grows **no faster than** a constant multiple of g, for large enough x — Big-O describes an **upper bound** on growth rate, ignoring constant factors and lower-order terms.

### How to find the Big-O of a polynomial (very testable shortcut)
For a polynomial, the **Big-O is just its highest-degree term**, with the coefficient dropped:
```
f(x) = 3x³ + 5x² + 7   is O(x³)
```
Rule: **drop all lower-order terms and all constant multipliers** — only the fastest-growing term matters for Big-O.

### Common growth rates, ordered slowest → fastest (memorize this order — extremely high-yield)
```
O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(n³) < O(2ⁿ) < O(n!)
```
| Class | Name | Typical example |
|---|---|---|
| O(1) | Constant | Array index access |
| O(log n) | Logarithmic | Binary search |
| O(n) | Linear | Linear search, single loop |
| O(n log n) | Linearithmic | Efficient sorting (merge sort) |
| O(n²) | Quadratic | Nested loops, bubble/insertion sort |
| O(2ⁿ) | Exponential | Generating all subsets of a set |
| O(n!) | Factorial | Generating all permutations |

> **Exam trap**: Big-O gives an UPPER bound, so technically "O(n²)" is also a valid (if unhelpfully loose) bound for an O(n) algorithm — but exam answers should always give the **tightest** (smallest) valid Big-O unless explicitly asked otherwise.

### Big-Omega and Big-Theta (the other two, less emphasized but sometimes tested)
- **Big-Ω (Omega)**: a LOWER bound — f(x) is Ω(g(x)) if f grows at least as fast as g.
- **Big-Θ (Theta)**: f is Θ(g(x)) if f is BOTH O(g(x)) AND Ω(g(x)) — i.e. g is a tight bound in both directions, the algorithm's growth rate exactly matches g's rate (up to constants).

### Complexity of common loop structures (how to derive Big-O from pseudocode)
- A single loop running n times, doing O(1) work per iteration → O(n)
- A loop nested inside another loop, both running n times → O(n²)
- A loop that halves the problem size each iteration (like binary search) → O(log n)
- Two SEPARATE (not nested) loops, each O(n) → still O(n) overall (add, don't multiply, for sequential code — take the max of the two, which is n here)

---

## 6. Quick self-test
1. List the 5 defining properties of an algorithm.
2. What precondition does binary search require that linear search doesn't?
3. Reduce to Big-O: f(x) = 5x⁴ + 2x² + 100.
4. Order these from slowest to fastest growing: O(n²), O(log n), O(1), O(n log n), O(2ⁿ), O(n).
5. What loop structure typically produces O(n²), and what produces O(log n)?
6. Distinguish Big-O, Big-Ω, and Big-Θ.
7. Why is "O(n²)" a technically valid but poor answer for describing an algorithm that is actually O(n)?
