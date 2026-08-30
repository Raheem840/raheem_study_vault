# CSC 2114 — Practice Questions: ML / Search / MDPs / Games / CSPs / Bayes Nets

Closed-book. Cover the answer key until you've committed to an answer.

## ML (Ch.19)
1. A set S has 10 examples: 7 positive, 3 negative. Compute H(S). *(Ans: H = −(0.7)log2(0.7) − (0.3)log2(0.3) ≈ 0.360 + 0.521 ≈ 0.881 bits)*
2. Explain, with an example, why a decision tree with 100% training accuracy can perform badly on a test set.
3. Compare bagging and boosting: which reduces variance, which reduces bias, and how does each achieve it?
4. Why can't you tune hyperparameters using the test set?

## Search (Ch.3)
5. Formulate the 8-puzzle as a search problem: give its states, actions, and goal test.
6. Trace: on a uniform-cost search where a graph has paths of cost 4, 2+1=3, and 1+1+1=3 to the same goal from start, which path is returned, and why?
7. Prove informally why A* with a consistent heuristic never needs to re-expand a node in graph search.
8. Compare the space complexity of BFS and IDDFS, and explain why IDDFS is often preferred despite repeated work.

## MDPs (Ch.17)
9. Why does an MDP need a discount factor even though the "true" objective might be to maximize total (undiscounted) reward?
10. A state s has R(s) = -1, gamma = 0.9, and one action leading to sA (U=10, prob 0.5) or sB (U=4, prob 0.5). Compute U'(s) for this action. *(Ans: −1 + 0.9×(0.5×10+0.5×4) = −1+0.9×7 = −1+6.3 = 5.3)*
11. Explain why Value Iteration is guaranteed to converge (intuitively).

## Games (Ch.5)
12. Given a 2-ply game tree where MAX's two moves lead to MIN nodes with children [3, 5] and [2, 9] respectively, compute the minimax value at the root and state MAX's chosen move. *(Ans: MIN node 1 = min(3,5)=3; MIN node 2 = min(2,9)=2; root MAX = max(3,2)=3; MAX picks the first move.)*
13. In the same tree, would alpha-beta pruning skip evaluating the "9" leaf? Explain using the alpha/beta values at that point.
14. Why is a good evaluation function necessary for real games like chess, and what two properties should it have?

## CSPs (Ch.6)
15. Formulate Sudoku as a CSP: variables, domains, and constraints.
16. You have variables A, B, C with domains {1,2,3} each, and constraint A≠B, B≠C. Apply MRV: if A is already assigned A=1 and this removes 1 from B's domain, which variable does MRV pick next between B (domain size 2) and C (domain size 3)? Why?
17. Explain why AC-3 re-queues arcs (Xk, Xi) after removing a value from Xi's domain.

## Bayesian Networks (Ch.12–13)
18. A rare event has prior P(E)=0.001. A sensor has P(alarm|E)=0.95 and P(alarm|¬E)=0.02. Compute P(E|alarm). *(Ans: (0.95×0.001) / [(0.95×0.001)+(0.02×0.999)] = 0.00095 / (0.00095+0.01998) = 0.00095/0.02093 ≈ 0.0454, about 4.5%)*
19. Given a network Rain→Sprinkler→WetGrass and Rain→WetGrass directly, write the joint distribution's chain-rule factorization.
20. Explain, using d-separation, why WetGrass and Rain might NOT be independent even after conditioning on Sprinkler.

---

## ANSWER KEY (for items without an inline answer above)

**2.** A tree grown to purity effectively memorizes noise/outliers in the training set (e.g. one mislabeled example creates its own tiny branch); on new data that noise pattern doesn't repeat, so accuracy drops — classic overfitting.

**3.** Bagging trains models independently on bootstrapped samples and averages — reduces variance because errors from different models (fit to different resamples) partially cancel out. Boosting trains models sequentially, each correcting the previous ones' mistakes via reweighting — reduces bias because the ensemble incrementally covers patterns simpler models missed.

**4.** The test set must simulate genuinely unseen data to give an unbiased estimate of generalization; if you tune using it, you leak information from the test set into your model choices, making your final performance estimate overly optimistic (a form of overfitting to the test set itself — use a separate validation set instead).

**5.** States = board configurations (arrangement of 8 tiles + blank in a 3×3 grid); Actions = slide blank Up/Down/Left/Right (when legal); Goal test = matches the target configuration.

**7.** Consistency (h(n) ≤ cost(n,n′)+h(n′)) guarantees that f(n) = g(n)+h(n) never decreases along any path from the start. Since A* expands nodes in non-decreasing order of f, once a node is expanded (removed from the frontier with the lowest f), no later-discovered path to it can have a lower g (and hence lower f) — so it never needs to be re-expanded.

**8.** BFS space is O(b^d) — exponential, since it stores the whole frontier level. IDDFS space is O(bd) — linear, since it's really DFS at each depth limit and only remembers the current path. IDDFS is often preferred because it gets BFS's completeness/optimality guarantees at DFS's memory cost, and its extra "wasted" time from repeating shallow levels is asymptotically negligible (bottom level dominates).

**11.** Value Iteration is a contraction mapping under the Bellman update (each iteration brings utility estimates closer together, shrinking the maximum error by at least a factor of gamma) — since gamma < 1, repeated application converges to a unique fixed point, which is the true optimal utility function.

**13.** Yes. After evaluating "2" as the first child of MIN node 2, beta for MIN node 2 becomes 2. Since this MIN node's parent (root, a MAX node) already has alpha=3 from exploring the first branch, and MIN's current best (beta=2) is already ≤ alpha=3, MAX will never choose this branch regardless of what "9" evaluates to — so alpha-beta prunes it (alpha-cutoff): the "9" leaf is never evaluated.

**14.** Because full minimax to terminal states is computationally infeasible for large game trees (chess, Go) within reasonable time. A good evaluation function should (i) agree with the true utility at terminal states / correlate well with actual winning chances, and (ii) be fast enough to compute at every node within the search's time budget.

**15.** Variables = the 81 cells; Domains = {1,...,9} for each empty cell; Constraints = all-different among each row, each column, and each 3×3 box.

**16.** MRV picks B (domain size 2, smaller than C's 3) — the heuristic always picks the variable with the fewest remaining legal values, to detect dead ends as early as possible.

**17.** Removing a value from Xi's domain might invalidate the arc consistency of any arc (Xk, Xi) that previously relied on that value being present in Xi's domain to support some value of Xk — so those arcs must be rechecked (re-queued) in case they now also need pruning.

**19.** P(Rain, Sprinkler, WetGrass) = P(Rain) · P(Sprinkler | Rain) · P(WetGrass | Sprinkler, Rain)

**20.** Rain has two paths to WetGrass: directly, and via Sprinkler. Conditioning on Sprinkler blocks the indirect path (Rain→Sprinkler→WetGrass, a chain, blocked once the middle node is observed) but the direct edge Rain→WetGrass is a separate, unblocked path — so Rain and WetGrass remain dependent (not d-separated) even given Sprinkler.
