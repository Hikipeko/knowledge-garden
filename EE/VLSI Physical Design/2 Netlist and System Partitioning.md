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
* See concrete example from P37

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

#### 2.4.2 Extensions of the KL Algorithm

* What about unequal partition size? Split initially according to the desired partition size.
* Unequal node area? Cast large node into multiplies of one with the unit area, adding an infinite weight.
* What about k-way partitioning? Split to k partition initially, and perform KL algorithm to all possible pairs until convergency.

#### 2.4.3 Fiduccia-Mattheyses Algorithm

* Substantial improvement over the KL algorithm
* Single cells are moved each time
* Cut cost extends to hypergraphs: based on nets
* Complexity is $O(|\text{Pins}|)$ per pass
* $\Delta g(c) = \text{FS}(c) - \text{TE}(c)$
* $FS(c)$ is the moving force, the number of nets connected to $c$ to not to connected to any other cells within $c$'s partition
* $TE(c)$ is the retention force, the number of uncut nets connected to $c$
* **Ratio factor** $r = area(A) / (area(A) + area(B))$ prevent all cells from clustering into one partition
* After a move, only cells belonging to critical nets need to be considered in the gain computation.
* See concrete example from P44

```algorithm
Fiduccia-Matheyes Algorithm
Input: graph G(V, E), ratio factor r
Output: partitioned graph G(V, E)

(lb, up) = Balance-Criterion(G, r)
(A, B) = Initial-Partition(G, lb, ub)
Gm = infinity
while (Gm > 0)
	i = 1
	order = Empty
	foreach (cell c in V)
		delta_g[c] = FS(c) - TE(c)
		status[c] = FREE
	while (!All-Fixed(V))
		cell = Max-Gain(delta_g[i], lb, ub)
		// the cell with max gain and statisfy the balance criterion
		Add(order, (cell, delta_g[i]))
		cirtical_nets = Critical-Nets(cell)
		Try-Move(cell, G)
		status[cell] = FIXED
		foreach (net n in critical_nets)
			foreach (cell c in nets, c != c)
				delta_g[c] = Update-Gain(net, c)
	(Gm, Cm) = Best-Moves(order)
	if (Gm > 0)
		Execute-Moves(Cm)
```

### 2.5 A Framework for Multilevel Partitioning

* FM algorithm work good for about a hundred gates
* Use multilevel framework to solve larger problems:
	1. **Coarsening** phase clusters the original netlist (graph) hierarchically into smaller graphs
	2. Apply FM to the clustered netlist.
	3. **Uncoarsening** the cluster
	4. **Refinement** phase, FM in incrementally applied to partially unclustered netlist

![[Pasted image 20231004170325.png|500]]

#### 2.5.1 Clustering

Groups of tightly connected nodes can be clustered.

#### 2.5.2 Multilevel Partitioning

* The **clustering ration** $r$ is often 1.3.
* Number of level can be estimated as $\lceil \log_r(|V| /v_0)\rceil$
* In practice, $v_0$ is typically 75-200
