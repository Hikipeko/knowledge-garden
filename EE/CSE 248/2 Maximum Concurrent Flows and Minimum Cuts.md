* Based on [[1 Ancestor Tree for Arbitrary Multi-Terminal Cut Functions]]
* Used for [[2 Netlist and System Partitioning]]

### 1 Introduction

### 2 Minimum Cut and Essential Cut Set

* $f$ has to be deterministic (distinct cuts have distinct cost) and symmetric
* Theorem 1: $F_{ik} > \min(F_{ij}, F_{jk})$
	* There are at most $n-1$ distinct values of min-cost partitions.
* Theorem 2: We need at most $n-1$ distinct cuts s.t. for all $i, j \in V$, one of the cuts is the minimum cut separating $i$ and $j$.

### 3 The Global Ratio Cut and the Maximum Concurrent Flow

##### Uniform Multicommodity Flow

![[Pasted image 20231014111951.png|500]]

* There's a commodity for every pair of nodes and the demand for every commodity is the same.
* The demand of flow between each pair of vertices is $f$, which is the maximization target.
* Sum of all arc flows is less than the arc capacity.
* Solving the problem:
	1. Write the prime problem
	2. Write the duo problem
	3. Simplify the duo problem
	4. Assume we get a solution for the duo problem, the solution also solves the original problem (minimum ratio cut).

Question:

1. Where is the NP-hard part?
2. How to think reversely?
