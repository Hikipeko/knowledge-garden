#### 10.1 Basics

How to transform result of multi-level synthesis (uncommitted logic) into real gates (netlist) for layout task?

Suppose we have gates in our "library," such as the AND gate, the D Flip-Flop. This is called **the technology** we are allowed to use to build the netlist.

1. Transform uncommitted logic into simple, real gates.
2. Transform every SOP form in each node into NAND & NOT gates and get the **subject tree**.
3. Map this onto standard cells in technology.

![[Pasted image 20230924220325.png|500]]

#### 10.2 Technology Mapping as Tree Covering

1. Represent cells in the library in the NAND & NOT gates representation, called the **target tree**.
2. Use [[DP 281]] to solve the problem simple and optimal.

![[Pasted image 20230924223106.png|450]]

Symmetries matter: there are 2 ways to match if symmetric.

#### 10.3 Tree-ifying the Netlist

Matching algorithms work only for trees while gate netlists are Directed Acyclic Graphs. Split every place you see a gate with fanout. These kind of gates become the root of a new sub-tree. Then we map these trees separately.

![[Pasted image 20230924223620.png|350]]

Problem: there are gates that cannot be trees, e.g. EXOR gate.

#### 10.4 Recursive Matching

* Goal: for every node in subject tree, determine what library gate can match.
* Do it recursively
* Be careful to match all possible orientations
* Annotate each such node in the tree with this matching information

![[Pasted image 20230925203347.png|300]]

#### 10.5 Minimum Cost Covering

* Goal: find the best cover of the subject tree **mincost**
* Dynamic Programming: if pattern $P$ is a mincost at node $s$, then each leaf of pattern tree must also be the root of some mincost matching pattern.

```algorithm
mincost(treenode) {
	cost = infinity
	for each (pattern P mathing at subject treenode) {
		Let L = {nodes in subject tree correponding to leaf nodes when P is place at treenode}
		newcost = cost(P)
		for each (node n in L) {
			newcost = newcost + mincost(n)
		}
		if (newcost < cost) {
			cost = newcost;
			treenode.BestPattern = P;
		}
	}
}
```

#### 10.6 Detailed Covering Example
