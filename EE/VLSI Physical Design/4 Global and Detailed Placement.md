### 4.1 Introduction

* Routing information is not available.
* Global placement + detailed placement + legalization
* Global placement followed by detailed placement determines the location of cells such that total length is minimized.

### 4.2 Optimization Objectives

* The placement should be routable: all nets can be routed simultaneously.
* Reduce signal delay => reduce total wirelength

##### Wirelength Estimation

* Manhattan distance/L1 norm
* Half-perimeter wirelength (HPWL): commonly used
* Complete graph (clique) model: sum of Manhattan distance
* Monotone chain
* Star
* Rectilinear minimum spanning tree (RMST)
* Rectilinear Steiner minimum tree (RSMT): NP-hard
* Rectilinear Steiner arborescence (RSA)
* Single-trunk Steiner tree (STST)

![[Pasted image 20231012074752.png|150]]![[Pasted image 20231012074809.png|150]]![[Pasted image 20231012074940.png|150]]![[Pasted image 20231012074950.png|150]]

##### Weighted Wirelength

* Net weight can be used to prioritize certain nets over others.

##### Maximum Cut size

* A net can be classified as *uncut* or *cut* given a vertical cutline.
* Define $V_P$ and $H_P$ be the set of global vertical and horizontal cutlines of $P$.
* $\Psi_P(cut)$ is the set of nets cut by a cutline cut.
* $X(P) = \max_{v \in V_P} (\Psi_P(v))$
* $X(P)$ and $Y(P)$ are lower bounds on the demand for horizontal/vertical routing tracks, assessing the routability of $P$.
* To improve routability, we can optimize $L(P) = \sum_{v \in V_P} (\Psi_P(v))+\sum_{v \in H_P} (\Psi_P(h))$.

![[Pasted image 20231012082828.png|500]]

##### Routing Congestion

* The ratio of demand for routing tracks to the supply of available routing tracks.
* *Local wire density* $\phi_P(e) = \eta_P(e)/\sigma_P(e)$, where $\eta_P(e)$ is the estimated number of nets that crosses $e$ and $\sigma_P(e)$ is the estimated capacity.
* The *wire density* of $P$ is $\Phi(P) = \max_{e\in E}(\phi_P(e))$.

![[Pasted image 20231012082845.png|500]]
![[Pasted image 20231012082857.png|500]]

##### Signal Delays

* Circuit timing is usually verified using [[12 Timing#Logic-Level Timing|STA]].

### 4.3 Global Placement

![[Pasted image 20231012083155.png|500]]

#### 4.3.1 Min-Cut Placement

* Use [[2 Netlist and System Partitioning#2.4.1 Kernighan-Lin (KL) Algorithm|KL algorithm]] or [[2 Netlist and System Partitioning#2.4.3 Fiduccia-Mattheyses Algorithm|FM algorithm]]
* Iteratively divide the netlist into two sub-netlists until the number of cells is below the criteria, then assign a region for the sub-netlist.
* Try to minimize $L(P)$, greedy, not optimal.
* Ensure that each region has a feasible number of cells, not all crowded in one cell.

![[Pasted image 20231012084553.png|500]]
![[Pasted image 20231012084606.png|500]]

* Problem: Naïve min-cut ignores the locations of connection pins within partitions that have already been visited, and the locations of fixed, external pins.
* Solution: Use *terminal propagation*, which places artificial connection points on the nearest boundary.

![[Pasted image 20231012084907.png|500]]
![[Pasted image 20231012085020.png|500]]

#### 4.3.2 Analytic Placement

* See [[9 ASIC Placer#9.6-9.7 Quadratic Wirelength Model]].
* Use repel force to prevent overlap?

