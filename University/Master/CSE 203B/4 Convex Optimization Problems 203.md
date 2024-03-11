#### 4.1 Optimization Problems

![[Pasted image 20240212230348.png|600]]

* optimal $p^* = \inf \{f_0(x)\,|\, x\text{ is feasible}\}$ 
* $X_{\text{opt}}$ is the set of optimal points.
* E.g. for $f_0(x) = 1/x, x > 0, p^* = 0, X_{\text{opt}} = \emptyset$
* Implicit constraints are the constraints implied by $\mathbf{dom}f_i$ and $\mathbf{dom}h_i$.
* Feasible problem is to find $x$ that satisfy the explicit constraints.

#### 4.2 Convex optimization

![[Pasted image 20240212232433.png|500]]

* Standard form
	* $f_0, f_1, \dots, f_m$ are convex, equality constraints are affine, i.e. $Ax=b$.
	* E.g. if $h$ is $x_1^2 + x_2^2 = 0$, the problem is not convex optimization. It is equivalent to a convex optimization problem with $x_1=0, x_2 = 0$.
* ==Local and global optima==
	* Any local minimal point of a convex problem is globally optimal.
* An optimality criterion for differentiable $f_0$
	* $x$ is optimal iff it is feasible and $\nabla f_0(x)(y-x) \geq 0$ for all feasible $y$.
	* Either a stationary point, or no points in the hyperplane defined by $x$ and $-\nabla f_0(x)$.
* Equivalent convex problems
	* Eliminating equality constraints
	* Introducing equality constraints
	* Introducing slack variables
		* change $a^Tx \leq b$ to $a^Tx + s =\leq= b, s \geq 0$.
	* Epigraph form
		* minimize $t$ subject to $f_0(x) -t \leq 0$
		* We can assume the objective function to be linear for all convex problem
	* Minimizing over some variable
* Quasiconvex optimization
	* If $f_0$ is quasiconvex, there exists a family of function $\phi_t$ such that $\phi_t(x)$ is convex for fixed $t$ and $f_0(x) \leq t \iff \phi(x) \leq 0$
	* Quasiconvex optimization via convex feasibility problems
		* Use $\phi_t(x)$ as additional condition to form feasibility problems. Use binary search to approximate $p^* \approx t$

#### 4.3 Linear optimization problems

Minimize $c^Tx+d$ subject to $Gx\succeq h, Ax=b$.

Feasible set is a polyhedron.

* Examples
	* Diet problem
	* Piecewise-linear minimization
		* minimize $\max_i (a_i^Tx+b_i)$
		* equivalent to minimize $t, a_i^Tx+b_i\leq t$
	* Chebyshev center of a polyhedron
		* maximize $f$
		* subject to $a_i^T + r\|a_i\|_2 \leq b_i$
	* 



