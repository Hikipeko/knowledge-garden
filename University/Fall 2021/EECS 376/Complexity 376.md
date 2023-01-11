## 15 Introduction to complexity

##### P

P is the class of languages decidable by a polynomial-time Turing machine, where

$$
P = \bigcup_{k \geq 1}DTIME(n^k)
$$
We informally consider P to be the class of efficiently decidable languages.


##### NP

NP (nondeterministic polynomial) is the class of efficiently verifiable languages.

P = NP?

##### NP-Hard

NP-hard is the set of problems that are at least as hard as the hardest problems in NP. To prove a problem is NP-hard, we need to reduce a known NP-hard problem to the problem in polynomial time.



## 16 The Cook-Levin Theorem

SAT is NP-Hard.

If we have an efficient decider $D$ for SAT, we can construct a program and run $D$ to decide it. Thus, an efficient decider for SAT implies that P = NP, and SAT is an NP-Hard problem.



## 17 NP-Completeness

##### D91 Mapping Reduction

![|600](Pasted%20image%2020221219180646.png)

##### D93 Polynomial-Time Mapping Reduction

![[Pasted image 20221219181127.png]]

![[Pasted image 20221219181416.png]]

#### 17.1 3SAT

$$
\text{SAT} \leq_p \text{3SAT}
$$
#### 17.2 Vertex Cover

Can we cover all the edges using k vertices?

#### 17.3 Set Cover

Can we cover all the tasks using k workers?

#### 17.4 Hamiltonian Cycle

Can we find a path that start and ends at the same vertex and visits all the other vertices exactly once?



## 18 Search and Approximation

#### $\alpha-$Approximation

![[Pasted image 20221219191708.png]]

#### 18.1 Approximation for Minimal Vertex Cover

```algorithm
Algorithm Greedy-Cover(G):
	C = empty set
	While G has at least one edge:
		Pick v with the most incident edges
		Remove v and all its adjacent edges from G
		Add v to C
	Return C
```

```algorithm
Algorithm Double-Cover(G)
	C = empty set
	While G has at least one edge:
		Pick e = (u, v) in E
		Remove u, v and their adjacent edges from G
		Add u, v to C
	Return C
```

Since the edges picked by the Double-Cover algorithm are pair-wise disjoint, we need at least half of these vertices to cover them. Thus it's a 2-approximation.

#### 18.2 Maximal Cut

Find the cut containing most number of edges that cross between the partition.

```algorithm
Algorithm Local-Search(G):
	S = empty
	T = V
	Repeat:
		For each v in S:
			If move v to T increases the size, move
		For each u in T:
			If move u to S increases the size, move
		If no move was made, break
	Return S, T
```

When exiting the loop, each point has at least half of its edges crossing the cut. Thus it's an 2-approximation.

#### 18.3 Knapsack

![[DP 281#Knapsack|Knapsack]]

You may wonder why this is NP-complete, let me explain: since $W$ is represents in $\log W$ bits, the time complexity is exponential with respect to the input size.

**Dumb-Greedy**

Pick based on the value.

**Relative-Greedy**

Pick based on average value.

**Smart-Greedy**

Pick the larger value of Dumb-Greedy and Relative-Greedy.
