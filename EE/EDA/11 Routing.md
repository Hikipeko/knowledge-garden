### 11.1 Routing Basics

![[Pasted image 20230929204705.png]]

* We can connect wires in different layers with vias.
* **FEOL**: Front end of line, make transistors on Si wafer.
* **BEOL**: Back end of line, make layers of wires on top of transistors
* Lots of placement algorithms, but not so many routing algorithms
* Geometric assumption: gridded routing, wire and their vias fit in the grid

![[Pasted image 20230929204856.png|300]]

## Maze Routing
### 11.2 2-Point Nets in 1 Layer

* Find the *best* wiring path one by one.
* Q: optimal choice for one net may block later nets?
* Problem: given a source S and a target T, find shortest path connecting S and T
* Expand: Breadth-first-search to find all the paths
* Backtrace: find the best path
* Clean-up: mark new path as obstacles

![[Pasted image 20230929205244.png|250]]

### 11.3 Multi-Point Nets

* Problem: Gates have fanouts, need to route from one source to many targets.
* Start: Pick one point as S, all the others as Ts
* First: Use maze route to find path from S to nearest T
* Next: Re-label all cells on found path as sources, then rerun maze router
* Repeat: For each remaining unconnected target point
* Not an optimal solution: Steiner Tree is NP-hard

### 11.4 Multi-Layer routing

* Use **vias** to access other layers
* Remember EECS 281 project 1
* Problem: vias is more expensive

![[Pasted image 20230930112310.png|500]]

### 11.5 Non-Uniform Grid Costs

Assign different costs for each cell

## Implementation Mechanics

### 11.6 How Expansion Works

* **Wavefront** is the neighbors of the new cells worth looking at
* Expand in pathcost order
* Variant of Dijkstra's algorithm
* Record the predecessor

```algorithm
wavefront = PQ of { source cell }
while (no reach target cell) {
	if (wavefront == empty)
		quit -- no path can reach

	C = wavefront.pop()
	if (C == target) {
		result = backtrace path in grid
		cleanup
		return result
	}
	foreach (unsearched neighbor N of cell C) {
		newcost = pathcost(C) + cellcost(C)
		if (newcost < pathcost(N)) {
			pathcost(N) = newcost
			pred(N) = C
			wavefront.push(N)
		}
		mark C as searched
	}
}
```

### 11.7 Data Structure and Constraints

* Grid: cost of cell
* Wavefront: cells to expand, pathcost, predecessor, searched or not
* Constraint for optimal solution: consistency, i.e. cost is independent of path
* Problem: in reality, cost is dependent on path, e.g. a bend can cost more
* Solution 1: search until any cell in the wavefront has a higher pathcost
* Solution 2: early quit, e.g. do 0.1 * \#expansion_to_target more searches

![[Pasted image 20230930114511.png|500]]

### 11.8 Depth First Search

* Problem: We want the wavefront to search towards the target
* Solution: Add a predictor function to the cost, index PQ with pathcost + predictor
* An application of a classical ides: **A\* search**
* Typical estimator is Distance_to_target x Smallest_cell_cost

### 11.9 From Detailed Routing to Global Routing

* Every other layer of meta wiring has a preferred direction
* Divide & Conquer: divide chip into coarser grid, ~ 200 x 200 wires in size
* Makes sure overall wiring congestion is reasonable
* Global router has a dynamic cost function, increases to penalize congestion

![[Pasted image 20230930120120.png|500]]