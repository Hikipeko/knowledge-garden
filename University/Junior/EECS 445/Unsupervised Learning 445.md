What if we don't have labels?

## L17 K-Means Clustering

![[Pasted image 20230103005915.png|400]]

**Input**: $S_n = \{\bar x^{(i)} \}_{i = 1}^n$

**Output**: a set of cluster assignments $c_1, ..., c_n$.

**Goal**

Assign similar points to the same clusters.

Partition data into k clusters such that the sum of squares of Euclidean distances of each points to its cluster's mean is minimized.

##### Algorithm

Iteratively do following until convergence. (guaranteed)

1. For each points, reassign x to the cluster with closest distance to cluster mean.
2. Re-compute each cluster mean.

**How to initialize the means**

Pick clustering with lowest objective function value.

**How to pick k**

Elbow method

![[Pasted image 20230103011905.png|400]]

K-means performs **coordinate descent** on the objective function.

It is guaranteed to converge, but not guaranteed to converge to global minimum.

**K-means++ Approximation Guarantee**

The expected value of the objective returned by k-mean++ is never more than $O(\log K)$.



## L18 Hierarchical Clustering

![[Pasted image 20230103012228.png]]

**Idea**

Start with each point in its own cluster and build a hierarchy.

1. Assign each point its own cluster.
2. Find the closest clusters and merge them until convergence.

Find a cut for k clusters.

**Linkage Criteria**

Single-linkage: closest points

Complete-linkage: farthest points

Average-linkage: average every combination of points in the two clusters

Centroid-linkage: distance the centroids of two clusters



## L19 Spectral Clustering

##### Defect of k-means

![[Pasted image 20230103012612.png]]

* Spectral clustering reformulated data clustering problem as [[Maximum Flow Min Cut 477|graph partition problem]], actually a ratio cut.
* Doesn't assume globular-shaped clusters.

##### Gaussian kernel similarity metric
$$
w_{ij} = \exp (-\frac{||x^{(i)} - x^{(j)}||^2}{2\sigma^2})
$$

##### Cost of a cut
$$
cut(A, \bar A) = \sum_{i \in A, j \in \bar A} w_{ij}
$$

##### Ratio cut

$$
RatioCut(A_1, ..., A_k) = \frac{1}{2}\sum_{i=1}^k\frac{cut(A_i, \bar A_i)}{|A_i|}
$$

##### Graph Laplacian

$$
\begin{align}
d_{ii} &= \sum_{j = 1} ^n w_{ij} \\
L &= D - W\\
\end{align}
$$
* The problem of minimizing the ratio cut can be approximate by minimizing $\min_F \text{Tr}F^TLF$, where $F$ is the indicator vectors. This is just minimizing $\sum_{(i<j)}w_{ij}(v_i - v_j)^2$
* Goal: minimizing the ratio cut, which is similar to the following problem.
* L is symmetric, PSD, has n non-negative real-valued eigenvalues.
* The multiplicity of the eigenvalues 0 is the number of connected components in the graph.
* For the special case of $k=2$, we can let $f_i = \sqrt{|\bar A| / |A|}$ if $v_i \in A$ and $-\sqrt{|A| / |\bar A|}$ otherwise. The cost function is $(|A|/|\bar A| + |\bar A| / |A| + 2)\sum_{e \in Cut} w_e$, pushing towards a balanced cut.
* Finding $\min_f f^TLf$ is NP-hard, so we look for real number solutions.

##### Steps

1. Build adjacency matrix $W$.
2. Compute graph Laplacian $L = D - W$.
3. Compute [[MATH 214#7 Eigenvalues and Eigenvectors|eigenvectors]] of $L$.
4. Build matrix with the first k eigenvectors as columns interpret rows as new data points.
5. Apply k-means to new data representation.

Output: cluster assignments.

The multiplicity of the eigenvalue 0 is the number of connected components in the graph, and the eigenvalues can be views as the cost of a cut.

![[Pasted image 20230103015059.png]]

High performance but not very efficient.



## L20 Collaborative Filtering

In a recommendation system, we want to predict a user's rating for a movie.

#### Matrix Factorization

![[20.1.PNG|500]]

**Idea**: construct low rank $\hat Y$ to approximate $Y$

![[Pasted image 20230103015532.png]]

![[20.2.PNG|550]]

$$
\begin{align}
\hat Y &= UV^T\\
\hat Y_{ai} &= \bar u^{(a)} \cdot \bar v^{(i)} \\ 
J(U,V) &= \frac{1}{2}\sum_{(a,i)\in D}(Y_{ai} - \bar u^{(a)} \cdot \bar v^{(i)})^2 + \frac{\lambda}{2}(\sum_a || \bar u^{(a)}||^2 + \sum_i || \bar v^{(i)}|| ^2)
\end{align}
$$

$U$ contains relevant features of the users, $V$ contains relevant features of the movie.

Minimize the lost function. *Coordinate descent?*

**Algorithm**

1. Repeat until converge:
	1. Fix V and solve U.
	2. Fix U and solve V.

#### Nearest Neighbor Prediction

We compute a prediction bases on $k$ users that are most similar to the target user.

##### Sample Pearson's Correlation

$$
\begin{align}
\tilde Y_{a:b} = \text{a's average rating of movies rated by both a and b} \\
sim(a, b) = \frac{\sum_{j \in R(a,b)}(Y_{a_j}-\tilde Y_{a:b})(Y_{b_j} - \tilde Y_{b:a})}{\sqrt {\sum_{j \in R(a,b)} (Y_{a_j} - \tilde Y_{a:b})^2 \cdot\sum_{j \in R(a,b)} (Y_{b_j} - \tilde Y_{b:a})^2}}
\end{align}
$$

Measure of linear relationship between two variables, ranging from \[-1, 1\].

$kNN(a, i)$: k nearest neighbors who have rated movie $i$.

$$
\hat Y_{a_i} = \bar Y_a + \frac{\sum_{b \in kNN(a, i)}sim(a,b)(Y_{b_i} - \bar Y_b)}{\sum_{b \in kNN(a, i)}|sim(a,b)|}
$$

**Choose k**

Neighbors with low correlation tend to introduce noise, and 20-50 is a reasonable range.



## L21 Generative Models

![[Pasted image 20230103160959.png]]

##### Discriminative

Learned a separator to discriminate (separate) two classes

##### Generative

Better understanding of where our data came from



## L22 MLE

**Assumption**

Data are generated i.i.d. from an unknow Bernoulli distribution with parameter $\theta$.

**Intuition**

Estimate parameter as the one that maximize the probability of observed outcome. We first write out the probability formulation, and then maximize it.

##### Gaussian Distribution

The MLE result is:

$$
\begin{align}
\mathcal{N}(x|\mu, \sigma^2) &= \frac{1}{\sqrt{2\pi\sigma^2}}\exp -\frac{(x - \mu)^2}{2\sigma^2}  \\
\mu_{MLE} &= \sum_{i = 1}^n \frac{x^{(i)}}{n}\\
\sigma^2_{MLE} &= \sum_{i = 1}^n\frac{(x^{(i)} - \mu_{MLE})^2}{n}
\end{align}
$$

##### Multivariate Gaussian Distribution

![[22.1.PNG]]

$$
\begin{align}
\Sigma &= E[(\bar x - \bar \mu) (\bar x - \bar \mu)^T]\\
|\Sigma| &= \det \Sigma
\end{align}
$$

$\Sigma$ measures the covariance between x.

##### Spherical Gaussian

A special kind of multivariate Gaussian assuming that variables are independent.

$$
\begin{align}
\Sigma &= \sigma ^2 \mathbf{I}_d\\
\bar{\mu}_{MLE} &= \sum_{i = 1}^n \frac{\bar x^{(i)}}{n}\\
\sigma^2_{MLE} &= \sum_{i = 1}^n\frac{||\bar x^{(i)} - \bar{\mu}_{MLE}||^2}{nd}
\end{align}
$$

##### Gaussian mixture model (GMM)

![[Pasted image 20230103164459.png]]

$$
\Pr (\bar x^{(i)}| \bar \theta) = \gamma_j N(\bar x | \bar \theta)\\
$$

$j$ is the corresponding cluster and $\gamma_j$ is the probability of that cluster.

MLE solution for known labels:

![[22.2.PNG|500]]



## L23 EM

![[Pasted image 20230103165750.png]]

**Unknown labels**

However, we don't have the labels! So our problem becomes: given the training data, find the model parameters that maximize the log-likelihood.

#### Expectation Maximization

Iterate over convergence:

1. **E step**: use current estimate of mixture model to softly assign data points to clusters
2. **M step**: re-estimate each cluster model based on the points assigned to it

##### E-Step

Fix $\gamma, \mu, \sigma^2$,

$$
p(j|i)=\frac{\gamma_j N(\bar x^{(i)} | \bar \mu_j, \sigma_j^2)}{\sum_t\gamma_t N(\bar x^{(i)} | \bar \mu_t, \sigma_t ^2)}
$$

##### M-Step

![[22.3.PNG]]

##### Bayesian Information Criterion (BIC)

How to pick k? Use BIC to penalize complex models.

l : log likelihood

$$
BIC(D;\bar \theta) = l(D;\bar \theta) - \frac{\#param}{2} \log(n)
$$



## L24 Bayesian Networks

![[Pasted image 20230103170857.png]]

* A probabilistic graphical model that represents a set of *variables* and their *dependencies* via a directed acyclic graph (DAG).
* Nodes: random variables
* Given its parent, a node $x$ is conditionally independent from all other nodes in the graph.
* Directed edges: dependencies

##### Marginal independence

$$
\begin{align}
\Pr (X_1, X_2) &= \Pr (X_1) \Pr (X_2) \\
\Pr(X_1) &= \Pr(X_1|X_2) \\
\end{align}
$$

##### Conditional Independence

$$
\begin{align}
X_1 \implies &X_3 \implies X_2 \\
X_1 &\perp X_2 | X_3 \\
\Pr(X_1, X_2,|X_3) &= \Pr(X_1|X_3)\Pr (X_2|X_3) \\
\Pr(X_2|X_3) &= \Pr(X_2|X_3, X_1)
\end{align}
$$

##### d-separation

1. Keep only "ancestral" graph of the variables of interest.
2. Connect nodes with common child and change graph into undirected.

If no path between variables of interest, then marginally independent.

If all path between variables of interest go through a particular *node*, then the variables are independent given that node (that node blocks the dependency).

#### Learning Bayesian Networks

**Estimate parameters**

Use MLE, probability based estimation.

**Find proper graph structure**

Use BIC.



## L25 HMM

![[Pasted image 20231202204851.png|500]]

* A special kind of [[Unsupervised Learning 445#L24 Bayesian Networks|Bayesian Network]] in which **future** depends on **past** via the **present**.
* $S_t$ are hidden states and $O_t$ are observations.
* Current observation only depends on current state (see from [[Unsupervised Learning 445#d-separation|d-separation]]), current state only depends on last state.
* E.g. observed data: words in a sentence; hidden state: POS tags.

$$
P(s_1, ..., s_T, o_1, ..., o_t) = \pi(s_1)\prod_{t=1}^{T-1}t(s_t, s_{t+1})\prod_{t=1}^T e(s_t, o_t)
$$

* $\pi_i = P(S_1 = i)$: initial state distribution
* $t_{ij} = P(S_{t+1}=j|S_t=i)$: transition matrix
* $e_{ik} = P(O_t  k|S_t = i)$: emission matrix
* But how to estimate these parameters to maximize the log-likelihood?

##### Forward Algorithm

![[Pasted image 20231202205840.png|600]]

* $\alpha_{i1} = \pi_i e_i(o_1)$
* $\alpha_{j, t+1} = \sum_{i=1}^n \alpha_{it}t_{ij}e_j(o_{t+1})$
* $P(o_1, o_2, \dots, o_T)=\sum_{i=1}^n \alpha_{iT}$

##### Viterbi Algorithm

![[Pasted image 20231202211047.png|500]]

* Infer the underlying hidden states.
* $l_{i1}^* = \log \pi_i + \log b_i(o_1)$
* $l_{j,t+1}^* = \max_i [l_{it}^* + \log t_{ij}] + \log b_j(o_{t+1})$
* We need another matrix for backtracking.
* $\Phi_{t+1}(j) = \arg\max_i[l_{it}^*+\log a_{ij}]$, we track where the max for each $l_{ij}^*$ comes from.
* $s_T^* = \arg \max_i[l_{iT}^*]$
* $s_t^* = \Phi_{t+1}(s_{t+1}^*)$

##### Parameter Estimation

* Goal: estimate $\pi, t, e$.

**Incomplete Data**

1. M step: given the hidden states and estimate the parameters
2. E step: fix the parameters and estimate the hidden states

