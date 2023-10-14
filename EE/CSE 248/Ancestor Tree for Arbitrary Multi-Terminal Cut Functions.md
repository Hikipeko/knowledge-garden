### Abstract

Need only $n-1$ computations of the cut function to gain minimum cuts for all $n\choose 2$ pairs of nodes.

### 1 Introduction

* Graph is undirected
* The cost function can be the value of the cut, or something related to this value. E.g. $F_{st} = C(X, \bar X) / (|X|\cdot |\bar X|), \, s\in X \cap t \in \bar X$
* There exists a set of minimum cuts which do not cross each other.
* For arbitrary functions $F$, the cuts might yield values for $F_{ij}$ that cross each other.

### 2 Ancestor Tree

* We conceptually obtain the ancestor tree by recursively find the min of min cut, and then separate the subset into two smaller subsets.
* Continue until each leaf of the tree contains only one node.
* For any two nodes, the lowest common ancestor of their respective leaves gives the min cut.

### 3 Numerical Example

* Consider the algorithm for finding $F_{ij}$ as a routine call (may be NP-hard).
* Define $F_{ij} = C(X, \bar X) / (|X|\cdot |\bar X|), \, i\in X \cap j \in \bar X$
* Each time after choosing two nodes $a, b$, mark them as seeded.
* In next recursion, select a seeded node and an unseeded one.

### 4 The Algorithm and its Proof

1. Arbitrarily set one node to be the seed.
2. Repeat until 
	1. Select a leaf and compute between a seeded node and an unseeded node.
	2. If new vertex $F_{pq}$ is larger than father, then keep $F_{pq}$ in its position
	3. Else put $F_{pq}$ in the lowest place s.t. it is smaller than its father, and attach the original vertex as child of $p$ if $p$ is contained in that vertex, or $q$ otherwise.

Proof is by induction, which is beautiful.


