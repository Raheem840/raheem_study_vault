# CSC 2105 — Practice Questions: Chapter 1 (Logic and Proofs)

Closed-book. Cover the answer key until you've committed to an answer.

## Section A — Truth tables & translation
1. Build the full truth table for (p∨q)→r (3 variables, 8 rows).
2. Translate to symbols, letting p = "it rains" and q = "the match is cancelled": "The match is cancelled if and only if it rains, unless the stadium has a roof" (introduce a third variable r = "the stadium has a roof" as needed, and state your assumption in words).
3. Write the converse, inverse, and contrapositive of: "If a number is divisible by 4, then it is divisible by 2." Which of these three is true, and which are you GUARANTEED true just from the original statement being true?

## Section B — Equivalences
4. Prove p→q ≡ ¬p∨q using a truth table.
5. Simplify ¬(p∨(¬p∧q)) step by step using named logical equivalences (not a truth table) down to its simplest form.

## Section C — Quantifiers
6. Negate: "∀x (x is a student → x has a student ID)". Simplify the result using De Morgan's + the conditional-as-disjunction equivalence.
7. Let the domain be all people, and L(x,y) mean "x loves y". Translate ∀x∃y L(x,y) and ∃y∀x L(x,y) into English, and explain why they mean very different things.

## Section D — Rules of inference
8. Given: "If the network is down, the server is unreachable" and "The server is unreachable." Can you validly conclude "The network is down"? Name the fallacy if not.
9. Given: p→q, q→r, and p. Using two rules of inference in sequence, derive r. Name both rules used.

## Section E — Proofs
10. Prove by contradiction: there is no smallest positive rational number. (Sketch the proof structure, you don't need full rigor.)
11. A student "proves" that all odd numbers are prime by checking 3, 5, 7, and 11. What's wrong with this proof, and what would actually disprove the claim?
12. Prove: "If n² is even, then n is even" using proof by contraposition. (Sketch the key steps.)

---

## ANSWER KEY

**1.**
| p | q | r | p∨q | (p∨q)→r |
|---|---|---|---|---|
| T | T | T | T | T |
| T | T | F | T | F |
| T | F | T | T | T |
| T | F | F | T | F |
| F | T | T | T | T |
| F | T | F | T | F |
| F | F | T | F | T |
| F | F | F | F | T |

**2.** Reasonable translation: "unless the stadium has a roof" modifies the whole biconditional as an exception, roughly: `¬r → (q↔p)` — i.e. when there's no roof, cancellation exactly tracks rain; the roof's presence is assumed to override this relationship. (Exact translation may vary — the point is correctly nesting the "unless" as a conditional wrapper around the biconditional, and explicitly stating that assumption.)

**3.** Converse: "If a number is divisible by 2, then it is divisible by 4" — FALSE (e.g. 6 is divisible by 2 but not 4). Inverse: "If a number is not divisible by 4, then it is not divisible by 2" — FALSE (same counterexample, 6). Contrapositive: "If a number is not divisible by 2, then it is not divisible by 4" — TRUE, and this is the ONLY one guaranteed true just from the original being true (contrapositive is always logically equivalent to the original; converse/inverse are not).

**4.** 
| p | q | ¬p | p→q | ¬p∨q |
|---|---|---|---|---|
| T | T | F | T | T |
| T | F | F | F | F |
| F | T | T | T | T |
| F | F | T | T | T |

Identical columns for p→q and ¬p∨q in every row confirms the equivalence.

**5.** ¬(p∨(¬p∧q)) → by Distributive law, p∨(¬p∧q) ≡ (p∨¬p)∧(p∨q) ≡ T∧(p∨q) ≡ p∨q (using Domination and Identity). So the original becomes ¬(p∨q) → by De Morgan's ≡ ¬p∧¬q. Final simplified form: **¬p∧¬q**.

**6.** ¬(∀x (S(x)→I(x))) ≡ ∃x ¬(S(x)→I(x)) [De Morgan's for quantifiers] ≡ ∃x ¬(¬S(x)∨I(x)) [conditional as disjunction] ≡ ∃x (S(x)∧¬I(x)) [De Morgan's for propositions + double negation]. In English: "There exists a student who does NOT have a student ID."

**7.** ∀x∃y L(x,y): "Everyone loves someone (possibly a different person for each x)" — each person just needs SOME person they love, not necessarily the same one. ∃y∀x L(x,y): "There is some ONE specific person who is loved by everyone" — a single universally-beloved individual. The first is much easier to satisfy (each person picks their own answer); the second requires one person to be everyone's answer simultaneously — a far stronger, much less likely claim.

**8.** No — this is the fallacy of **Affirming the Consequent** ("network down → unreachable" plus "unreachable" does NOT let you conclude "network down," since the server could be unreachable for other reasons, e.g. a crashed service). Valid only in the Modus Tollens direction: if you were told the server WAS reachable, you could validly conclude the network is NOT down.

**9.** Step 1: from p→q and q→r, apply **Hypothetical Syllogism** to get p→r. Step 2: from p→r and p (given), apply **Modus Ponens** to get r.

**10.** Assume, for contradiction, that there IS a smallest positive rational number, call it r. Since r is positive and rational, r/2 is also positive and rational, and r/2 < r — this contradicts r being the SMALLEST positive rational. Since the assumption leads to a contradiction, no smallest positive rational number exists.

**11.** This is "proof by example" — checking a handful of specific odd numbers (3, 5, 7, 11) does not establish the claim for ALL odd numbers, since a universal (∀) statement requires a general argument, not selected instances. The claim is actually false, and it takes just ONE counterexample to disprove it: 9 is odd but not prime (9 = 3×3).

**12.** Contrapositive of "n² even → n even" is "n odd → n² odd." Proof: assume n is odd, so n = 2k+1 for some integer k. Then n² = (2k+1)² = 4k²+4k+1 = 2(2k²+2k)+1, which is of the form 2m+1 (odd), since 2k²+2k is an integer. This proves the contrapositive, and since the contrapositive is logically equivalent to the original statement, the original statement ("n² even → n even") is also proven true.
