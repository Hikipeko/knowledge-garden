---
year: 2018
author: CK Cheng, AB Kahng, I Kang, L Wang
citation: 138
---
### Abstract

* Dissect the previous implementation and show that solution quality can be significantly improved using (1) constraint-oriented local smoothing and (2) dynamic step size adaption.
* Propose a new density function that comprehends local overflow of area resources.
* Extend our global placer to address routability.

### 1 Introduction

* [[2015 ePlace Electrostatics-Based Placement Using Fast Fourier Transform and Nesterov’s Method|ePlace]] cannot produce routable placements. ePlace routing hotspots demand twice of the routing supply.
* RePlAce add optimization of routability in global routing to the Nesterov's approach, achieving substantial scaled HPWL improvements on DAC-2012 and ICCAD-1012.
* The standard minimum total HPWL placement objective at some point becomes detrimental to routability.

##### Contributions

* We propose a new **constraint-oriented local-density function** for mixed-size placement that incorporates (i) a constraint-oriented local-density penalty factor for each bin, and (ii) a constraint-oriented local-density cost coefficient for each instance. Combining the previous global density.
* We propose a methodology for **density-penalty adaptation** via an improved dynamic step size adaptation that automatically adjusts the density penalty factor based on the HPWL curve.
* We propose a **layer-aware cell inflation technique**.

### 2 Placement Overview

##### Necessity of Local Density Function

* Globally applying the density penalty factor sacrifices wirelength in less-overlapped bins to resolve more-overlapped bins.
* Our local-density function provides more repulsive forces for overflowed bins.
* Formulate a **constraint-oriented local-density penalty factor** $v_j$ per bin.
* Apply a **constraint-oriented local density coefficient** $\Delta_i$ per instance to the local-density function.

##### Constraint-Oriented Local-Density Penalty for Each Bin

* $v_j = \exp(\alpha \cdot (BinDemand_j - BinCapacity_j))$, where $BinDemand$ is the overlapping area.
* $\alpha$ starts small, e.g. $1e-12$, and gradually increases through the Nesterov's optimization.

![[Pasted image 20231109144227.png|450]]

##### Local-Density Cost Coefficient per Each Cell

$$
D^{local}(\mathbf v) = \sum_{b_j \in B}v_jD_j^{local}(\mathbf v)= \sum_{b_j \in B}v_j(\sum_{c_i \in b_j}A_{ij}q_i\phi_j),
$$

where $A_{ij}$ is the overlapped area, $q_i$ is the electric charge, and $\phi_j$ is the local electric potential of bin $b_j$.



