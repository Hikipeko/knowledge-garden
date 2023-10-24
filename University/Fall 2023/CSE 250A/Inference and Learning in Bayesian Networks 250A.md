### 2 Probability and Commonsense Reasoning 250A
#### Review of Probability

* M: month, W: weather, S: sidewalk
* M => W => S
* S and M are not [[Unsupervised Learning 445#Marginal independence|marginally independent]]
* S is [[Unsupervised Learning 445#Conditional Independence|conditionally independent]] of M given W

##### Marginalization

$$
P(X=x_i) = \sum_j P(X=x_i,Y=y_j)
$$

##### Bayes Rule

$$
\begin{align}
P(X|Y) &= \frac{P(Y|X)P(X)}{P(Y)}\\
P(X|Y,E) &= \frac{P(Y|X,E)P(X|E)}{P(Y|E)}
\end{align}
$$

#### Example of Commonsense Reasoning

![[Pasted image 20231013162800.png|300]]

* B: burglary, E: earthquake, A: alarm
* $P(B=1) = 0.001, \quad P(E=1) = 0.002$
* $P(B = 1) = 0.001$
* $P(B=1|A=1) = 0.37$
* $P(B=1|A=1,E=1) = 0.0033$
* **Explaining away**: the earthquake explains away the alarm, diminishing our belief in the burglary.

### 3 Belief Networks (BN)

![[Pasted image 20231013194509.png|400]]

* This is just [[Unsupervised Learning 445#L24 Bayesian Networks]]
* Motivation: we want to perform inference in a systematic way.
* **Conditional probability tables (CPTs)** describes how each node depends on its parent.
* BN = DAG + CPTs

### 4 Conditional Probability Tables

* Tabular CPT: a lookup table exhaustively enumerate for every parent configuration
* Logical/Deterministic: express as product of $X_i$ or $(1-X_i)$
* Noisy-OR: $P(Y=0|X_1, X_2, \dots, X_k) = \prod_{i=1}^k(1-p_i)^{X_i}$
* Sigmoid CPT: $P(Y=0|X_1, X_2, \dots, X_k) = \sigma (\sum_{i=1}^k \theta_i X_i)$

#### d-separation

* Let $X$, $Y$, and $E$ refer to disjoint sets of nodes in a BN. When is $X$ conditionally independent of $Y$ given $E$?
* X and Y are said to be separated by E if every path from a node in X to a node in Y contains one or more nodes in E.

### 5 Inference in Polytrees

* Problem: given a set $E$ of evidence nodes, and a set $Q$ of query nodes, how to compute the posterior distribution $P(Q|E)$?

#### Polytrees

![[Pasted image 20231022215159.png|600]]
![[Pasted image 20231023204231.png|400]]

* A singly connected belief network: between any two nodes there is at most one path.
* Let $X$ be a node in a polytree, its parents $U_i$ and its children $Y_i$ are also polytrees.
* We want to compute $P(X = x|E)$, given $E$ as a set of evidence nodes.
* $E = E_X^+ \cup E_X^-$, where $E_X^+$ is evidence from above, and $E_X^-$ is evidence from below
* Let $E_{U_i \backslash X} \subset E_X^+$ denote the evidence in the $i^{th}$ parent's box

$$
\begin{align}
P(X = x | E) &= \frac{P(E_X^-|X = x)P(X=x|E_X^+)}{P(E_X^-|E_X^+)}\\
P(E_X^-|E_X^+) &= \sum_x P(E_X^- | X=x) P(X=x|E_X^+)\\
P(X=x|E_X^+) &=\sum_{\vec u}P(\vec U = \vec u | E_X^+) P(X=x|\vec U = \vec u)\\
P(\vec U = \vec u|E_X^+) &= \prod_{i=1}^mP(U_i = u_i|E_{U_i \backslash X})\\
P(X=x|E_X^+) &= \sum_{\vec u} P(X=x|\vec U = \vec u) \prod_{i=1}^mP(U_i = u_i|E_{U_i \backslash X})\\
P(E_X^-|X=x) &= \prod_{j=1}^n P(E_{Y_j\backslash}X | X = x)
\end{align}
$$

### 6 Inference in Loopy BNs

![[Pasted image 20231023210953.png|400]]

1. Node clustering: merge nodes in the DAG to remove loops
2. Cutset conditioning: remove one or more nodes by instantiating them as evidence

![[Pasted image 20231023211325.png|500]]

* Exact inference in belief networks is NP-hard
* Sampling from the joint distribution in a discrete BN
* Let $Q$ be the query nodes, $E$ be the evidence nodes, how to estimate $P(Q=1|E=e)$?

##### Rejection Sampling

* Sample N times, reject if $E \neq e$
* $P(Q=q|E=e) \approx N(q,e) / N(e)$
* Very inefficient

##### Likelihood weighting

* Instantiate evidence nodes instead of sampling them
* Weight each sample using CPTs at evidence nodes
* Move efficient than rejection sampling, but still very slow

![[Pasted image 20231023213741.png|500]]