---
year: 2015
author: J Lu, P Chen, CC Chang, L Shan, DJH Huang, CC Teng, CK Cheng
citation: 93
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
\min_{\mathbf v}W(\mathbf v) &\text{ s.t. } \rho_b(\mathbf v) \leq \rho_t , \forall b \in B &(2)\\
\tilde W_{e_x} &= \frac{\sum_{i,j\in e}x_i \exp(x_i/\lambda)}{\sum_{i,j\in e}\exp(x_i/\lambda)} - \frac{\sum_{i,j\in e}-x_i \exp(-x_i/\lambda)}{\sum_{i,j\in e}\exp(-x_i/\lambda)} &(3)\\
\min_{\mathbf v} f(\mathbf v)&= \tilde W(\mathbf v) + \lambda N(\mathbf v) &(4)
\end{align}
$$

* A **density penalty function** helps incorporate the $|B|$ constraints in Eq. (2) to enhance analyticity.
* $N(\mathbf v)$ is the density function, and $\lambda$ is the penalty factor.

### 3 EDensity

#### 3.1 System Modeling Using Electrostatic Equilibrium

* Each cell is a positive charge $q_i$ equal to its area.
* Correlate the global placement constraint of an even density distribution with the system state of the electrostatic equilibrium.
* Remove DC component from the density distribution $\rho(x,y)$ so that the integral of the density function over the region is 0.
	* Thus underfilled region become negatively charged, attracting cells in overfilled region.
* In the end, the system reaches the electrostatic equilibrium state zero charge density over the entire placement region, while the total potential energy is reduced to 0.

#### 3.2 Density Penalty and Gradient Formulation

##### Filler Insertion

![[Pasted image 20231030122309.png|600]]

* Let $A_m$ denotes the total area of movable nodes.
* Let $A_{ws}$ denotes the total area of whitespace.
* Add equal size, movable, and disconnected cells with total area $A_{fc}=\rho_t A_{ws}-A_m$.
* Size of each cell $A_i$ is average size of the mid-80% of movable cells.

##### Dark Node Insertion

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

### 4 Poisson's Equations and Numerical Solution

* See [[2013 FFTPL An analytic placement algorithm using Fast Fourier Transform for Density Equalization#3 Electrostatic System Modeling|2013 FFTPL Electrostatic System Modeling]].

#### 4.4 Behavior and Complexity Analysis

![[Pasted image 20231030134842.png]]

* Suppose we have $n' = |V_m| + |V_{fc}|$ cells and $m\times m$ grid.
* **Density computation**
	* Traverse all the bins and clears value to 0 is $O(m^2)$.
	* Traverse all the cells to determine the area contribution to according bins is $O(n')$.
* **Potential and field computation**
	* FFT library call is $O(m^2 \log m^2)$.
* Total complexity is $O(n'+m^2 \log m)$ for each placement iteration.
* Usually $O(n') = O(m^2)$, filler cells is $O(n)$, thus the overall complexity is $O(n\log n)$.

#### 4.5 Local Smoothness over Discrete Grids

![[Pasted image 20231030140825.png]]

* Reflect infinitely small movement of cells within each bin.

### 5 Nonlinear Optimization


#### 5.1 Conjugate Gradient Method with Line Search

* Line search is the bottleneck in Conjugate Gradient methods.
* Inaccurate steplength prevent if from matching expected performance.

#### 5.2 Nesterov's Methods with Lipschitz Constant Prediction

![[Pasted image 20231029172941.png|450]]

* Want to solve a convex programming problem in Hilbert Space.
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

##### Steplength Backtracking

![[Pasted image 20231029173455.png|450]]

* Prevent steplength overestimation.
* Set the steplength computed by Eq. (10) as a temporary variable $\hat \alpha_k$.
* Need more understanding

#### 5.3 Preconditioning

* Need more understanding.

### 6 Global Placement Algorithm

#### 6.1 Self-Adaptive Parameter Adjustment

* **Grid dimension** $n = \max\{1024, \log_2 \sqrt{m}\}$.
* **Step length** $\alpha$ is initialized to $\alpha_0^{max} = 0.044w_b$, where $w_b$ is the grid width.
	* Update as $\alpha_k^{max} = \max(\alpha_0^{max}, 2\alpha_k)$
* **Penalty factor** $\lambda_0$ is initially set as [[2008 NTUplace3-An Analytical Placer for Large-Scale Mixed-Size Designs With Preplaced Blocks and Density Constraints#Global Placement|NTU-place3 global placement]].
	* Update as $\lambda_k = \mu_k \lambda_{k-1}$
	* Scaling factor $\mu_k$ is determined by (13)
* **Density overflow** is similar to Eq. (11) in [[2008 NTUplace3-An Analytical Placer for Large-Scale Mixed-Size Designs With Preplaced Blocks and Density Constraints#3 Proposed Algorithm|NTUplace3 density overflow]].
	* Stops when $\tau \leq 10\%$.
* **Wirelength coefficient** $\gamma$ is larger at early time to encourage global movement and smaller at later to move only HPWL-insensitive cells. set as (14).

$$
\begin{align}
\mu_k &= 1.1^{-\frac{\Delta w_k}{\Delta w_{ref}}+1.0},\; \mu_k \in [0.75,1.1] & (13)\\
\Delta w_k &= W(\vec v_k) - W(\vec v_{k-1})\\
\Delta w_{ref} &= 3.5 \times 10^5\\
\gamma &= 8.0 w_b \times 10^{20/9 \times(\tau - 0.1) - 1.0}
\end{align}
$$

![[Pasted image 20231101160217.png]]

#### 6.2 Global Placement

![[Pasted image 20231101155813.png]]

### 7 Experiments and Results

* No parameter tuning towards specific benchmarks.
* Density upper bound is set as 100%.

![[Pasted image 20231101162732.png]]



### Questions

* How is $\lambda$ updated?
* How is $\rho(x,y)$ calculated?
* Use inertia to prevent large cell from oscillating?
* DC component is removed from $\rho(x,y)$ , $p_i$ still remain the same?
* Can we do parameter tuning based on different benchmark properties?
