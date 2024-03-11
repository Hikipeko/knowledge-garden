First partitions the chip into routing regions and searches for region-to-region paths for all signal nets.

### 5.1 Introduction

* A **net** is a set to two or more **pins** that have the same electric potential.
* **Netlist** refers to all nets.
* The chip area is represented by a coarse routing grid, and available routing resources are represented by edges with capacities in a grid graph.
* Each net is given a numerical indicator of its importance.

![[Pasted image 20231020134238.png|600]]

### 5.2 Terminology and Definitions

* A **routing track** is an available wiring path.
* A **routing region** is a region that contains routing tracks and/or columns.
* A **uniform routing region** is formed by evenly spaced horizontal and vertical grid lines that induced a uniform grid over the chip area. The grid is referred to as **ggrid**, composed of grid cells **gcells**.
* Grid lines are typically spaced 7-40 tracks.
* A **nonuniform routing region** aligns routing region to external pin connections or macro-cell boundaries.
* A **channel** is a rectangular routing region with pins on two opposite sizes and no pins on the other sides. Horizontal and vertical.
* The **channel capacity** represents the number of available routing tracks within this channel $c = \lfloor h / d_{\text{pitch}}\rfloor$
* A **switchbox** is the intersection of horizontal and vertical channels. A 3D switchbox has terminals on six boundaries.
* A **T-junction** occurs when a vertical channel meets a horizontal channel.

![[Pasted image 20231020134837.png|500]]
![[Pasted image 20231020135341.png|500]]
![[Pasted image 20231020135443.png|500]]

### 5.3 Optimization Goals

1. Determine whether a given placement is routable
2. Determine a coarse routing for all nets within available routing regions

##### Full-custom Design

* Channel definition and channel ordering must be performed.
* **Channel definition** seeks to divide the global routing area into appropriate routing channels and switchboxes.
* Minimize the total routed length.
* Minimize the length of the longest timing path.

##### Standard-cell Design

* **Feedthrough cells** can be used to route a net across multiple cell rows.
* Ensure routability of the design.
* Find an uncongested solution.

![[Pasted image 20231020140155.png|500]]

### 5.4 Representations of Routing Regions

* Use a grid graph $ggrid = (V,E,C)$, where $V$ represent the routing grid cells, $E$$ represent connection of grid cell pairs, and $C$ represents capacities.
* Other varieties: **channel connectivity graph** and **switchbox connectivity graph**.

![[Pasted image 20231020140611.png|500]]

![[Pasted image 20231020140628.png|500]]

### 5.5 The Global Routing Flow

* Input: layout, netlist.
* Steps:
	1. Define the routing regions
	2. Map nets to the routing regions
	3. Assign crosspoints: routes are assigned to fixed locations, or crosspoints, along the edges of the routing regions.

### 5.6 Single-Net Routing

#### 5.6.1 Rectilinear Routing

* Multi-pin nets are often decomposed into two-pin subnets, followed by point-to-point routing of each subnet according to some ordering.
* See *rectilinear (minimum) spanning tree* and *rectilinear Steiner (minimum) tree* from [[Research/VLSI Physical Design/1 Introduction#1.7 Graph Theory Terminology|graph terminology]].
* An RSMT for a $p$-pin net has 0 to $p-2$ Steiner points, with degree 3 or 4.
* RSMT is NP-hard, while RMSP is much faster.
* **Hanna Grid** To find an RSMT, it suffices to consider only Steiner points located at the intersection.
* Defining the routing regions
* **Mapping nets to routing regions** Each net segment is assigned to a specific grid cell.

![[Pasted image 20231020142313.png|500]]

##### Steiner Tree Construction Heuristic

```algorithm
Sequaltial-Steiner-Tree-Heuristic
Input: set of all pairs P
Output: Steiner tree T(V, E)

P' = P
(pa, pb) = closest_pair(P')
curr_MBB = MBB(pa, pb)
While P' is not empty
	(pMBB, pc) = Closest-Pair(curr_MBB, P')
	// found edge for previous two points and a new point
	Add(E, L-shape that includes pMBB)
	Add(V, pMBB)
	Add(V, pc)
	remove(P', pc)
	curr_MBB = MBB(pMBB, pc) // minimum bounding box with two potential Stenier points
Return (V, E)	
```

#### 5.6.2 Global Routing in a Connectivity Graph

##### Connectivity Model

![[Pasted image 20231020144645.png|550]]
![[Pasted image 20231020144707.png|500]]

1. Define the routing region by stretching the boundary box of cells until a cell or chip boundary is reached.
2. Define the connectivity graph, with each node maintaining horizontal and vertical capacities.
3. Determine the net order in which they are routed.
4. Assign tracks for all pin connections. For each pin, a horizontal track and a vertical track are reserved within pin's routing region.
5. Global routing of all nets.
	1. Net and/or subnet ordering
	2. Track assignment in the connectivity graph (e.g. maze routing)
	3. Capacity update

#### 5.6.3 Finding Shorted Path with Dijkstra's Algorithm

See [[11 Routing]]

### 5.7 Full-Netlist Routing

Consider all nets simultaneously.

#### 5.7.1 Routing by Integer Linear Programming

* [[Linear Programming and Duality 477]] problems can be solved by available software tools such as GLPK and MOSEK.
* Variables:
	* $k$ Boolean variables $x_{net1}, x_{net2}, \dots, x_{netk}$ as indicator for one of $k$ specific routing options.
	* $k$ real variables $w_{net1}, w_{net2}, \dots, w_{netk}$ represents a net weight for a net weight, reflects the desirability of each route option.
	* $k \cdot |Netlist|$ variables in total
* Constrains:
	* Each net must select a single route. $\sum_i x_{neti} \leq 1$
	* The number of assigned routes cannot exceed its capacity.

#### 5.7.2 Rip-Up and Reroute

* Goal: commercial EDA tools require greater scalability and lower runtimes
* Allow temporary violations, so that all nets are routed, but then iteratively remove some nets (rip-up) and reroute them.
* Rely on selecting an effective net ordering.

![[Pasted image 20231020152219.png|540]]

### 5.8 Modern Global Routing

#### 5.8.1 Pattern Routing

Dijkstra's algorithm and maze routing can be slow. Pattern routing only tries commonly used topologies instead.

#### 5.8.2 Negotiated Congestion Routing (NCR)

* Force nets to negotiate for a resource and thereby determine which net needs the resource most.
* Each edge is assigned a cost value $\text{cost}(e)$ that reflects the demand for edge $e$.
* $\text{cost}(net) = \sum_{e \in net}\text{cost}(e)$.
* The edge cost is increased according to the **edge congestion** $\phi(e) = \eta(e) / \sigma(e)$



