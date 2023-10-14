### Conditional Probability Tables

* Tabular CPT: a lookup table exhaustively enumerate for every parent configuration
* Logical/Deterministic: express as product of $X_i$ or $(1-X_i)$
* Noisy-OR: $P(Y=0|X_1, X_2, \dots, X_k) = \prod_{i=1}^k(1-p_i)^{X_i}$
* Sigmoid CPT: $P(Y=0|X_1, X_2, \dots, X_k) = \sigma (\sum_{i=1}^k \theta_i X_i)$

### d-separation

* Let $X$, $Y$, and $E$ refer to disjoint sets of nodes in a BN. When is $X$ conditionally independent of $Y$ given $E$?
* X and Y are said to be separated by E if every path from a node in X to a node in Y contains one or more nodes in E.

### Inference