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
* Move efficient than rejection sampling
* Problem: suppose all the evidences are below the queries, then this degrade to rejection sampling.

![[Pasted image 20231023213741.png|500]]

### 7 Inference and Learning in BNs

#### Markov Chain Monte Carlo

![[Pasted image 20231103201659.png|350]]

* **Markov blanket** $B_X$ of a node $X$ consists of its parents, children, and spouses (parents of children).
* The node $X$ is conditionally independent of the nodes outside its Markov blanket given the nodes inside its Markov blanket.

1. **Initialization** Fix evidence nodes to observed values. Initialize non-evidence nodes to random values.
2. **Repeat N times**
	1. Pick a non-evidence node $X$ at random
	2. Use Bayes rule to compute $P(X|B_X)$
	3. Resample $x\sim P(X|B_X)$
	4. Take a snapshot t of all the nodes in the BN.
3. **Estimate** Count the snapshots $N(q, q') \leq n$ with $Q = q \land Q' = q'$.

#### Learning in BNs

* Model data by the BN that assigns it the highest probability.
* In CSE 250A, we will always assume the DAG is given.

##### Assumptions

* The DAG is fixed over a finite set of discrete random variables.
* Data consists of $T$ complete instantiations, independent and identically distributed (IID) from the joint distribution of the BN.

### 8 Learning from Complete Data

#### Maximum Likelihood

* See [[Unsupervised Learning 445#L22 MLE]]
* DAG is specified, and we want to set CPT parameters s.t. P(data) is maximized.

![[Pasted image 20231118130737.png|400]]

##### Solution

$$
\begin{align}
P_{ML}(X_i=x|pa_i=\pi) &= \frac{\#(X_i=x,pa_i=\pi)}{\#(pa_i=\pi)}\\
P_{ML}(X_i=x) &= \frac{\#(X_i=x)}{T}
\end{align}
$$

#### Markov Models

* **Finite context**: to predict the $l^{\text{th}}$ word, it's sufficient to consider $n$ words precede it.
* **Position invariance** predictions should not depend on where the context occurs
* n-gram

#### Naïve Bayes Models

See [[2 Classification 258#Naïve Bayes]].

### 9 Linear Regression and Least-Squares

#### Linear Regression

* Problem: If the parents are real-valued, then it is no longer possible to use to CPT.
* Solution: We can use **Gaussian conditional distribution**.
* We need to learn the variance $\sigma$ and weights $\vec w$.
* Maximizing the log likelihood $P(y1, \dots, y_T|\vec x_1, \dots, \vec x_t)$ is equivalent to the least-square problem for a linear fit.

$$
\begin{align}
P(y|\vec x) &= \frac{1}{\sqrt{2\pi\sigma^2}}\exp\{-\frac{(y - \vec w \cdot \vec x)^2}{2\sigma^2}\}  \\
\sum_ty_t x_{\alpha t} &= \sum_t (\vec w \cdot \vec x_t)x_{\alpha t}\\
\end{align}
$$

![[Pasted image 20231118135130.png|500]]

##### Time Series Prediction

* Problem: Let $\{x_1, x_2, \dots, x_T\}$ be a time series, how can we predict the future from the past?
* Solution: We can maximize $P(x_t | x_{t-1}, \dots, x_{t-d})$.

#### Numerical Optimization

* **Analytical** Compute the gradient and solve for where it vanishes.
* **Numerical** Hillclimbing methods

##### Gradient Ascent/Descent

$$
\theta \leftarrow \theta + \eta_t(\frac{\partial f}{\partial \vec \theta})
$$

Cons: Tricky to tune the learning rate, no guarantee of monotonic convergence.

##### Newton's Method

![[Pasted image 20231118141601.png|400]]

By setting the derivative of quadratic approximation to 0, we obtain

$$
\theta \leftarrow \theta - H^{-1}\nabla f
$$

Cons: expensive to compute, unstable when initial estimate is poor.

#### Logistic Regression

$$P(Y=1|\vec x) = \sigma (\vec w \cdot \vec x) =\frac{1}{1 + \exp(-\vec w \cdot \vec x)}$$

* We do not have an analytical solution for Logistic regression, so we need to use numerical methods.
* Theorem: the log-conditional likelihood for logistic regression is a **concave** function of $\vec w$.
	* The Hessian is negative semidefinite.
* We will converge to the same point no matter what the initialization.
* Set $\eta = 0.2/T$, initialize $w = (0, \dots, 0)$

![[Pasted image 20231118142606.png|450]]

### 10 Learning from Incomplete Data

* The data is IID, but only consists of T partially complete instantiations of the nodes in the BN.
* Example: build a movie recommendation system, assume the Bayes Network is as follow.

![[Pasted image 20231118143211.png|400]]

* $H_t$ denotes the set of hidden variables for the $t^{\text{th}}$ example.
* $V_t$ denotes the set of visible variables for the $t^{\text{th}}$ example.
* When maximizing the log-likelihood, we can not decouple CPTs as for complete data.

![[Pasted image 20231118161419.png|400]]

#### Auxiliary Functions

$$
\vec \theta_{\text{new}} = \arg\max_{\vec\theta} Q(\vec \theta, \vec \theta_{\text{old}})\quad(*)
$$

* A function $Q(\vec \theta', \vec \theta)$ is call an auxiliary function for the objective function $f(\vec\theta)$ if it satisfies two properties:
	1. $Q(\vec \theta, \vec \theta) = f(\vec \theta)$ for all $\vec \theta$
	2. $Q(\vec \theta', \vec \theta)\leq f(\vec \theta')$ for all $\vec \theta, \vec \theta'$.
* Theorem: Update rule $(*)$ converge monotonically to a stationary point with $f(\vec \theta_{\text{new}}) \geq f(\vec \theta_{\text{old}})$ 

![[Pasted image 20231117164247.png|400]]

### 11 EM Algorithm

* See [[Unsupervised Learning 445#L23 EM]].
* E-Step: Compute posterior probabilities $P(H_t=h|V_t = v_t)$ based on current CPT
* M: compute CPT based on point assignment

##### E-Step (Inference)

* At root nodes: $P(X_i = x|V_t = v_t)$
* At other nodes: $P(X_i = x,\text{pa}_i = \pi|V_t = v_t)$
* These probabilities must be computed over a quadruple loop:$V_t, X_i, X_i = x, \text{pa}_i = \pi$.

##### M-Step (Learning)

![[Pasted image 20231118163337.png|450]]

#### Derivation

**Key Inequality**

$$
\log \tilde P(v) \geq \sum_h P(h|v) \log \frac{\tilde P(h,v)}{P(h|v)}
$$

* Let $\Theta$ denote the collection of CPTs in a BN.
* Let $\{v_t\}_{t=1}^T$ denote an incomplete data set for this BN.

$$
\begin{align}
\mathcal{L}(\Theta) &= \sum_t \log P(V_t = v_t) \\
Q(\tilde \Theta, \Theta) &= \sum_t \sum_h P(H_t = h|V_t = v_t) \log \frac{\tilde P(H_t = h, V_t = v_t)}{P(H_t = h|V_t = v_t)}\\
(i)\; Q( \Theta, \Theta) &= \mathcal{L}(\Theta)\\
(ii)\; Q(\tilde \Theta, \Theta) &\leq \mathcal{L}(\tilde \Theta)
\end{align}
$$

* E-Step is equivalent to computing of the auxiliary function.
* M-Step is equivalent to maximizing the function.