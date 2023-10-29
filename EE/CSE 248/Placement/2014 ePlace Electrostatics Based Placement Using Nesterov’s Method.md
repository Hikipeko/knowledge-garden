---
year: 2014
author: J Lu, P Chen, CC Chang, L Shan, DJH Huang, CC Teng, CK Cheng
citation: 61
---
### Abstract

* A generalized analytic algorithm to handle large scale standard-cell and mixed-sized placement.

### 1 Introduction

##### Categories

* **Min-cut**
* **Quadratic** use quadratic function to approximate wirelength.
* **Nonlinear** approach use high-order wirelength.
* **Two-stage** methods construct floorplanning followed by standard-cell placement. Often induces suboptimal solutions.
* **Constructive** methods group standard cells into soft blocks.
* **One-stage** methods place Macros and standard cells simultaneously to avoid above limitations. Hard to converge.

##### ePlace

* A generalized one-stage, flat, analytic nonlinear placement algorithm.
* Provide detailed analysis on the density function.
* Use Newterov's method as the nonlinear solver with steplength dynamically predicted via Lipschitz constant.
* Develop an approximated preconditioner to resolve the gap between standard cells and macros.
* Devise an annealing-based macro legalizer to directly control macro shifting.

### 2 Essential Concepts

* $G = (V,E,R)$, where $R$ is the region.
* **Constraint** is no density violation.
* **Solution** is $\mathbf{v} = \{x_1, \dots, x_n, y_1, \dots, y_n\}$, the coordinates of $n$ cells.
* Decompose the region into $n\times n$ rectangular grids (bins) denoted as $B$.
* **Objective** is usually minimizing the HPWL.
* We use [[Weighted-Average Wirelength Mode]] for **wirelength smoothing**.

$$
\begin{align}
W(\mathbf{v}) &= \sum_{e\in E} W_e(\mathbf{v}) = \sum_{e\in E}(\max_{i,j\in e}|x_i - x_j|+|y_i - u_j|) &(1)\\
\min_{\mathbf v} &\text{ s.t. } \rho_b(\mathbf v) \leq \rho_t , \forall b \in B &(2)\\
\tilde W_{e_x} &= \frac{\sum_{i,j\in e}x_i \exp(x_i/\lambda)}{\sum_{i,j\in e}\exp(x_i/\lambda)} - \frac{\sum_{i,j\in e}-x_i \exp(-x_i/\lambda)}{\sum_{i,j\in e}\exp(-x_i/\lambda)} &(3)\\
\min_{\mathbf v} &= \tilde W(\mathbf v) + \lambda N(\mathbf v) &(4)
\end{align}
$$

* A **density penalty function** helps incorporate the $|B|$ constraints in Eq. (2) to enhance analyticity.
* $N(\mathbf v)$ is the density function, and $\lambda$ is the penalty factor.

### 3 Placement Overview

* **MIP** ePlace quadratically minimize the total wirelength at the mixed-size initial placement.
* **MGP** populates extra area with dummy fillers, and co-optimizes all the objects.
	* The most important part.
	* Terminates when density overflow $\tau \leq 10\%$
* **MLG**
* **CGP**
* **CDP**

![[Pasted image 20231028111838.png|500]]
![[Pasted image 20231028112516.png|500]]

### 4 Density Function Analysis

* The density function in Eq. (5) is modeled as the total electric potential energy.
* Enable ePlace to achieve the minimum density overflow.




