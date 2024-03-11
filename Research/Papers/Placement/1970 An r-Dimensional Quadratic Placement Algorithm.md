---
year: 1970
author: KM Hall
citation: 657
---
### Abstract

* Give the solution to the problem of placing $n$ connected points in $r$-dimensional Euclidean space.
* Minimize a weighted sum of squared distance between points subject to quadratic constraints of the form $X'X = 1$, where $X$ is a $r$-dimensional vector.
* Reduce the problem to finding $r$ eigenvectors of a special disconnection matrix.
* Minimize $z = \frac12\sum_{i,j}(x_i - x_j) ^2 c_{ij}, \; X'X = 1$

### 2 Optimum Solution in 1-Dimension

* $B = D - C$, the [[Unsupervised Learning 445#Graph Laplacian|graph Laplacian]].
* $z = X'BX$.
* **Theorem 1** $B$ is positive semi-definite (obviously), and whenever $G$ is connected, $B$ is of rank $n - 1$.
* Take the partial derivative of $L = X'BX - \lambda(X'X - 1)$, we obtain $BX = \lambda X$. The solutions are [[MATH 214#7 Eigenvalues and Eigenvectors|eigenvectors]], the corresponding $\lambda = X'BX$ is the corresponding weighted sum of squared distance.

### 3 Extension to 2-Dimensions

* We want to optimize $z = X'BX + Y'BY, \quad X'X = 1, Y'Y = 1$.
* Yields nontrivial solutions $X$ and $Y$ if and only if $X$ and $Y$ are eigenvectors of $B$.



### Spectral Graph Theory Resources

Resource: Spielman book "Spectral Algebraic Graph Theory"

#### The Quadratic Form

* Undirected finite graph $G = (V,E)$
* Allow multiple parallel edges and self-loops
* Disallow vertices of degree 0
* Assume graph is regular (each vertex has same number of neighbors)
* Object: we want to label each $v$ by real numbers (or column vectors)
* $\{f: V \to \mathbb{R}\}$ can be viewed as a vector space, with dimension $|V|$.

##### Key to Spectral Graph Theory

$$
\mathcal{E}(f) = \frac12 \mathbb{E}_{(u,v)\sim E}(f(u)-f(v))^2 
$$

* Laplacian **quadratic form** (Dirichlet form, Local variance, Analytic boundary size)

#### The Standard Random Walk

* Let $t \in \mathbb{N}$, draw $u \sim \Pi$, a distribution for vertices proportional to $deg(u)$, do "standard random walk", which is walk to a randomly chosen neighbor, the probability distribution will always be $\Pi$.
* $\Pi$ is called the invariant/stationary distribution.
* Fast convergence is equivalent to a never being small $\mathcal{E} (f)$

https://web.stanford.edu/class/cs168/l/l10.pdf

Let $L$ be the Laplacian matrix associated with $G$, we have

$$
v^TLv = \sum_{i < j}w_{ij}(v(i) - v(j))^2
$$
