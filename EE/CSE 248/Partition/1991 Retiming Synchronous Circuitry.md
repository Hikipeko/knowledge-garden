---
year: 1991
author: CE Leiserson, JB Saxe
citation: 1369
---
### Abstract

* A circuit transformation by relocating registers, while preserving the functional behavior.
* Transform synchronous circuit into a more efficient one.
* $V$ as combinational logic elements and $E$ as interconnections.
* $O(|V||E|\log|V|)$ algorithm determining an equivalent retimed circuit with ==optimal== clock period.

### 1 Introduction

![[Pasted image 20231016162939.png|600]]

### 2 Preliminaries

![[Pasted image 20231015122918.png|500]]

* View a circuit as a network of functional elements and globally clocked registers.
* Model a circuit as a finite, vertex-weighted, edge-weighted, directed graph $G = \langle V, E, d, w \rangle$
* Each vertex (functional element) has an associated propagation delay $d(v)$.
* Each edge is a connection, weighted with a register count $w(e)$.
* An edge $e$ goes from $u$ to $v$ is represented as $u \xrightarrow{e} v$
* A path $p$ from $u$ to $v$ is represented as $u \twoheadrightarrow v$
* Define **path weight** as sum of edge weights $w(p) = \sum_{i=0}^{k-1}w(e_i)$.
* Define **path delay** as sum of vertex delays $d(p) = \sum_{i=0}^{k} d(v_i)$.
* **Synchronous circuit**
	* **D1**. $d(v)$ is nonnegative
	* **W1**. $w(e)$ is nonnegative
	* **W2**. No directed cycles of zero weight

##### Clock period

The maximum amount of propagation delay through which any signal must ripple between clock ticks.

$$\Phi(G) = \max\{d(p):w(p) = 0\}$$

##### Algorithm CP

* Computes the clock period of a circuit
* Runtime is $O(|E|)$

1. Let $G_0$ be the subgraph of $G$ that contains precisely those edges $e$ with $w(e) = 0$.
2. Perform a topological sort on $G_0$.
3. Go through the order, calculate $\Delta(v) \leftarrow d(v) + \max\{\Delta(u): u\to v\}$
4. The clock period is $\Phi(G) = \max_{v\in V} \Delta(v)$
### 3 Retiming

* Assignment of a lag to each vertex in a circuit.
* An integer-valued vertex-labeling $r: V \to \mathbb{Z}$, changes $G$ to $G_r$ with new edge-weighting $w_r(e) = w(e) +r(v) - r(u)$.
* **Lemma 1**. For any path in $G$, we have $w_r(p) = w(p) + r(v) - r(u)$.
* **Corollary 2**. For any cycle $p$, we have $w_r(p) = w(p)$.
* A retiming $r$ is **legal** is $G_r$ satisfies condition W1 and W2.
* **Corollary 3**. Satisfying W1 implies satisfying W2.
* $G$ and $G_r$ are functionally equivalent.
* Retiming is the most general possible method for changing the register counts without disturbing the circuits' function.

### 4 An Algorithm for Clock Period Minimization

* Goal: find a legal retiming s.t. $\Phi(G_r)$ is minimized.
* Rely on the solution of LP problem.

##### Problem LP

* Let $S$ be a set of $m$ linear inequalities of the form $x_j - x_i \leq a_{i,j}$, determine feasible values for the unknowns $x_i$, or determine that no such values exist.
* This constraint systems arise in SSSP, and can be solved by Bellman-Ford in $O(mn)$ time. ==Why?==
* https://www.youtube.com/watch?v=Z7l-sdWIIeg
* Create a vertex for each $x$.
* For $x_j - x_i \leq a_{i,j}$, create an edge $(v_i, v_j)$ with weight $a_{i,j}$.
* Create a dummy node $v_0$ with $w(v_0, v_i) = 0$ for all the other vertices.
* If a negative loop is detected, then it's not feasible to find a solution.

##### W and D

![[111.png]]

* $W(u, v) = \min \{w(p): u \twoheadrightarrow v\}$, the minimum number of registers on any path from $u$ to $v$.
* A path s.t. $w(p) = W(u, v)$ is called a **critical path**.
* $D(u, v) = \max\{d(p): u\twoheadrightarrow v \land w(p) = W(u,v)\}$ is the maximum of delay on any critical path from $u$ to $v$.
* **Lemma 4**. (1) $\Phi(G) \leq c$ is equivalent to (2) For all vertices $u$ and $v$ in V, if $D(u, v) > c$, then $W(u, v) \geq 1$.
* $W$ can be computed by solving the all-pairs shortest-paths problem in $G$ (ASSP), e.g. using Floyd-Warshall methods in $O(|V|^3)$.
* **Algorithm WD**
	1. Weight each edge $u\xrightarrow{e}?$ with ordered pair $(w(e), -d(u))$.
	2. Compute the weight of shortest path joining each connected pair of vertices by solving an ASSP.
	3. For each pair $u, v$ with resulting weight $(x, y)$ set $W(u, v) \leftarrow x, D(u, v) \leftarrow d(v) - y$.
* **Lemma 5**. Let $W, D$ be defined on $G$, $r$ is a legal retiming from $G$ to $G_r$, $W_r, D_r$ are defined on $G_r$, we have
	* a critical path in $G$ is also a critical path in $G_r$, and vice versa
	* $W_r(u,v) = W(u,v) + r(v) - r(u)$ for all connected $u, v$, and
	* $D_r(u, v) = D(u, v)$ for all connected $u, v \in V$.
* **Corollary 6**. Let $r$ be a retiming, then the clock period $\Phi(G_r)$ is equal to $D(u,v)$ for some $u, v$.

##### Retiming Algorithm

**Theorem 7**. Let $c$ be an arbitrary positive real number, and $r$ be a retiming, then $r$ is a legal retiming s.t. $\Phi(G_r) \leq c$ if and only if

* 7.1 $\forall e\in E, r(u) - r(v) \leq w(e)$, no negative weighted edge.
* 7.2 $\forall u,v \in V,\;D(u,v) > c \implies r(u) -r(v) \leq W(u,v)-1$, clock period less than $c$.

**Algorithm OPT1**

1. Compute $W$ and $D$.
2. Sort the elements in the range of $D$.
3. Binary search among the elements $D(u,v)$ for the minimum achievable clock period by applying the Bellman-Ford algorithm to determine whether the conditions in Theorem 7 can be satisfied.
4. For the clock period from 3, use the values for $r(v)$ as optimal retiming.

**Complexity**

* Variables: we have $|V|$ variables $r$
* For 7.1, we have $|E|$ constraints
* For 7.2, we have $|V|^2$ constraints
* The graph we construct has $|V|$ vertices and $|E|+|V|^2$ edges
* For each run of Bellman-Ford, the time complexity is $O(|V|^3)$
* The total complexity is $O(|V|^3 \log |V|)$

### 5 A More Efficient Algorithm for Clock-Period Minimization

* For Algorithm OPT1 step 3, we can optimize it to $O(|V||E|)$.
* This will improve Algorithm OPT1 to $O(|V||E|\log|V|)$

**Algorithm FEAS** feasible clock-period test

1. For each vertex $v\in V$, set $r(v) \leftarrow 0$.
2. Repeat for $|V| - 1$ times:
	1. Compute graph $G_r$ with the existing values for $r$.
	2. Run [[1991 Retiming Synchronous Circuitry#Algorithm CP|Algorithm CP]] on graph $G_r$ to determine $\Delta(v)$ for each vertex $v\in V$.
	3. For each $v$ s.t. $\Delta(v) > c$, set $r(v) \leftarrow r(v) + 1$.
3. Run Algorithm CP o circuit $G$. If $\Phi(G_r) > c$, then no feasible retiming.
