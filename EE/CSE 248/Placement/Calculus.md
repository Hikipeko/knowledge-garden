### Vector Space

> Vv186 p331

A tripe $(V, +, \cdot)$ is called a real vector space if

1. $V$ is any set
2. $+: V \times V \to V$ is a map with the following properties:
	* $(u+v) + w = u+(v+w), \, \forall u,v,w \in V$ (associativity)
	* $u+v=v+u,\, \forall u,v \in V$ (commutativity)
	* These exists an element $e\in V$ s.t. $v+e = v, \, \forall v \in V$ (identity element)
	* For every $v\in V, \, \exists -v \text{ s.t. } v+(-v)=e$ (inverse element)
3. $\cdot:\mathbb{R}\times V \to V$ is a scalar multiplication with the following properties:
	* $1\cdot u = u, \, \forall u \in V$
	* $\lambda \cdot (u+v) = \lambda \cdot u + \lambda \cdot v, \, \forall \lambda \in \mathbb{R}, u, v \in V$
	* $(\lambda+\mu) \cdot u = \lambda \cdot u + \mu \cdot u, \, \forall \lambda,\mu \in \mathbb{R}, u\in V$
	* $(\lambda \mu)\cdot u = \lambda \cdot (\mu \cdot u), \, \forall \lambda ,\mu\in \mathbb{R}, u\in V$

#### Examples

* For any set $M$, the set of maps $f: M \to \mathbb R(\mathbb C)$.
* For any open set $\Omega \subset \mathbb{R}$, the set of all continuous functions, denoted as $C(\Omega)$.
* For any open set $\Omega \subset \mathbb{R}$ and $k\in\mathbb{N}$ the set of all $k$ times continuously differentiable function, denoted as $C^k(\Omega)$.

### Continuously Differentiable

* A function whose derivative is also continuous is called continuously differentiable.
* If $f$ is differentiable at $x$, $f'$ is not necessarily continuous at $x$.

![[Pasted image 20231029165350.png|500]]