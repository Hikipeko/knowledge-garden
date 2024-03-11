#### 3.1 Basic properties and examples

* Definition
	* $f$ is convex if $\mathbf{dom} f$ is a convex set and $f(\theta x + (1-\theta)y) \leq \theta f(x) + (1-\theta) f(y)$ for all $x, y \in \mathbf{dom}f, 0 \leq \theta \leq 1$.
	* $x\log x$ on $\mathbf{R}_{++}$
	* affine function $f(x) = a^Tx + b$
	* $l_p$ norms for $p \geq 1$
	* affine function $f(X) = \sum A_{ij} X_{ij} + b$
	* spectral norm: maximum of singular values
* Restriction of a convex function to a line
	* $f$ is convex if and only if the function $g(t) = f(x+tv)$ with proper domain is convex for any $x \in \mathbf{dom} f, v \in \mathbf{R}^n$.
	* Useful for checking/proving convexity
* Extended-value extensions
	* $\tilde f(x) = f(x),\, x \in \mathbf{dom} f, \quad \tilde f(x) = \infty, \,x \notin \mathbf{dom} f$
* ==First-order conditions==
	* Differentiable $f$ with convex domain is convex iff $f(y) \geq f(x) + \nabla f(x)^T(y-x) , \forall x, y \in \mathbf{dom} f$
	* First order approximation is a global underestimator.
* ==Second-order conditions==
	* $f$ is twice differentiable if $\mathbf{dom} f$ is open and the Hessian $\nabla^2f(x) \in \mathbf{S}^n$ exists at each $x \in \mathbf{dom}f$.
	* Twice differentiable $f$ with convex domain is convex iff $\nabla^2f(x) \succeq 0$ for all $x \in \mathbf{dom}f$
* Examples
	* Exponential, powers, minus logarithm, norms, max
	* quadratic: $f(x) = 1/2x^TPx + q^Tx + r, J = Px+q, H = P$
	* log-sum-exp $f(x) = \log \sum \exp x_k$
	* geometric mean $f(x) = (\prod x_k)^{1/n}$
	* Log-determinant $f(X) = \log \det X$
* Sublevel sets
	* $C_\alpha = \{x \in \mathbf{dom} f \,|\, f(x) \leq \alpha\}$
* Epigraph
	* The set above the function.
	* $f$ is convex iff the epigraph is a convex set.
* Jensen's inequality and extensions
	* $f(\mathbf{E}[z]) \leq \mathbf{E}[f(z)]$
* Inequalities
	* $\sqrt{ab} \leq (a+b) /2$, since $-\log$ is convex
	* Holder’s inequality

#### 3.2 Operations that preserve convexity

* Nonnegative weighted sums
* Composition with an affine mapping
	* $f(x) = \|Ax+b\|$
* Pointwise maximum and supremum
	* $f(x) = \max\{f_1(x), \dots, f_m(x)\}, f_i$ is convex extends to 
	* $g(x) = \sup_{y \in A} f(x, y), f(x,y)$ is convex in $x$ for each $h \in A$
	* E.g. distance to farthest point in a set $C$ $f(x) = \sup_{y \in C} \|x - y\|$
	* maximum eigenvalue of symmetric matrix $\lambda_{\max}(X) = \sup_{\|y\|_2 = 1}y^TXy$
* Composition
	* $f(x) = h(g(x)) = h(g_1(x), \dots, g_k(x)$
	* Scalar composition extends to
	* Vector composition
	* E.g. $\log \sum \exp g_i(x)$ if $g_i$ are convex.
* Minimization
	* if $f(x,y)$ is convex in $(x,y)$ and $C$ is a convex set, then $g(x) = \inf_{y \in C}f(x,y)$ is convex.
	* E.g. distance to a convex set
* Perspective of a function
	* $g(x, t) = t f(x/t)$ is convex if $f$ is convex.

#### 3.3 The conjugate function

![[Pasted image 20240212212738.png|500]]

* $f^*(y) = \sum_{x \in \mathbf{dom}f}(y^Tx-f(x))$
* $f^*$ is convex (regardless of $f$)

#### 3.4 Quasiconvex functions

![[Pasted image 20240212224405.png|500]]

* Definition and examples
	* $f$ is quasiconvex if $\mathbf{dom}f$ is convex and the sublevel sets are convex for all $\alpha$.
	* $f$ is quasilinear if it is quasiconvex and quasiconcave.
	* ceil is quasilinear
* Basic properties
	* $f(\theta x + (1-\theta)y) \leq \max\{f(x), f(y)\}$
* Differentiable quasiconvex functions
	* First-order conditions: Differentiable $f$ with convex domain is quasiconvex iff $f(y) \leq f(x) \implies \nabla f(x)^T(y-x) \leq 0, \forall x, y \in \mathbf{dom} f$
	* The hyperspace defined by $-\nabla f(x)$ at $x$ is smaller than $f(x)$

#### 3.5 Log-concave and log-convex functions

* Definition
	* A positive function $f$ is log-concave if $\log f$ is concave: $f(\theta x + (1-\theta)x) \geq f(x) ^\theta f(y)^{1-\theta}$
	* E.g. Gaussian distribution and many other common probability density function
* Properties
	* Twice differentiable $f$ with convex domain is log-concave iff $f(x)\nabla^2f(x) \preceq \nabla f(x) \nabla f(x)^T$ for all $x \in \mathbf{dom}f$

#### 3.6 Convexity w.r.t. generalized inequalities

