---
year: 2013
author: J Lu, P Chen, CC Chang, L Shan, DJH Huang, CC Teng, CK Cheng
citation: 15
---
### 2 Essential Concepts and Related Works

$$
\begin{align}
D_b(\mathbf x, \mathbf y)&= \sum_{v\in V}P_x(b,v)P_y(b,v) & (1) \\
\rho_b(\vec v) &\leq \rho_t, \forall b \in B & (2)
\end{align}
$$

* (1) is the density function of a bin, where $P_x$ and $P_y$ are the overlap functions of bin $b$ and cell $v$.
* (1) is not differentiable, thus hard to optimize.
* **Local smoothing** functions replaces the piece-wise linear original density function with a bell-shaped quadratic function.
* (2) is the density constraints.

### 3 Electrostatic System Modeling

$$
N(\mathbf v) = \sum_{i\in V}N_i(\mathbf V)= \sum_{i \in V} q_i \psi_i(v)\quad (6)
$$

* By using the potential function, we do not need to perform gradient descent on density penalty function.
* Here $q_i$ is the electric quantity of object $i$ (equal to its area), and $\psi_i$ is the local potential.
* [[Electric Charges]] for backgrounds.
* Poisson's equation Eq. (8) relates spatial density and potential distribution.
* Neumann boundary condition (zero gradient at the boundary) is enforced to prevent cells from moving outside.
* $\hat n$ is the outer unit normal.
* $\partial R$ is the boundary of $R$.
* The potential integral is set to zeros thus Poisson's equation has unique solution.

$$
\begin{align}
\begin{cases}
\nabla \cdot \nabla \psi(x,y) &= -\rho(x,y) &\\
\hat n \cdot \nabla\psi(x,y) &= 0, (x,y) \in \partial R, \\
\iint_R \rho(x,y) &= \iint_R \phi(x,y) = 0
\end{cases}
(8)
\end{align}
$$

##### Discrete-Time Fourier Transform (DTFT)

$$
\begin{align}
X(\Omega) &= \sum_{- \infty}^{\infty}x[n]e^{-j\Omega n}\\
X(k) &= \sum_{n=0}^{N-1}x[n]e^{-j2\pi n/N}, \, k = 0, \dots, N-1\\
X(n) &= \frac1N \sum_{k=0}^{N-1}x_k e^{j2\pi n/N}, \, k = 0, \dots, N-1\\
\end{align}
$$

* The infinite sum is replaced by a sum from 0 to $N - 1$.
* Computed for $\Omega = k \frac{2\pi}{N}$
* In software, the DFT is evaluated using the fast Fourier transform, which is $O(n \log n)$.

##### Fast Numerical Solution

![[Pasted image 20231028231400.png|450]]

* Use FFT to solve the Poisson's equation.
* We have $n\times n$ grids and $m$ cells. We need $O(n^2 \log n^2)$ to calculate all the $a_{u,v}$.
* The electric field is the force (gradient descent) of the density penalty.

### 4 Global Placement Algorithms

![[Pasted image 20231028231626.png|500]]

* The initial placement solution is based on quadratic wirelength minimization using B2B net model
* The placement problem is solved using nonlinear **Conjugate Gradient** (CG) method.
	* See https://o-o-sudo.github.io/numerical-methods/-conjugate-gradient.html

#### FFTPL Algorithm

![[Pasted image 20231029105754.png|470]]

##### Self-Adaptive Parameter Adjustment

* **Grid dimension** $n = \max\{1024, \log_2 \sqrt{m}\}$.
* **Step length** $\alpha$ is initialized to $\alpha_0^{max} = 0.044w_b$, where $w_b$ is the grid width.
	* Update as $\alpha_k^{max} = \max(\alpha_0^{max}, 2\alpha_k)$
* **Penalty factor** $\lambda_0$ is initially set as [[2008 NTUplace3-An Analytical Placer for Large-Scale Mixed-Size Designs With Preplaced Blocks and Density Constraints#Global Placement|NTU-place3 global placement]].
	* Update as $\lambda_k = \mu_k \lambda_{k-1}$
	* Scaling factor $\mu_k$ is determined in (13)
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

### 5 Experiments and Results

* Set target placement density $\rho_t = 1.0$ for all benchmarks.



### Questions

* Converge tau < 10%, what is the formula for tau?
* grid number is $\log_2 \sqrt{m}\}$, that small?
* What is CG solver, how does it incorporate with result of FFT?
* Why CG solver instead of gradient descent?
