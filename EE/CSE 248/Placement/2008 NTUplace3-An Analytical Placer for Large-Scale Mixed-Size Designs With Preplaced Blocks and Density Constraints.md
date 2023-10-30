---
year: 2008
author: TC Chen, ZW Jiang, TC Hsu, HC Chen, YW Chang
citation: 306
---
### Abstract

We propose a high-quality analytical placement algorithm considering wirelength, preplaced blocks, and density based on the multilevel framework.

### 2 Analtical Placement Model

![[Pasted image 20231029111551.png|500]]

$$
\begin{align}
\min &W(x,y)  \quad\text{s.t. } D_b(\mathbf x, \mathbf y) \leq M_b \quad &(1)\\
M_b &= t_{\text{density}}(w_bh_b - P_b)& (2)\\
D_b(\mathbf x, \mathbf y) &= \sum_{v\in E} P_x(b,v)P_y(b,v) \quad &(5)\\
\hat D_b(\mathbf x, \mathbf y) &= \sum_{v\in E} c_v p_x(b,v)p_y(b,v)& (8)\\
\min &W(x,y) +\lambda\sum_b(\hat D_b(\mathbf x, \mathbf y)-M_b)^2 &(9)
\end{align}
$$

Finally, the problem changes to (9) with increasing $\lambda$. This problem is solved by CG method.

### 3 Proposed Algorithm

#### Global Placement

* $\lambda_m = 2\lambda_{m-1}$, solve (9) until $overflow\_ratio$ < 10%
* $grid\_num = \sqrt{BlockNumber}$
* Value of $\lambda$ is initialize according to the strength of wirelength and density gradient as

$$
\begin{align}
\lambda_0 &= \frac{\sum |\partial W(\mathbf x, \mathbf y)|}{\sum |\partial \hat D_b(\mathbf x, \mathbf y)|} &(10)\\
overflow\_ratio &= \frac{\sum_b \max(D_b(\mathbf x, \mathbf y) - M_b, 0)}{\sum \text{total movable area}}  &(11)\\
\alpha_k &= sw_b/\lVert d_k\rVert _2 &(12)\\
\sum_{v_i \in V}(\Delta x_i^2 + \Delta y_i^2) &= \lVert \alpha_k d_k \rVert_2^2
 = s^2w_b^2 &(13)
 \end{align}
$$

* Overflow ratio $\tau$ is used as the stop criteria.
* Conjugate gradient (CG) is used to minimize (9). After computing the CG direction $d_k$, the step size $\alpha_k$ is computed by (12), where $s$ is a user-specified scaling factor.
* The total quadratic Euclidean movement is thus fixed as (13).

GP spends 72% of the total runtime.

### Questions

* Why do we choose $\lambda_0$ as this value?
* Why doubled $\lambda$ each round? Are there any better approaches?
* Can we do something about the overflow ratio?

