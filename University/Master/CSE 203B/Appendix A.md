#### A.1 Norms

* Inner product
* Euclidean norm, or $l_2$-norm
* $\langle X, Y\rangle = \mathbf{tr}(X^T Y)$
* Distance $\mathbf{dist}(x,y) = \|x-y\|$
* Unit ball
* $\|x\| = (\text{sup}\{ t\geq 0 \,|\,tx \in C\})^{-1}$
* Equivalence of norms
* Operator norm of $X\in \mathbf{R}^{m\times n}$ induced by the norms $\|\cdot\|_a$ and $\|\cdot\|_b$ as

$$
\|X\|_{a,b} = \text{sup}\{ \|Xu\|_a \,|\, \|u\|_b \leq a  \}
$$
* Dual norm $\|\cdot\|_*$ is defined as

$$
\|z\|_* = \text{sup}\{ z^Tx \,|\, \|x\| \leq 1 \}.
$$

* The dual of the $l_p$-norm is the $l_q$-norm, where $1/p + 1/q = 1$.

#### A.2 Analysis

* Interior point
* Interior of set $C$ $\mathbf{int}\,C$
* Open if $\mathbf{int} \, C = C$
* Closed
* Closure $\mathbf{cl}\, C = \mathbb{R}^n \backslash \mathbf{int}(\mathbb{R}^n\backslash C)$
* Boundary $\mathbf{bd} \, C = \mathbf{cl}\, C \backslash \mathbf{int}\, C$
* Boundary point
* Supremum (sup) is the least upper bound
* Infimum (inf) is the greatest lower bound

#### A.3 Functions

* Continuity
* Closed. A function $f: \mathbf{R}^n \to \mathbf{R}$ is closed if for each $\alpha \in \mathbf{R}$, the sublevel set $\{x\in \mathbf{dom} \, f \, |\, f(x) \leq \alpha\}$ is closed.
* If $f: \mathbf{R}^n \to \mathbf{R}$ is continuous and its domain space is closed, then $f$ is closed.

#### A.4 Derivatives

* Differentiable
