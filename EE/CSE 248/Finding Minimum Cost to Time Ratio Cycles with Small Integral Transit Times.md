### 1 Introduction

* $D = (V,E,t,c)$, where $t_e$ is the transit time, and $c_e$ is the cost.
* Find a cycle $C$ with the minimum cost to time ratio $\lambda(C) = \sum_{e \in C} c_e / \sum_{e \in C} t_e$.
* Let $E' \subseteq E$ denotes the set of arcs $e$ s.t. $t_e >0$.
* $\lambda^* = \min_C \lambda(C)$
* If we define a new length $c_e(\lambda) = c_e - \lambda t_e$, we can interpret $\lambda^*$ as the largest value of $\lambda$ s.t. $D$ has no negative cycle.

### 2 Preliminary Reductions

##### Prime Problem

* Minimize: $\sum_{e \in E} c_e x_e$
* Constraints:
	* $\sum_{e\in E} t_e x_e = 1$
	* $Ax=0$ (flow conservation)
	* $x \geq 0$
* By [[Linear Programming and Duality 477|LP Duality]], we get the dual problem.

##### Dual Problem

* Maximize: $\lambda$
* Constraints: $\lambda t_{(u,v)} - \pi_u + \pi_v \leq c_{(u,v)}$

* If $\lambda^*$ and $\pi^*$ are optimal solutions, we can find a cycle $C$ with $\lambda(C) = \lambda^*$ in $O(n + m)$ time by using [[Graph Connectivity 477#Strong Connectivity]] and [[Depth First Search 477]].
* We can view $c_e(\lambda) = c_e - \lambda t_e$ as arc length, and $\pi$ as shortest distance from a source. 
* Again, we can interpret $\lambda^*$ as the largest value of $\lambda$ s.t. $D$ has no negative cycle.

### 3 Minimum Cycle Means and 0-1 Transit Times

* We look at the simplified situation where $t_e = 1$, and all edges are positive.
* Use a variant of  [[Bellman-Ford Algorithm]] to find cycle with minimum cost to time ratio.
* The idea to the paper is to early stop the algorithm as long as the minimum is found (by checking the dual equations).
