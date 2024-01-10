Many [[Graph Connectivity 477|connectivity problems]] can be solved with DFS.

```algorithm
DFS-recur(u):
	Mark u as discovered
	For each outgoing edge (u,v)
		If v is not discovered/finished, Call DFS(u)
	Mark u as finished

DFS:
	While there is still an undiscovered vertex u
		Call DFS-recur(u)
```

![[Pasted image 20231020104538.png|500]]

![[Pasted image 20231020104502.png|500]]

In undirected graph, there's no cross edge; forward and backward edges are equivalent.