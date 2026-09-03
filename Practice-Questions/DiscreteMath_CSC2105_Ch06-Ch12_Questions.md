# CSC 2105 — Practice Questions: Ch.6 Counting, Ch.8 Recurrence Relations, Ch.9 Relations, Ch.12 Boolean Algebra

Closed-book. Cover the answer key until you've committed to an answer.

## Counting (Ch.6)
1. A password must be exactly 6 characters, each an uppercase letter or digit (36 choices), repetition allowed. How many possible passwords are there? (Which rule/formula applies?)
2. How many ways can a committee of 4 be chosen from 15 people? (Order matters or not — justify the formula choice.)
3. How many distinct arrangements are there of the letters in "MISSISSIPPI"? (Show the formula setup.)
4. Use the Pigeonhole Principle: show that in any group of 367 people, at least two share the same birthday (365 possible days, ignoring leap years).

## Recurrence Relations (Ch.8)
5. Solve the recurrence aₙ = 4aₙ₋₁ − 4aₙ₋₂ with a₀=1, a₁=4. (Note: this has a repeated root — watch the general solution form.)
6. Set up the recurrence relation for the number of ways to climb n stairs if you can take 1 or 2 steps at a time. What sequence is this?

## Relations (Ch.9)
7. Let A={1,2,3}, R={(1,1),(2,2),(3,3),(1,2),(2,3)}. Determine whether R is reflexive, symmetric, antisymmetric, and transitive — for each, either confirm it holds or give a counterexample.
8. Is "has the same birthday as" an equivalence relation on a set of people? Justify using all three required properties.
9. Give the equivalence classes of "a ≡ b (mod 4)" on the set {0,1,2,...,9}.

## Boolean Algebra (Ch.12)
10. Simplify: x'y + xy + xy' (use Boolean identities, showing each step).
11. Construct the sum-of-products expression for a 3-input majority function M(x,y,z) that outputs 1 when at least 2 of the 3 inputs are 1.
12. Explain, using De Morgan's Law, why a NAND gate alone can implement a NOT gate (hint: connect both NAND inputs to the same signal).

---

## ANSWER KEY

**1.** Product rule applies (6 independent sequential character choices, repetition allowed): 36⁶ = 2,176,782,336.

**2.** Order doesn't matter for a committee (no distinct roles stated) → combination: C(15,4) = 15!/(4!·11!) = 1365.

**3.** "MISSISSIPPI" has 11 letters: M=1, I=4, S=4, P=2. Arrangements = 11!/(1!·4!·4!·2!) = 39,916,800/(1·24·24·2) = 34,650.

**4.** By the Pigeonhole Principle, placing 367 people (items) into 365 possible birthdays (containers) means at least one container (birthday) must receive more than one person, since 367 > 365 — so at least two people share a birthday.

**5.** Characteristic equation: r²−4r+4=0 → (r−2)²=0 → repeated root r=2. General solution: aₙ = α₁(2ⁿ) + α₂n(2ⁿ). Apply initial conditions: n=0: α₁=1. n=1: 2α₁+2α₂=4 → 2(1)+2α₂=4 → α₂=1. Final: **aₙ = 2ⁿ + n·2ⁿ = (n+1)2ⁿ**.

**6.** aₙ = aₙ₋₁ + aₙ₋₂ (the last step taken is either a single step from stair n−1, or a double step from stair n−2), with initial conditions a₁=1 (one way: single step), a₂=2 (two ways: two singles, or one double). This is exactly the **Fibonacci sequence** (shifted).

**7.** Reflexive: (1,1),(2,2),(3,3) all present → **Yes**. Symmetric: (1,2)∈R but (2,1)∉R → **No**, counterexample (1,2). Antisymmetric: no pair (a,b) and (b,a) both present with a≠b (check: (1,2) has no (2,1); (2,3) has no (3,2)) → **Yes**. Transitive: (1,2)∈R, (2,3)∈R, need (1,3)∈R — but (1,3)∉R → **No**, counterexample: (1,2) and (2,3) present but (1,3) missing.

**8.** Yes. Reflexive: everyone has the same birthday as themselves. Symmetric: if A has the same birthday as B, B has the same birthday as A. Transitive: if A shares a birthday with B, and B shares with C, then A shares with C (all three have the same date). All three hold, so it's an equivalence relation — its equivalence classes are the groups of people sharing each specific birthday.

**9.** Classes: {0,4,8} (remainder 0), {1,5,9} (remainder 1), {2,6} (remainder 2), {3,7} (remainder 3).

**10.** x'y + xy + xy' = x'y + x(y+y') [factor x from last two terms] = x'y + x(1) [complement law] = x'y + x [identity law] = (x'+x)(x... — cleaner path: x'y + xy = y(x'+x) = y(1) = y first, then y + xy' = y + x [by the same absorption-style identity: a + a'b = a+b] — **final simplified result: x + y**.

**11.** M(x,y,z)=1 for the rows (1,1,0),(1,0,1),(0,1,1),(1,1,1) — the rows with at least two 1s. Sum-of-products: **M(x,y,z) = xyz' + xy'z + x'yz + xyz**.

**12.** NAND(x,x) = (x·x)' = x' by the idempotent law (x·x=x) combined with the complement operation — feeding the same signal into both NAND inputs collapses the AND to just x, and the NAND's built-in negation then produces x', which is exactly the NOT gate's behaviour. (De Morgan's connection: NAND is literally "AND then NOT," so NAND(x,x) is just NOT(x AND x) = NOT(x).)
