### 8 Learning from Complete Data

#### Maximum Likelihood

* See [[Unsupervised Learning 445#L22 MLE]]
* DAG is specified, and we want to set CPT parameters s.t. P(data) is maximized.

![[Pasted image 20231118130737.png|400]]

##### Solution

$$
\begin{align}
P_{ML}(X_i=x|pa_i=\pi) &= \frac{\#(X_i=x,pa_i=\pi)}{\#(pa_i=\pi)}\\
P_{ML}(X_i=x) &= \frac{\#(X_i=x)}{T}
\end{align}
$$

#### Markov Models

* **Finite context**: to predict the $l^{\text{th}}$ word, it's sufficient to consider $n$ words precede it.
* **Position invariance** predictions should not depend on where the context occurs
* n-gram

#### Naïve Bayes Models

See [[2 Classification 258#Naïve Bayes]].

### 9 Linear Regression and Least-Squares

#### Linear Regression

* Problem: If the parents are real-valued, then it is no longer possible to use to CPT.
* Solution: We can use **Gaussian conditional distribution**.
* We need to learn the variance $\sigma$ and weights $\vec w$.
* Maximizing the log likelihood $P(y1, \dots, y_T|\vec x_1, \dots, \vec x_t)$ is equivalent to the least-square problem for a linear fit.

$$
\begin{align}
P(y|\vec x) &= \frac{1}{\sqrt{2\pi\sigma^2}}\exp\{-\frac{(y - \vec w \cdot \vec x)^2}{2\sigma^2}\}  \\
\sum_ty_t x_{\alpha t} &= \sum_t (\vec w \cdot \vec x_t)x_{\alpha t}\\
\end{align}
$$

![[Pasted image 20231118135130.png|500]]

##### Time Series Prediction

* Problem: Let $\{x_1, x_2, \dots, x_T\}$ be a time series, how can we predict the future from the past?
* Solution: We can maximize $P(x_t | x_{t-1}, \dots, x_{t-d})$.

#### Numerical Optimization

* **Analytical** Compute the gradient and solve for where it vanishes.
* **Numerical** Hillclimbing methods

##### Gradient Ascent/Descent

$$
\theta \leftarrow \theta + \eta_t(\frac{\partial f}{\partial \vec \theta})
$$

Cons: Tricky to tune the learning rate, no guarantee of monotonic convergence.

##### Newton's Method

![[Pasted image 20231118141601.png|400]]

By setting the derivative of quadratic approximation to 0, we obtain

$$
\theta \leftarrow \theta - H^{-1}\nabla f
$$

Cons: expensive to compute, unstable when initial estimate is poor.

#### Logistic Regression

$$P(Y=1|\vec x) = \sigma (\vec w \cdot \vec x) =\frac{1}{1 + \exp(-\vec w \cdot \vec x)}$$

* We do not have an analytical solution for Logistic regression, so we need to use numerical methods.
* Theorem: the log-conditional likelihood for logistic regression is a **concave** function of $\vec w$.
	* The Hessian is negative semidefinite.
* We will converge to the same point no matter what the initialization.
* Set $\eta = 0.2/T$, initialize $w = (0, \dots, 0)$

![[Pasted image 20231118142606.png|450]]

### 10 Learning from Incomplete Data

* The data is IID, but only consists of T partially complete instantiations of the nodes in the BN.
* Example: build a movie recommendation system, assume the Bayes Network is as follow.

![[Pasted image 20231118143211.png|400]]

* $H_t$ denotes the set of hidden variables for the $t^{\text{th}}$ example.
* $V_t$ denotes the set of visible variables for the $t^{\text{th}}$ example.
* When maximizing the log-likelihood, we can not decouple CPTs as for complete data.

![[Pasted image 20231118161419.png|400]]

#### Auxiliary Functions

$$
\vec \theta_{\text{new}} = \arg\max_{\vec\theta} Q(\vec \theta, \vec \theta_{\text{old}})\quad(*)
$$

* A function $Q(\vec \theta', \vec \theta)$ is call an auxiliary function for the objective function $f(\vec\theta)$ if it satisfies two properties:
	1. $Q(\vec \theta, \vec \theta) = f(\vec \theta)$ for all $\vec \theta$
	2. $Q(\vec \theta', \vec \theta)\leq f(\vec \theta')$ for all $\vec \theta, \vec \theta'$.
* Theorem: Update rule $(*)$ converge monotonically to a stationary point with $f(\vec \theta_{\text{new}}) \geq f(\vec \theta_{\text{old}})$ 

![[Pasted image 20231117164247.png|400]]

### 11 EM Algorithm

* See [[Unsupervised Learning 445#L23 EM]].
* E-Step: Compute posterior probabilities $P(H_t=h|V_t = v_t)$ based on current CPT
* M: compute CPT based on point assignment

##### E-Step (Inference)

* At root nodes: $P(X_i = x|V_t = v_t)$
* At other nodes: $P(X_i = x,\text{pa}_i = \pi|V_t = v_t)$
* These probabilities must be computed over a quadruple loop:$V_t, X_i, X_i = x, \text{pa}_i = \pi$.

##### M-Step (Learning)

![[Pasted image 20231118163337.png|450]]

#### Derivation

**Key Inequality**

$$
\log \tilde P(v) \geq \sum_h P(h|v) \log \frac{\tilde P(h,v)}{P(h|v)}
$$

* Let $\Theta$ denote the collection of CPTs in a BN.
* Let $\{v_t\}_{t=1}^T$ denote an incomplete data set for this BN.

$$
\begin{align}
\mathcal{L}(\Theta) &= \sum_t \log P(V_t = v_t) \\
Q(\tilde \Theta, \Theta) &= \sum_t \sum_h P(H_t = h|V_t = v_t) \log \frac{\tilde P(H_t = h, V_t = v_t)}{P(H_t = h|V_t = v_t)}\\
(i)\; Q( \Theta, \Theta) &= \mathcal{L}(\Theta)\\
(ii)\; Q(\tilde \Theta, \Theta) &\leq \mathcal{L}(\tilde \Theta)
\end{align}
$$

* E-Step is equivalent to computing of the auxiliary function.
* M-Step is equivalent to maximizing the function.