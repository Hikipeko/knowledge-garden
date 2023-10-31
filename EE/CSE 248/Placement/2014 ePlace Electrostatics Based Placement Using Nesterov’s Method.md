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
	* Low solution quality and robustness.
* **Nonlinear** approach use high-order wirelength.
	* Due to high complexity, usually employ multilevel cell clustering.
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
\min_{\mathbf v} f(\mathbf v)&= \tilde W(\mathbf v) + \lambda N(\mathbf v) &(4)
\end{align}
$$

* A **density penalty function** helps incorporate the $|B|$ constraints in Eq. (2) to enhance analyticity.
* $N(\mathbf v)$ is the density function, and $\lambda$ is the penalty factor.

### 3 Placement Overview

1. **MIP** ePlace quadratically minimize the total wirelength at the mixed-size initial placement.
2. **MGP** populates extra area with dummy fillers, and co-optimizes all the objects.
3. **MLG**
4. **CGP**
5. **CDP** the detailed placer in "Unified Analytical Global Placement for Large-Scale Mixed-Size Circuit Designs" is used to legalize the standard-cell layout.

![[Pasted image 20231028111838.png|500]]
![[Pasted image 20231028112516.png|500]]

### 4 Density Function Analysis

* See [[2013 FFTPL An analytic placement algorithm using Fast Fourier Transform for Density Equalization#3 Electrostatic System Modeling|2013 FFTPL Electrostatic System Modeling]].
* We obtain $\psi(x,y), \xi_x(x,y), \xi_y(x,y)$ in $O(n\log n)$. ==What is n?==

#### System Modeling Using Electrostatic Equilibrium

* Each cell is a positive charge $q_i$ equal to its area.
* Correlate the global placement constraint of an even density distribution with the system state of the electrostatic equilibrium.
* Remove DC component from the density distribution $\rho(x,y)$ so that the integral of the density function over the region is 0.
	* Thus underfilled region become negatively charged, attracting cells in overfilled region.
* In the end, the system reaches the electrostatic equilibrium state zero charge density over the entire placement region, while the total potential energy is reduced to 0.

#### Density Penalty and Gradient Formulation

##### Filler Insertion

![[Pasted image 20231030122309.png|600]]

* Let $A_m$ denotes the total area of movable nodes.
* Let $A_{ws}$ denotes the total area of whitespace.
* Add equal size, movable, and disconnected cells with total area $A_{fc}=\rho_t A_{ws}-A_m$.
* Size of each cell $A_i$ is average size of the mid-80% of movable cells.

##### Dark Node

* Illegal placement regions are modeled as dark node.
* $A_d$ is the set of all dark nodes, and $A_d$ is the total area.

##### Density Scaling

![[Pasted image 20231030125808.png|600]]

The areas $A_i$ of each fixed or dark node must be scaled by the target density $\rho_t$.

##### Potential Energy Computation

* Let $V' = V_m \cup V_f\cup V_{fc} \cup V_d$ (movable, fixed macros, filler, dark) denote the set of all elements.
* For each node $i \in V'$, let $\rho_i, \xi_i, \psi_i$ denote the electric density, field, and potential at that point.
* Total potential energy $N(\mathbf v ) = \frac12 \sum_{i\in V'}q_i\psi_i$.
* Cast the numerous grid density constraints into a single $N(\mathbf v)=0$ constraint.
* Our analytic approach handles the problem by following the Lorentz force law:

$$
\nabla f(\mathbf v) = \nabla W(\mathbf v) + \lambda \nabla N(\mathbf v) = (\frac{\partial W}{\partial x_1}, \frac{\partial W}{\partial y_1} \cdots) - \lambda (q_1 \xi_{1_x},q_1 \xi_{1_y}, \cdots)^T
$$

#### Behavior and Complexity Analysis

![[Pasted image 20231030134842.png]]

* Suppose we have $n' = |V_m| + |V_{fc}|$ cells and $m\times m$ grid.
* **Density computation**
	* Traverse all the bins and clears value to 0 is $O(m^2)$.
	* Traverse all the cells to determine the area contribution to according bins is $O(n')$.
* **Potential and field computation**
	* FFT library call is $O(m^2 \log m^2)$.
* Total complexity is $O(n'+m^2 \log m)$ for each placement iteration.
* Usually $O(n') = O(m^2)$, filler cells is $O(n)$, thus the overall complexity is $O(n\log n)$.

#### Local Smoothness over Discrete Grids

![[Pasted image 20231030140825.png]]

* Reflect infinitely small movement of cells within each bin.

### 5 Mixed-Size Global Placement

* MGP handles macros and standard cells in the same way.
* Line search is the bottleneck in Conjugate Gradient methods.

##### B Nesterov's Methods with Lipschitz Constant Prediction

![[Pasted image 20231029172941.png|450]]

* Two solutions $\mathbf u_k$ and $\mathbf v_k$ are concurrently updated at each iteration, initialized to $\mathbf v_{mIP}$.
* $\alpha_k$ is the steplength.
* $a_k$ is an optimization parameter, $a_0 = 1$.
* **BkTrk** denotes the steplength backtracking.
* $\nabla f_{pre}$ denotes the preconditioned gradient.
* Recall that $f(\mathbf v)= \tilde W(\mathbf v) + \lambda N(\mathbf v)$
* Instead of line search, compute the steplength through a closed-form formula of the Lipschitz constant of the gradient defined as below.
* $f$ is required to have [[Lipschitz Continuity]].
* The Lipschitz constant is approximated as in (10).

$$
\begin{align}
\forall \text{ convex } f(\mathbf v) &\in C^{1,1}(H), \, \exists L > 0 \text{ s.t. }\forall \mathbf u,\mathbf v \in H,\\
\lVert \nabla f(\mathbf u) &-  \nabla f(\mathbf v) \rVert \leq L\lVert\mathbf u - \mathbf v\rVert. &(9)\\
\tilde L_k &= \frac{\lVert\nabla f(\mathbf v_k)-\nabla f(\mathbf v_{k-1})\rVert}{\lVert \mathbf v_k -\mathbf v_{k-1} \rVert}, \, \alpha_k = \tilde L_k^{-1} &(10)
\end{align}
$$

##### C Steplength Backtracking

![[Pasted image 20231029173455.png|450]]

* Prevent steplength overestimation.
* Set the steplength computed by Eq. (10) as a temporary variable $\hat \alpha_k$.
* Need more understanding

##### D Nonlinear Placement Preconditioning

* Need more understanding.

### 6 Macro Legalization & Standard-Cell Global Placement

#### Macro Legalization

![[Pasted image 20231029211826.png|500]]

* At each iteration of the outer loop, update the cost function as $f_{mLG}(\mathbf v) = W(\mathbf v) + \mu_D D(\mathbf v) + \mu_O O_m(\mathbf v)$
* $D$ is the total standard-cell area covered by macros, $O$ is macro overlap.
* **Objective** is to minimize $W(\mathbv v) + \mu_D D(\mathbf V)$, set $\mu_D = W(\mathbf v) / D(\mathbf v)$.
* **Constraint** is $O(v) = 0$, we set $\mu_O$ as penalty factor and multiplied by $\kappa$ at each mLG iteration.
* Inner loop is simulated annealing.

#### Standard-Cell Global Placement

* Same with mGP except for fixed macros.





### Questions

* How is $\lambda$ updated?
 