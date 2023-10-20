### Linear Programming

* Find a real variables vector $x$
* that maximize $c^T x$
* subject to $Ax \leq b$
* and $x \geq 0$.

**Three possibilities**

* The feasible set is empty
* The objective is unbounded
* It has a feasible maximum solution

How to show upper bound? By LP duality

#### Duality

* Find a real variables vector $y$
* that minimize $b^T y$
* subject to $A^Ty \geq c$
* and $y \geq 0$.

##### Strong Duality Theorem

* If Opt(Prime) exists, Opt(Dual) exists, then Opt(Prime) = Opt(Dual).
* Weak Duality Theorem. If Opt(Prime) exists, Opt(Dual) exists, then Opt(Prime) $\leq$ Opt(Dual).

![[Pasted image 20231019122958.png|500]]