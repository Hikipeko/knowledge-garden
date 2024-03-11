#### 3.1-3.2 Basics

![[Pasted image 20230716163050.png]]

* Different variable orders are possible
* The decision diagram is not unique for a function
* **Ordering**: variables must appear in the specific order along all paths

##### Canonical Form

* Representation that does not depend on gate implementation of a Boolean function
* Same function always produces this exactly same representation

##### Reduction

1. Merge equivalent leaves, leaves only one 0 and one 1
2. Merge isomorphic (nodes with same variable and identical children) nodes
3. Eliminate redundant tests

![[Pasted image 20230716163837.png|200]] ![[Pasted image 20230716163911.png|130]]

##### Reduced Ordered BDD

* Finally we reduce the BDD to a Reduced Ordered BDD, which is ==canonical==.
* Q: how do we practically get the ROBDD?

#### 3.3 BDD Sharing

* In a ROBDD, a Boolean function is ==a pointer to the root node== of a canonical graph data structure.
* BDD can have multiple roots, called a **multi-rooted BDD**.
* Minimize size over several separate BDDs by maximum sharing, e.g. 64-bit adder nodes 12,481 -> 571

![[Pasted image 20230716164500.png|300]]

#### 3.4 BDD Ordering

##### Solving Problems Using BDD 

* Are two boolean functions $F, G$ the same? Check if they have same BDD.
* What inputs make $F, G$ give different answers? Ask "satisfiable" for $H = F \oplus G$.
* Tautology checking? Check whether the BDD is root - 1.
* Satisfiability? Whether there's a path from root to leaf 1.

##### Ordering Matters

![[Pasted image 20230716175522.png|400]]

* Variable ordering heuristics: make nice BDDs for reasonable problems
* Characterization: know what problems never make nice BDDs (e.g. multipliers)
* Dynamic ordering: pick the order on the fly
* Related inputs should be near each other in order
* Groups of inputs that can determine function should be (i) close to each other, and (ii) near top of BDD.