## L19 Graphs

$G = (V, E)$

**Simple graph**

Graphs without parallel edges and without self-loops.

Simple Path, Connected Graph, Cycle

Directed graph vs. undirected graph

### Adjacency Matrix

A [[EECS 281#Data Structure|data structure]] used to represent graphs, useful for dense graph.

### Adjacency List

A [[EECS 281#Data Structure|data structure]] used to represent graphs, useful for sparse graph (more common)

## L20 Minimum Spanning Tree

### Prim's Algorithm

Solves MST

Repeat: greedily select the minimum-weight edge that connects the tree to vertices that not yet in the tree.

```algorithm
Loop n times:
	1. Find the undiscoverd nodes with smallest distance -- O(n)
	2. Set it to discovered
	3. Adjust the distance of its adjacent nodes -- O(n)
```

Complexity: $O(n^2)$ if use simple nested loop, $O(m \log n)$ with a [[Heap 281#Binary Heap|binary heap]] storing vertices in the order of their smallest edge-weight, $O(m + n \log n)$ if ==Fibonacci heap== is used.

### Kruskal's Algorithm

Solves MST

Repeat: greedily select the minimum-weight edge that connects two trees into one until the forest merges into a single tree.

Based on [[Heap 281#Disjoint-Set (Union-Find)|Union-Find]].

```algorithm
Sort all the edges -- O(m log(m))
For each e = (u, v) ordered by weight in E:
	if find(u) != find(v):
		union(u,v) -- O(a(n))
```

Complexity: $O(m \log n)$.

## TSP

Use [[Backtracking 281#Branch and Bound Algorithms|Branch and Bound]]

```c++
template<class T>
void genPerm(vector<T> &path, size_t permLength) {
	if (permLength == path.size()) {
		if (cost(path) < currBest)
		currBest = cost(path);
	}
	if (!promising(path, permLength))
		return;
	for (size_t i = permLength; i < path.size(); ++i) {
		swap(path[permLength], path[i]);
		genPerm(pahh, permLength + 1);
		swap(path[permLength], path[i]);
	}
}
```

## Shortest Path

Find a path that starts at $s$ and ends at $t$ that has the smallest weighted path length.

### Single-Source Shortest Path (SSSP)

Find the shortest path from $s$ to all the other vertices.

### Dijkstra's Algorithm

An algorithm for SSSP problem.

Recursively add the vertex that is closest to the running set.

```algorithm
for each vertex v in V:
	dist[v] = inf
	prev[v] = None
	add v to Q
dist[s] = 0

while Q is not empty:
	u = vertex in Q with min dist[u]
	remove u from Q
	for each (u, v) in E:
		alt = dist[u] + w(u, v)
		if alt < dist[v]:
			dist[v] = alt
			prev[v] = u
			
return dist[], prev[]
```

Using ==Fibonacci heap==:

```algorithm
for each vertex v in V:
	if v == source: continue
	dist[v] = inf
	prev[v] = None
	Q.add_with_priority(v, dist[v])
dist[s] = 0

while Q is not empty:
	u = vertex in Q with min dist[u]
	remove u from Q
	for each (u, v) in E:
		alt = dist[u] + w(u, v)
		if alt < dist[v]:
			dist[v] = alt
			prev[v] = u
			Q.decrease_priority(v, alt)
			
return dist[], prev[]
```

Time complexity: $\Theta(m + n \log n)$
