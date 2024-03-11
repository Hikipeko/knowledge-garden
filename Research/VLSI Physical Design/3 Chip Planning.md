* Object: determining the location on a chip of large modules such as caches, embedded memories, and IP cores.
* If modules are not specified, we can use [[2 Netlist and System Partitioning]]
* Three stages
	1. Floorplanning
	2. Pin assignment
	3. Power planning

![[Pasted image 20231004173146.png|500]]
### 3.1 Introduction to Floorplanning

* Objective: Assign shape and location for each chip module
* Parameters: potential size of each module and the netlist

### 3.2 Optimization Goals

* Area and shape of the global bounding box
* Total wirelength
	* Estimation: $L(F) = \sum_{i, j \in F}C[i][j]\cdot d_M(i, j)$, where $C[i][j]$ is the degree of connectivity, and $d_M(i, j)$ is the Manhattan distance.
* Combination of above two
* Signal delays
	* Can use [[12 Timing#Logic-Level Timing|STA]] for time estimation.

### 3.3 Terminology

* **Rectangular dissection** is a division of the chip area into a set of blocks or nonoverlapping rectangles.
* **Slicing floorplan** is a rectangular dissection obtained by repeatedly dividing each rectangle recursively.
* **Constraint graph pair** is a floorplan representation that consists of two directed graphs—vertical constraint graph and horizontal constraint graph—which capture the relations between block positions.
* In a **vertical constraint graph (VCG)**, node weights represent the heights of the corresponding blocks.
* A **sequence pair** is an ordered pair (S+,S-) of block permutations. Together, the two permutations represent geometric relations between every pair of blocks a and b.

![[Pasted image 20231004174427.png|500]]
![[Pasted image 20231004174624.png|500]]

### 3.4 Floorplan Representations

* 3.4.1 Floorplan to a Constraint Graph Pair
* 3.4.2 Floorplan to a Sequence Pair
* 3.4.3 Sequence Pair to a Floorplan

```algorithm
Longest-Common-Sequence
Input: sequence S1 and S2, weights of n elements weight[n]
Output: position of each block positions, total span L

for (i = 1 to n)
	block_order[S2[i]] = i // block_order[x] is x's position in S2
	length[i] = 0 // the leftmost position for block i

for (i = 1 to n)
	block = S1[i] // current block
	index = block_order[block] // index in S2, greater than 
	positions[block] = lengths[index] // position of this block
	t_span = position[block] + weight[block]

	for (j = index to n)
		if (t_span > lengths[j])
			lengths[j] = t_span
		else break
L = lengths[n]
```

The update start from j = index because if a block $b$ is after the current block $a$ in $S_2$, either $b$ is above $a$, which means its position has already been decided. Or $b$ is at the right side of $a$, so `length[b] > t_span[a]`.

### 3.5 Floorplanning Algorithms

* Minimize the total length of interconnection
* Simultaneously optimize wirelength and area

#### 3.5.1 Floorplan Sizing

##### Shape Functions and Corner Points

![[Pasted image 20231004212154.png|600]]

##### Minimum-area Floorplan

1. Construct the shape functions of the blocks
2. Determine the shape function of the top-level floorplan
3. Find the floorplan an individual block's dimensions and locations

![[Pasted image 20231004212358.png|600]]

#### 3.5.2 Cluster Growth

 * The floorplan is constructed by iteratively adding blocks until all blocks have been assigned.
 * Find initial floorplan solutions for iterative algorithms such as simulated annealing.

##### Linear Ordering

* Arranges all the blocks in a single row, minimizing the total wirelength of connections between block.
* Provides a initial placement order for cluster growth.
* Choose an initial block and iteratively put the block with maximum gain next to the previous block.

#### 3.5.3 Simulated Annealing

```algorithm
if (cost < 0)
	adopt the update
else
	if (Ramdon(0, 1) < exp(-cost/T))
	adopt the update
```

#### 3.5.4 Integrated Floorplanning Algorithms

* MILP
* LP

### 3.6 Pin Assignment

![[Pasted image 20231004215236.png|550]]

### 3.7 Power and Ground Routing

