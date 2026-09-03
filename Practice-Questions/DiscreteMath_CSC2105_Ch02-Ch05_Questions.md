# CSC 2105 — Practice Questions: Ch.2 Sets/Functions, Ch.3 Algorithms, Ch.4 Number Theory, Ch.5 Induction

Closed-book. Cover the answer key until you've committed to an answer.

## Sets and Functions (Ch.2)
1. A = {1,2,3,4}, B = {3,4,5,6}. Compute A∪B, A∩B, A−B, A⊕B, and |P(A)|.
2. Let f: ℤ → ℤ, f(x) = 2x. Determine if f is injective, surjective (onto ℤ), and explain.
3. Let f: ℤ → ℕ, f(x) = |x|. Determine if f is injective and/or surjective, and explain.

## Algorithms (Ch.3)
4. Reduce to Big-O: f(n) = 7n³ + 2n² log n + 50.
5. Pseudocode has a loop that runs n times, and inside it, a second loop that runs from 1 to the outer loop's current index. What is the Big-O of this nested structure? (Hint: think about the total number of inner-loop iterations across all outer passes.)
6. Explain why binary search is O(log n) — what happens to the search space each step?

## Number Theory (Ch.4)
7. Compute gcd(252, 105) using the Euclidean Algorithm, showing every step.
8. Solve the linear congruence 3x ≡ 4 (mod 7) by finding the modular inverse of 3 mod 7 first.
9. In a small RSA example, p=3, q=11, so n=33 and φ(n)=20. If e=3 (check gcd(3,20)=1), find d such that 3d ≡ 1 (mod 20).

## Induction and Recursion (Ch.5)
10. Prove by induction that 1+3+5+...+(2n−1) = n² for all n ≥ 1.
11. Prove by induction that 2ⁿ > n for all n ≥ 1.
12. Define, with base case and recursive case, a recursive function for computing the sum of the first n positive integers.

---

## ANSWER KEY

**1.** A∪B = {1,2,3,4,5,6}. A∩B = {3,4}. A−B = {1,2}. A⊕B = {1,2,5,6}. |P(A)| = 2⁴ = 16.

**2.** f(x)=2x is injective: if 2a=2b then a=b. NOT surjective onto ℤ: there's no integer x with 2x=3 (only even numbers are ever produced), so odd targets are never hit.

**3.** f(x)=|x| is NOT injective: f(2)=f(−2)=2, but 2≠−2. It IS surjective onto ℕ (including 0): every non-negative integer n is hit by f(n) (and also by f(−n) if n≠0).

**4.** O(n³) — the highest-degree/fastest-growing term is 7n³; n²log n grows slower than n³ for large n, and the constant 50 is dropped entirely.

**5.** Total iterations = 1+2+3+...+n = n(n+1)/2, which is O(n²) — even though it "looks" like it should be less than a full n×n nested loop, the sum of an arithmetic series up to n is still quadratic in n.

**6.** Each comparison eliminates half of the remaining search space (compare to the middle element, discard the half that can't contain the target); starting from n elements, after k halvings only n/2^k elements remain, and the search ends when this reaches 1 — solving n/2^k=1 gives k=log₂(n), hence O(log n) comparisons.

**7.**
```
252 = 2×105 + 42
105 = 2×42 + 21
42 = 2×21 + 0     ← gcd = 21
```

**8.** Find 3⁻¹ mod 7: test values — 3×5=15≡1 (mod 7), so 3⁻¹≡5 (mod 7). Then x ≡ 5×4 = 20 ≡ 6 (mod 7). Check: 3×6=18≡4 (mod 7) ✓. So x≡6 (mod 7).

**9.** Need 3d ≡ 1 (mod 20). Testing: 3×7=21≡1 (mod 20). So d=7. (Public key (33,3), private key d=7 — this is the classic textbook-small RSA example.)

**10.** Base case (n=1): LHS=1, RHS=1²=1. Equal ✓. Inductive step: assume 1+3+...+(2k−1)=k². Show 1+3+...+(2k−1)+(2(k+1)−1) = (k+1)². LHS = k² + (2k+1) = k²+2k+1 = (k+1)² ✓. By induction, the formula holds for all n≥1. ∎

**11.** Base case (n=1): 2¹=2 > 1 ✓. Inductive step: assume 2^k > k for some k≥1. Show 2^(k+1) > k+1. 2^(k+1) = 2·2^k > 2k (using the inductive hypothesis). Since k≥1, 2k = k+k ≥ k+1. So 2^(k+1) > 2k ≥ k+1, giving 2^(k+1) > k+1 ✓. By induction, 2ⁿ > n for all n≥1. ∎

**12.**
```
S(0) = 0                     (base case)
S(n) = n + S(n−1), n ≥ 1     (recursive case)
```
