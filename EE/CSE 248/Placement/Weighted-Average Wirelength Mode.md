In VLSI placement, we estimate the length of a net as half-perimeter wirelength (HPWL). However, HPWL is not smooth and non-convex, we want to find a convex approximation function.

$$
W_e(V) = \max_{i,j\in e}|x_i - x_j| + |y_i - u_j|
$$

To simulate the absolute value, we want a weight function growing fast to separate larger values from smaller ones. $F(x_i) = \exp(x_i / \lambda)$ is used, where $\lambda$ is close to 0. $|x_i - x_j|$ is approximate by

$$
\frac{\sum_{i,j\in e}x_i \exp(x_i/\lambda)}{\sum_{i,j\in e}\exp(x_i/\lambda)} - \frac{\sum_{i,j\in e}-x_i \exp(-x_i/\lambda)}{\sum_{i,j\in e}\exp(-x_i/\lambda)}
$$

* **Lemma 1** The WA wirelength model is strictly convex and continuously differentiable.
* **Theorem 1** Let $\epsilon_{WA}(\mathbf{x}_e)$ be the estimation error of the WA wirelength model w.r.t. the $x$ coordinate, we have

$$
0 \leq \epsilon_{WA}(\mathbf{x}_e) \leq \frac{\gamma \Delta x}{1+\exp(\Delta x)/n}
$$