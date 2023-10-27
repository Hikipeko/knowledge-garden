---
year: 1993
author: LT Liu, M Shih, NC Chou, CK Cheng
citation: 32
---
### Abstract

Propose a novel paradigm for two-way circuit partitioning which minimizes the block cycle.

### 1-2 Introduction & Timing-Optimal Problem

* Combinational block delay $l_i = \sum_{v \in i}w_v$ 
* Edge delay (intermodule delay) $\hat d_i = \sum_{e \in i} c_e$
* Number of registers $r_i$
* **Iteration bound** $L = \max((l_i + \hat d_i)/ r_i)\quad \forall \,\text{loop}\, i$
* Sum of functional block delay $dj$
* **Path delay bound** $D = \max((d_j + \hat d_j)/ r_i - 1)\quad \forall \,\text{I/O path}\, j$
* **Dominant delay** $T = \max(L,D)$
* A partition $P$ is **bound-optimal** if $T = M(P)$
* A partition $P$ is **timing-optimal** if $M(P) < M(P')$ for all other $P'$

### 3 Theoretical Aspects

* Use [[1991 Retiming Synchronous Circuitry#4 An Algorithm for Clock Period Minimization]] for retiming.
* **Lemma 1** Given a graph and a constant $K$, if each simple loop $l$ satisfies $(t_l + d_l) / r_l \leq K$, $G$ can be retimed to achieve a cycle time equal $K$.
* **Theorem 2** If each path between the primary I/O is cut at most once and each loop is not cut, then $P$ can be retimed to achieve a clock cycle time of $T$.
* The complexity is $NP$.

### 4 Heuristics

##### Flow Timing Cut

1. Condense each strongly connected component in $G$ into one supernode to obtain an acyclic graph $G'$ (we want to ensure that a loop is not cut)
2. If (a supernode exceed 0.2 * `total_size`) apply replication cut
3. Invoke saturated_network($G'$) to saturate $G'$ with flow to get a distance function $d$ for each node
4. Select a partition according to $d$
5. Adjust $P$ such that each path between the primary I/O is cut at most once