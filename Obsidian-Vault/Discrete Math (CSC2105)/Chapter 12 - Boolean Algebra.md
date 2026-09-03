---
chapter: 12
title: Boolean Algebra
source: "Rosen, Discrete Mathematics and Its Applications, 7th Ed, Ch.12"
course: "[[00 - Course Map|CSC 2105 - Discrete Math]]"
tags: [discrete-math, csc2105, exam-material, boolean-algebra]
---

# Ch.12 — Boolean Algebra

## 1. Why this matters
This is the direct mathematical foundation of digital logic circuits (and connects straight back to [[Chapter 01 - Logic and Proofs]] — Boolean algebra IS propositional logic, relabeled for hardware). Classic asks: **"simplify this Boolean expression"**, **"draw the logic circuit for this expression / write the expression for this circuit"**, **"find the sum-of-products form given a truth table."**

---

## 2. Boolean values and operators
Boolean algebra works over {0, 1} (equivalently {False, True}), with three core operations mapping directly onto Ch.1's logical connectives:
| Boolean | Logic equivalent | Symbol | Also written |
|---|---|---|---|
| Complement | NOT | x̄ | x' |
| Boolean sum (OR) | ∨ | x + y | x OR y |
| Boolean product (AND) | ∧ | x · y (or xy) | x AND y |

> **Exam trap**: in Boolean algebra "+" means OR (not addition) and "·" means AND (not multiplication) — the notation borrows arithmetic symbols but the rules are different (e.g. 1+1 = 1 in Boolean algebra, not 2).

---

## 3. Boolean identities (mirror Ch.1's logical equivalences exactly — same laws, different notation)
| Law | Form |
|---|---|
| Identity | x·1 = x ; x+0 = x |
| Domination | x·0 = 0 ; x+1 = 1 |
| Idempotent | x·x = x ; x+x = x |
| Complement | x·x̄ = 0 ; x+x̄ = 1 |
| Double complement | (x̄)̄ = x |
| Commutative | x+y = y+x ; xy = yx |
| Associative | (x+y)+z = x+(y+z) |
| Distributive | x(y+z) = xy+xz ; x+(yz) = (x+y)(x+z) |
| **De Morgan's** | (xy)̄ = x̄+ȳ ; (x+y)̄ = x̄ȳ |
| Absorption | x+(xy) = x ; x(x+y) = x |

> Every one of these is literally the corresponding logical equivalence from [[Chapter 01 - Logic and Proofs]] with ∧→·, ∨→+, T→1, F→0. If you know Ch.1 cold, this chapter is mostly relabeling, not new content.

---

## 4. Boolean functions and truth tables
A **Boolean function** F(x₁,...,xₙ) maps each combination of n Boolean inputs to a single Boolean output — fully described by a truth table with 2ⁿ rows.

---

## 5. Sum-of-Products (SOP) — deriving an expression from a truth table (VERY testable procedure)
Given a truth table, you can always construct a Boolean expression:
1. For each row where the output F = 1, write a **minterm**: the AND of all variables (uncomplemented if the variable is 1 in that row, complemented if it's 0).
2. OR all these minterms together — this is the **sum-of-products** (also called **disjunctive normal form**).

**Worked example**: F(x,y) is 1 when (x,y) = (0,1) or (1,0) [this is XOR]:
```
Row (0,1) → minterm: x̄y
Row (1,0) → minterm: xȳ
F(x,y) = x̄y + xȳ     ← this is exactly the standard XOR expression
```

---

## 6. Logic gates and circuits
Each Boolean operator corresponds to a physical **logic gate**: AND gate, OR gate, NOT gate (inverter) are the basic three; NAND, NOR, XOR gates are common combinations. A Boolean expression translates directly into a circuit diagram, and vice versa — this is the literal bridge from math to hardware.

**Functional completeness**: a set of gates is "functionally complete" if ANY Boolean function can be built using only those gates. {AND, OR, NOT} is functionally complete; remarkably, **NAND alone** is also functionally complete (you can build AND, OR, and NOT purely from NAND gates) — a classic exam/interview fact.

---

## 7. Simplifying Boolean expressions
Two main approaches:
- **Algebraically**, applying the identities from Section 3 repeatedly (e.g. absorption, De Morgan's) to reduce term count — useful for exam-style "simplify this expression" questions.
- **Karnaugh maps (K-maps)**: a visual grid-based method for spotting which terms can be combined/eliminated, especially useful for 3-4 variable functions where algebraic simplification gets error-prone by hand.

### Worked simplification example
Simplify: `xy + xy'` (using ' for complement here)
```
xy + xy' = x(y + y')     [distributive]
         = x(1)          [complement law: y+y'=1]
         = x             [identity law]
```

---

## 8. Quick self-test
1. Translate "+" and "·" in Boolean algebra to their logical connective equivalents, and state the trap about their notation.
2. State De Morgan's Laws in Boolean algebra notation.
3. Derive the sum-of-products expression for F(x,y,z) given it's 1 only when exactly one of x,y,z is 1 (this is a 3-input variant of a classic pattern).
4. What are the three basic logic gates, and which single gate type is functionally complete on its own?
5. Simplify algebraically: x + xy (show which law you use).
6. Why is Ch.12 described as "mostly relabeling" of Ch.1's content?
7. What does a Karnaugh map help you do that plain algebraic simplification can make error-prone?
