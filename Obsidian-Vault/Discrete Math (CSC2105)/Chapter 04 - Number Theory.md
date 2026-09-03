---
chapter: 4
title: Number Theory and Cryptography
source: "Rosen, Discrete Mathematics and Its Applications, 7th Ed, Ch.4"
course: "[[00 - Course Map|CSC 2105 - Discrete Math]]"
tags: [discrete-math, csc2105, exam-material, number-theory]
---

# Ch.4 — Number Theory and Cryptography

## 1. Why this matters
Foundation for cryptography (RSA later in the chapter) and a rich source of proof-writing practice. Classic asks: **"compute gcd(a,b) using the Euclidean algorithm, showing all steps"**, **"solve a linear congruence"**, **"is n prime — justify"**, **"apply the RSA idea to encrypt/decrypt a small example."**

---

## 2. Divisibility
`a | b` ("a divides b") means there exists an integer k such that b = a·k. Key facts:
- If a|b and a|c, then a|(b+c) and a|(b−c).
- If a|b and b|c, then a|c (transitivity).

## 3. Primes and the Fundamental Theorem of Arithmetic
A **prime** number has exactly two positive divisors: 1 and itself. **The Fundamental Theorem of Arithmetic**: every integer greater than 1 can be written as a product of primes in exactly one way (up to ordering) — this uniqueness is why prime factorization is such a powerful tool.

## 4. GCD and the Euclidean Algorithm (HIGH YIELD — practice tracing this by hand)
The **greatest common divisor** gcd(a,b) is the largest integer dividing both a and b.

**Euclidean Algorithm**: repeatedly replace the larger number with the remainder of dividing it by the smaller, until the remainder is 0 — the last nonzero remainder is the gcd.
```
gcd(48, 18):
48 = 2×18 + 12
18 = 1×12 + 6
12 = 2×6  + 0     ← remainder 0, so gcd = 6
```
**Key identity**: gcd(a,b) × lcm(a,b) = a × b — lets you compute lcm quickly once you have the gcd.

### Extended Euclidean Algorithm
Also finds integers x, y such that `ax + by = gcd(a,b)` (Bézout's identity) — essential for computing modular inverses (used in RSA).

---

## 5. Modular arithmetic
`a ≡ b (mod m)` means m | (a−b), i.e. a and b leave the same remainder when divided by m. Modular arithmetic "wraps around" — think of a clock face for mod 12.

**Rules (behave like normal arithmetic, congruence-preserving):**
```
If a≡b (mod m) and c≡d (mod m), then:
  (a+c) ≡ (b+d) (mod m)
  (a×c) ≡ (b×d) (mod m)
```
**Modular exponentiation** (compute aⁿ mod m efficiently via repeated squaring) is the computational backbone of RSA — direct computation of huge powers is infeasible, but squaring-and-reducing at each step keeps numbers small.

---

## 6. Solving linear congruences
`ax ≡ b (mod m)` has a solution if and only if gcd(a,m) divides b. If gcd(a,m) = 1 (a and m are **coprime**), a has a **modular inverse** a⁻¹ (mod m), found via the Extended Euclidean Algorithm, and the unique solution is x ≡ a⁻¹·b (mod m).

---

## 7. RSA cryptography — the payoff of this chapter (conceptual level, matches typical exam depth)
RSA is **public-key cryptography**: encryption and decryption use DIFFERENT keys, so you can publish the encryption key openly while keeping the decryption key secret.

**Setup (simplified):**
1. Choose two large primes p, q. Compute n = p×q and φ(n) = (p−1)(q−1) (Euler's totient).
2. Choose e coprime to φ(n) — this becomes the **public key** (n, e).
3. Compute d = e⁻¹ mod φ(n) (via Extended Euclidean Algorithm) — this is the **private key**.
4. **Encrypt**: C = Mᵉ mod n. **Decrypt**: M = Cᵈ mod n.

**Why it's secure**: computing d from the public key alone requires factoring n back into p and q — believed to be computationally infeasible for sufficiently large primes, even though multiplying p×q is easy. This "easy one way, hard to reverse" asymmetry is the whole basis of RSA's security.

---

## 8. Quick self-test
1. State the Fundamental Theorem of Arithmetic.
2. Trace the Euclidean Algorithm to compute gcd(105, 45), showing every step.
3. If gcd(a,b)=6 and lcm(a,b)=180, and a=18, find b.
4. Define a ≡ b (mod m) in your own words.
5. Under what condition does ax ≡ b (mod m) have a solution, and what's special about the case gcd(a,m)=1?
6. Name the public and private key components in RSA, and state the encryption and decryption formulas.
7. Why is RSA considered secure — what hard problem does breaking it require solving?
