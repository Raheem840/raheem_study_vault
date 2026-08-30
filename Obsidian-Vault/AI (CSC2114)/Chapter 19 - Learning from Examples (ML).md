---
chapter: 19 (lecture: Wk2-3, "Machine Learning")
title: Learning from Examples
source: "AIMA 4th Ed, Ch.19 (pp.651–708) + Ch.21 intro concepts"
course: "[[00 - Course Map|CSC 2114 - AI]]"
tags: [ai, csc2114, exam-material, machine-learning]
---

# ML Block — Learning from Examples

## 1. Why this matters for the exam
This block is usually tested with: (a) definitions of learning types, (b) **trace a decision tree build by hand using entropy/information gain** (a classic numeric question), (c) explain overfitting and how to fix it, (d) compare model types (parametric vs non-parametric, ensembles).

---

## 2. Forms of learning (19.1)
| Type | What's given | Goal |
|---|---|---|
| **Supervised** | Input–output pairs (x, y) | Learn a function f: X→Y that generalizes to new x |
| **Unsupervised** | Inputs only, no labels | Find structure/patterns (e.g. clustering) |
| **Reinforcement** | Rewards/punishments from acting in an environment | Learn a policy that maximizes cumulative reward |
| **Semi-supervised** | Few labeled + many unlabeled | Use unlabeled data to improve the labeled-only model |

Supervised learning splits into:
- **Classification** — output y is discrete/categorical (e.g. spam / not spam)
- **Regression** — output y is continuous/numeric (e.g. predicting price)

---

## 3. The core supervised learning setup
- **Hypothesis space (H)**: the set of candidate functions the learner is allowed to choose from.
- **Training set**: examples used to fit the model.
- **Test set**: held-out examples used to estimate generalization (never used during training!).
- **Overfitting**: model fits the training data (including its noise) so closely that it performs worse on new data. Detected when training accuracy is high but test accuracy is low.
- **Underfitting**: model is too simple to capture the real pattern — poor on both training and test data.
- **Bias–variance tradeoff**: high-bias models (too simple) underfit; high-variance models (too complex/flexible) overfit. Good models balance the two.
- **Cross-validation (k-fold)**: split data into k parts, train on k−1, test on the remaining one, rotate and average — gives a more reliable estimate of generalization than one train/test split.
- **No Free Lunch theorem**: no single learning algorithm is best for every possible problem — averaged over all problems, all algorithms perform the same; you must pick a hypothesis space (inductive bias) suited to your problem.

---

## 4. Decision Trees (19.3) — HIGH YIELD, learn to compute by hand

A decision tree classifies by asking a sequence of attribute-based questions, branching until it reaches a leaf (the predicted class).

### Building one: the ID3 / greedy algorithm
At each node, pick the attribute that **best splits** the remaining examples, using **Information Gain**.

**Entropy** of a set S with classes having proportions pᵢ:
```
H(S) = − Σ pᵢ log₂(pᵢ)
```
- Entropy = 0 → all examples same class (pure, no uncertainty)
- Entropy = 1 → maximum uncertainty for a binary split (50/50)

**Information Gain** of splitting S on attribute A:
```
IG(S, A) = H(S) − Σ_v ( |Sᵥ|/|S| ) · H(Sᵥ)
```
where Sᵥ is the subset of S where attribute A = v. Pick the attribute with the **highest IG** at each node; recurse on each branch.

### Worked mini-example (memorize the mechanics, not this exact table)
9 examples: 6 "Yes", 3 "No" for the target class.
```
H(S) = −(6/9)log₂(6/9) − (3/9)log₂(3/9) ≈ −(0.667)(−0.585) − (0.333)(−1.585)
     ≈ 0.390 + 0.528 ≈ 0.918 bits
```
Then for a candidate attribute A that splits S into two subsets, compute H of each subset, weight by size, subtract from 0.918 to get IG(S,A). Repeat for every candidate attribute; the largest IG wins as the root/next split.

### Overfitting in trees & the fix
Trees can grow until every leaf is pure (perfectly fits training data → overfits). Fixes: **pruning** (remove branches that don't improve validation performance, e.g. via a statistical significance test like chi-squared), or **early stopping** (stop splitting once a node has too few examples or IG is negligible).

---

## 5. Linear models (19.6)
- **Linear regression**: fit `ŷ = w·x + b` minimizing squared error (least squares). Solvable in closed form or via **gradient descent** (iteratively update weights in the direction that reduces loss: `w ← w − α·∇Loss`).
- **Linear classification / logistic regression**: pass `w·x + b` through a **sigmoid** to output a probability; classify by thresholding (e.g. at 0.5). Trained by minimizing a loss like cross-entropy, also via gradient descent.
- **Perceptron**: the simplest linear classifier; updates weights only on misclassified examples; guaranteed to converge only if data is **linearly separable**.

---

## 6. Non-parametric models (19.5)
- **k-Nearest Neighbours (k-NN)**: classify a new point by majority vote (or average, for regression) among its k closest training points (by some distance metric, e.g. Euclidean). No explicit training phase — all computation happens at prediction time ("lazy learning"). Small k → sensitive to noise (high variance); large k → smoother, more biased.

---

## 7. Ensemble learning (19.8)
Combine multiple "weak" models into one stronger model.
- **Bagging (Bootstrap Aggregating)**: train many models independently on random resampled (with replacement) subsets of the data; average/vote their predictions. Reduces **variance**. **Random Forest** = bagging of decision trees + random feature subsets per split.
- **Boosting**: train models sequentially, each new model focusing more on the examples the previous ones got wrong (reweighting). Reduces **bias**. E.g. AdaBoost, Gradient Boosting.

---

## 8. Model selection & optimization (19.4)
- Use validation data (separate from test data) to tune hyperparameters (e.g. tree depth, k in k-NN, regularization strength) without leaking test-set information.
- **Regularization**: add a penalty term for model complexity (e.g. L1/L2 penalty on weights) to discourage overfitting.

---

## 9. Quick self-test
1. Define entropy and information gain, and state the formula for each.
2. Why does a decision tree that perfectly classifies all training examples often perform poorly on new data?
3. Distinguish bagging from boosting — which reduces variance and which reduces bias?
4. What is the No Free Lunch theorem's practical implication for choosing an algorithm?
5. Why is k-NN called a "lazy" learning method?
6. What does cross-validation give you that a single train/test split doesn't?
