`* Input: A directed graph $G = (V,E,c)$
* Problem: the rate that material can "flow" from $s$ to $t$.

#### Definition  of Flow

f: $V\times V \to \mathbb{Z}^+$ is a legal flow if it satisfies:

* Capacity constraint: $0 \leq f(u,v)\leq c(u,v)$
* Flow conservation: for all $u \in V - \{s,t\}, \sum_{(x,u)\in E}f(x,u) = \sum_{(u,x)\in E}f(u,x)$

#### Ford-Fulkerson Method

* Start with initial flow $f(u,v) = 0$ for all $u,v$ ($G_f = G$)
* Repeat: (how many times?)
	* Find a path from $s$ to $t$ in $G_f$ (DFS)
	* $f':=$ maximum legal flow along $P$
	* $f := f + f'$ (does the adding make sense?)
* Repeat until no such path can be found in $G_f$ (is $f$ a maximum flow?)
* Complexity: $O(m\cdot v(f))$

##### Residual Network

* A flow $f$ in $G$ defines a residual network $G_f$
* $c_f(u,v)$ represents the amount of additional flow that could be sent through $(u,v)$ without violating the capacity constraint.
* $c_f(u,v) = c(u,v) - f(u,v)$
* Only edge with positive capacity appear in $G_f$

#### s-t Cuts

* $(A,B)$ is an **s-t cut** if $s \in A, t \in B, A \cup B = V$
* $c(A,B)$ = total capacity of edges from $A$ to $B$
* For any flow $f$ and s-t cut $(A,B)$, $v(f) \leq c(A,B)$

##### Max-Flow Min-Cut Theorem

The following are equivalent:

1. $f$ is a maximum flow in $G$
2. There is no path form $s$ to $t$ in $G_f$
3. $v(f) = c(A,B)$ for some s-t cut $(A,B)$


#### Dinitz's Max-Flow Algorithm

1. $f := 0$
2. While ($f$ is not a maximum flow)
	1. Let $G_f$ be the residual network for $G$ w.r.t. f.
	2. Let $G' = (V, E')$, where $E' \subseteq E(G_f)$ are those edges in shorted paths from $s$ in $G_f$. (BFS)
	3. $f' :=$ any blocking flow in $G'$
	4. $f := f + f'$

##### Blocking Flows

* An edge is **saturated** by $f$ if $f(e) = c(e)$
* A flow $f$ in $G$ is called a **blocking flow** if every path from $s$ to $t$ in $G$ contains a saturated edge.
* Computation of blocking flow is $O(|V||E|)$
* **Lemma**: In every iteration of the while loop of Dinitz's algorithm, $\text{dist}_{G_f}(s,t)$ increases by at least 1.
* **Corollary**: The algorithm computes at most $n-1$ blocking flows.
