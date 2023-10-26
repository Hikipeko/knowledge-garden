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

### 10: Tensors, and Low-Rank Tensor Recovery

https://web.stanford.edu/class/cs168/l/l10.pdf

Let $L$ be the Laplacian matrix associated with $G$, we have

$$
v^TLv = \sum_{i < j}w_{ij}(v(i) - v(j))^2
$$


