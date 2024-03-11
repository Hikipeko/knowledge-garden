#### 2.1 Affine and convex sets

* Lines and line segments
	* $\theta x_1 + (1-\theta)x_2, 0 \leq \theta \leq 1$
* Affine sets
	* Contain the line through any two distinct points in the set
	* E.g. $\{x\,|\, Ax=b\}$
* Affine dimension and relative interior
* Convex sets
	* Contian the line segment between any two points in the set
* Cones
	* $\theta_1 x_1 + \theta_2 x_2, \theta \geq 0$

#### 2.2. Some important examples

* Hyperplanes and halfspaces
	* $\{x \,|\, a^Tx = b\}$
	* $\{x \,|\, a^Tx \leq b\}$
* Euclidean balls and ellipsoids
	* $\{x\,|\,(x-x_c)^TP^{-1}(x-x_c)\leq 1\}, P \in \mathbf{S}_{++}^n$
* Norm balls and norm cones
	* $C = \{(x,t)\,|\, \|x\| \leq t \} \subseteq \mathbf{R}^{n+1}$
* Polyhedra
	* $\mathcal{P} = \{x\, | \, Ax \preceq b, Cx = d\}$
* Positive semidefinite cone
	* E.g. $\mathbf{S}_+^n$ 

#### 2.3 Operations that preserve convexity

* Intersection
	* The intersection of (any number of) convex set is convex
* Affine functions
	* The image of a convex set $S$ under affine function $f$ is convex
* Linear-fractional and perspective functions
	* $f(x) = (Ax+b)/(c^Tx + d), c^Tx + d > 0$
	* $P(x,t) = x/t, t>0$

#### 2.4 Generalized inequality

* Proper cones and generalized inqeualities
	* $K$ is a proper cone if it contains no line.
	* E.g. $\mathbf{R}_+^n$
	* Generalized inequality $x \preceq_K y \iff y - x \in \mathbf{int}K$
* Minimum and minimal elements
	* Minimum: "less" than any other points
	* Minimal: cannot find a "larger" point

#### 2.5 Separating hyperplane theorem

![[Pasted image 20240212163712.png|400]]

* If $C$ and $D$ are disjoint convex sets, then there exists a hyperplane separates $C$ and $D$.
* Supporting hyperplanes
	* Hyperplane that is "parallel" to the point.
	* If $C$ is convex, then there exists a supporting hyperplane at every boundary point of $C$.

#### 2.6 Dual cones and generalized inequality

![[Pasted image 20240212164447.png|450]]

* $K^* = \{y\,|\,y^T x \geq 0, \forall x \in K\}$
	* E.g. $K = \mathbf{R}_+^n = K^*$
* Dual generalized inequality
* Optimal production frontier
