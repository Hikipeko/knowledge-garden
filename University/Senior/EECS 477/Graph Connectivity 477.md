### Undirected Graph

An undirected graph is **connected** if there is a path between each pair of vertices.

**Articulation point** is a vertex that, fi removed, would disconnect the graph.

**Bridge** is an edge that, if removed, would disconnect the graph.

### Directed Graph

In a directed graph, $u$ and $v$ are **strongly connected** if there are path from $u$ to $v$ and $v$ to $u$.

#### Strong Connectivity

```algorithm
Strongly-Connect(G):
	Let GT be G with all edges reversed
	Pick any vertex s
	Run DFS-recur(s) in G
	Run DFS-recur(s) in GT
	If both search discovered all vertices Return True
	Else return False
```

* Define a relation $\text{SC} \subseteq V \times V,\; \text{SC}(u,v)$ iff $u$ and $v$ are strongly connected.
* The **strongly connected components (SCCs)** are the equivalent classes of SC.
* Find-SCCs has time complexity $O(m+n)$.

![[Pasted image 20231020105815.png|500]]

```algorithm
Find-SCCs(G):
	Run DFS in G, sort vertices by finish time
	Run DFS in GT (2)
		Whenever you choose a new root vertex u, always choose the u maximizing finish time
	Trees in DFS forest from (2) are SCCs in G
```

![[Pasted image 20231020105831.png|500]]