### 2 Problem Formulation

Directed graph $G = (V,E,s,c)$ where $s_i$ denotes size for each node, and $c_k$ is the weight of the edge. We want to find a replication cut $(S, T)$ minimizing $c((S \to \bar S) \cup (T \to \bar T)), \, S \cap T = \varnothing$.

##### Prime Linear Programming Formulation

![[Pasted image 20231022120613.png|400]]

##### Dual Linear Programming Formulation

![[Pasted image 20231022120647.png|400]]
![[Pasted image 20231022120704.png|400]]

![[Pasted image 20231022120830.png|600]]

The dual problem can be solved in $O(mv \log(n^2/m))$ by any max flow algorithms, which also maps to a result solving the prime problem.
