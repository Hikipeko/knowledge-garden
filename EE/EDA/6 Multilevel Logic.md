#### 6.1 Multilevel Logic and the Boolean Network Model

![[Pasted image 20230718114253.png|400]]

* 2-level forms are too restrictive
* **Area** = gates + literals (wires)
* **Delay** = maximum levels of logic gates required to compute function

##### Boolean Logic Network Model

![[Pasted image 20230718114534.png|300]]

* A graph of connected blocks
* Individual component blocks can be 2-level Boolean function in SOP form
* Optimization goal: total literal count
* Optimization operators:
	* **Simplify**: simplify inside each nodes, e.g. using ESPRESSO
	* **Remove**: take too small nodes and push them back into its fan-outs (output nodes)
	* **Add**: factoring, identify common divisors, extract them, connect as fan-ins.

![[Pasted image 20230718115103.png|500]]

![[Pasted image 20230718115204.png|300]]

#### 6.2 Algebraic Model for Factoring

* Algebraic model pretends that Boolean expressions behave like polynomials of real numbers.
* A variable and its complements must be treated as totally unrelated.
* **Algebraic Division**: $F = D \cdot Q + R$, **divisor**, **quotient**, and **remainder**.
* **Factor**: quotient with remainder = 0.

#### 6.3 Algebraic Division

* How to divide?
* $F, D$ are represented as lists of cubes.
* $F$ must have no redundant cubes (think of $F = a + ab, D = a$)

![[Pasted image 20230718120919.png|500]]

![[Pasted image 20230718121109.png|550]]

#### 6.4 Kernels and Co-Kernels in Factoring

![[Pasted image 20230718122236.png|450]]

* How to find good divisors for set of n functions? In the **kernels** of $F$.
* **Kernel** is a **cube-free** quotient $k$ obtained by dividing $F$ by a single cube c.
* **Cube-free**: cannot factor out a single cube divisor that leaves no remainder. (e.g. a = a(1) + 0, ab + ac are not cube-free).
* $c$ is a **co-kernel** of function $F$.

##### Brayton & McMullen Theorem

* Expression $F, G$ has a common multiple-cube divisor $d$ iff there are kernels $k_1 \in K(F), k2 \in K(G)$ s.t. $d = k1 \cap k2$ is an expression with at least 2 cubes in it.
* In other words, the only way to look for divisors is in the ==intersection of kernels==.

![[Pasted image 20230718122613.png|450]]

#### 6.5 Finding the Kernels

![[Pasted image 20230718122937.png|300]]

* Do it recursively.
* Kernel's kernel is also a kernel.
* How to find potential co-kernels? Co-kernels of a Boolean expression in SOP form correspond to ==intersection of 2 or more of the cubes== in this constituent SOP form. E.g. $ace \cap bce = ce$.
* If $F$ is not cube-free, just divide by the biggest common cube.
* We may get duplicate kernels during the recursion.

![[Pasted image 20230718123606.png|500]]

![[Pasted image 20230718123648.png|500]]
