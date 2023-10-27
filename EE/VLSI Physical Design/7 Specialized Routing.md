Some types of design, e.g. analog circuits and printed circuit boards cannot warrant the separation into global and detailed routing.

### 7.1 Area Routing

#### 7.1.1 Introduction

**Goals** is to route all the nets in the design

* without global routing
* within the given layout space
* meeting all geometric and electrical design rules
* minimize total length and number of vias
* minimize total area of wiring and number of routing layers
* minimize circuit delay
* avoid harmful capacitive coupling

#### 7.1.2 Net Ordering

* **MBB** is the minimum bounding box
* **AR** is the aspect ratio, $\max(W/L, L/W)$
* **Rule 1** Route large AR first.
* **Rule 2** For two nets, the one whose pins are contained in another nets' MBB is routed first.
* **Rule 3** Route based on the number of pins inside MBB, from small to large.

![[Pasted image 20231023160501.png|500]]
![[Pasted image 20231023160514.png|500]]

### 7.2 Non-Manhattan Routing

Octilinear Steiner Tree Algorithm generalize rectilinear Steiner trees by allowing segments that extend in eight directions.

### 7.3 Clock Routing

Once the clock signal entry points and sinks are known, **clock tree routing** generates a clock tree for each clock domain of the circuit.

#### 7.3.1 Terminology

* A **clock routing problem** instance (clock net) is represented by $n+1$ terminals with $s_0$ being the source and $S = \{s_1, s_2, \dots, s_n\}$ being the sink.
* A **clock routing solutions** contains:
	* **Clock tree topology**: a rooted binary tree $G$ with internal nodes correspond to the Steiner points.
	* **Embedding** provides exact physical locations of edges and internal nodes.
* The cost of each edge is its wirelength, denoted by $|e|$, linear model or [[4 Delay#4.3.5 Elmore Delay]].
* Clock **skew** is the difference in clock signal arrival times between sinks.

![[Pasted image 20231026191037.png|600]]

#### 7.3.2 Problem Formulations for Clock-Tree Routing

* Zero-skew, zero-skew tree ZST
* Bounded skew (with minimal cost)

##### Useful Skew

* Two failure mode
	* **Zero clocking** $x_i + t_{\text{setup}} + \max(i,j) \leq x_j +P$, $\max(i,j)$ is the longest signal propagation from $FF_i$ to $FF_j$
	* **Double clocking** $x_i + \min(i,j) \geq x_j + t_{\text{hold}}$
* Can use linear programming to find optimal clock arrival time to either minimize clock or maximize slack.

![[Pasted image 20231026192649.png|500]]

### 7.3 Modern Clock Tree Synthesis

#### 7.4.1 Constructing Trees with Zero Global Skew

**H-tree** is a self-similar, fractal structure.

##### Method of Means and Medians (MMM)

Recursively partition the set of terminals into two subsets of equal cardinality, and connect center of mass to center of mass of two subsets.

![[Pasted image 20231026193038.png|600]]

##### Recursive Geometric Matching (RGM)

Recursively find a minimum-cost geometric matching of $n$ points, connect them with line segments, and find a balance or tapping point.

![[Pasted image 20231026193355.png|600]]

##### Exact Zero Skew

* Find exact zero-skew tapping points with respect to the Elmore delay model.
* Maintain exact delay balance even when two subtrees have very different delays by elongating wires.

##### Deferred-Merge Embedding (DME)

![[Pasted image 20231026193834.png|550]]

* Defer the decision for Steiner points.
* **Manhattan arc** aka tilted line segment is the set of middle points of two points under Manhattan distance.
* **Tilted rectangular region (TRR)** is a collection of points within a fixed **radius** of a Manhattan arc. 
* The boundary of TRR is the **core**.
* The **merging segment** of a node $ms(v)$ is the locus of feasible locations for $v$.
* The **DME bottom-up phase** determines all possible locations of internal nodes.

```algorithm
Build Tree of Segments (DME Bottom-Up Phase)
Input: set of sinks and tree topology G(S,Top)
Output: merging segments ms(v)

foreach (node v in G, in bottom-up order)
	if (v is a sink node) // if v is a terminal, then ms(v) is a
		ms[v]=PL(v)       // zero-length Manhattan arc
	else                  // otherwise, if v is an internal node,
		(a,b) = CHILDREN(v)     // find v’s children and
		CALC_EDGE_LENGTH(e_a,e_b) // calculate the edge length
		trr[a][core] = MS(a)    // create trr(a) – find merging segment
		trr[a][radius] = |ea|   // and radius of a
		trr[b][core] = MS(b)    // create trr(b) – find merging segment
		trr[b][radius] = |eb|   // and radius of b
	    ms[v]=trr[a] Intersect trr[b] // merging segment of v
```

![[Pasted image 20231026194252.png|550]]
![[Pasted image 20231026195222.png|550]]

**DME Top Down Phase** finds exact locations.

![[Pasted image 20231026195342.png|550]]

#### 7.4.2 Clock Tree Buffering in the Presence of Variation

* Trade-offs between skew and total capacitance are important.
* Clock buffers are inserted into circuits to drive the clock signal.