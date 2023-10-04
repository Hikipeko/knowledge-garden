### 2.1 Introduction

* A popular approach to decrease the design complexity is to partition IC into smaller modules.
* A large number of connections between partitions may introduce inter-block dependencies.
* The primary goal of partitioning is to ==divide the circuits into partitions such that the connections between subcircuits is minimized.==

### 2.2 Terminology

* *K-way partitioning* problem seeks to divide a circuit into $k$ partitions.
* Abstracted using a graph representation
* *area(v)* represents the area of module/cell
* *w(e)* represent the priority or weight of an edge
* *disjoint*, *cut*

### 2.3 Optimization Goals

* Minimize the total weight of the cut while balancing the sizes of partitions
* *Bounded-size partitioning* enforce an upper bound $area(V_i) \leq UB_i$. Choose $UB = \lceil \frac{|V|}{k} \rceil$ to get a balanced partitioning.

### 2.4 Partitioning Algorithms

#### 2.4.1 Kernighan-Lin (KL) Algorithm

* Partition $V$ into two disjoint subsets $A$ and $B$ with $|A| = |B| = n$.
* The benefit of moving a node $v$ from $A$ to $B$ is $D(v) = |E_B(v)| - |E_A(v)|$, where $E_A(v)$ is the number of edges $(v, u)$ where $u \in A$.
* The gain of switching two nodes is $\Delta g(a, b) = D(a) + D(b) - 2c(a, b)$
* The maximum positive gain $G_m$ corresponds to the best prefix of $m$ swaps within the swap sequence of a given pass.
* Time complexity: $O(n^2\log n)$
* Question: can we do swap in a stochastic way?

```algorithm
Input: graph G(V,E) with |V| = 2n
Output: partitioned graph G(V,E)

(A, B) = Initial-Partition(G) // arbigrary
Gm = infinity
while (Gm > 0)
	i = 1
	Order = empty set
	foreach (node v in V)
		status[v] = FREE
		D[v] = Benefit(v)
	while (!All-Fixed(V))
		delta_gi, ai, bi = Max-Gain(A, B) // get the pair with max gain
		Add(Order, delta_gi, ai, bi)
		Try-Swap(ai, bi)
		status[ai] = status[bi] = FIXED
		foreach (free node v connected to ai or bi)
			D[v] = Benefit(v) // update benefit
		i = i + 1
	(Gm, m) = Best-Moves(Order) // swap sequence <1 ... m> that maximize Gm

	if (Gm > 0)
		Execute-moves(m)	
```
