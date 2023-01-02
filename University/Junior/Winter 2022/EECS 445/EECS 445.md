# Introduction to Machine Learning

### 李航《统计学习方法》

- [ ] 监督学习
- [ ] 感知机
- [ ] k近邻法
- [ ] 朴素贝叶斯法
- [ ] 决策树
- [ ] 逻辑斯回归与最大熵模型
- [ ] SVM
- [ ] Boost
- [ ] EM算法
- [ ] 隐马尔可夫
- [ ] 条件随机场
- [ ] 无监督
- [ ] 聚类方法
- [ ] 奇异值分解
- [ ] 主成分分析
- [ ] 潜在语义分析
- [ ] 马尔可夫链蒙特卡罗法



## Supervised Learning

### L2 Linear Classifiers

##### Classification

Given the features, predict the labels as a discrete value.

##### Feature Vector

Statistics or attributes that describe the data, represented in terms of vectors.

#### Linear Classifiers

Goal: learn a linear decision boundary that minimizes training error.

##### Hyperplane

A plane specified by a parameter vector $\bar{\theta}  \in \mathbb{R}^d$ and offset $b \in \mathbb{R}$. It is the set of point $\bar{x}$ such that $\bar\theta \cdot \bar{x} + b = 0$.

Linear separable vs. not linear separable.



### L3 Perceptron

An algorithm that gives a linear classifier. It repeatedly find a misclassified point and do gradient descent on that point.

##### Training Error

We can define the training error as the amortized number of points that is incorrectly classified.
$$
E_n(\bar{\theta}) = \frac{1}{n}\sum_{i=1}^n [[y^{(i)} (\bar{\theta} \cdot \bar{x}^{(i)}) \leq 0]]
$$
##### Perceptron Update
$$
\begin{align}
\text{if} \quad y^{(i)}(\bar{\theta}^{(k + 1)} \cdot \bar{x}^{(i)}) \leq 0\\
\bar{\theta}^{(k + 1)} = \bar{\theta}^{(k)} + y^{(i)}\bar{x}^{(i)}
\end{align}
$$
The perceptron algorithm converges as long as the training examples are linear separable. This the the result of gradient descent if we define lost function as $\max(-z, 0)$.

##### Empirical Risk
$$
R_n(\bar\theta) = \frac{1}{n}\sum_{i = 1}^n \text{Loss}(y^{(i)} (\bar\theta \cdot \bar x^{(i)}))
$$
##### Hinge loss Function
$$
\text{HingeLoss} (z) = \max((1 - z), 0)
$$



### L4 Gradient Descent

Take a small step opposite to which the gradient points.
$$
\bar\theta^{(k+1)} = \bar\theta^{(k)} - \eta \nabla_\bar\theta R_n(\bar\theta)|_{\bar\theta = \bar\theta^k}
$$


#### Stochastic Gradient Descent

Update based on a single points a time instead of on the whole dataset.

Often gets close to minimum faster than GD.



### L5 SVM

#TODO 整理cs229

#### Hard Margin SVM

We want the linear classifier to be maximally removed from training examples closest to the decision boundary.
$$
\min_{\gamma, w, b} \frac{1}{2}||w||^2, \,\,
y^{(i)}(w^T x^{(i)} + b) \geq 1, \,\,i = 1, ..., m
$$
Only works if data are liner separable. Fixed by Soft-Margin SVM.

**Soft-Margin SVM**
$$
\min_{\gamma, w, b} \frac{1}{2}||w||^2 + C\sum_{i = 1}^m \xi
$$
$$
y^{(i)}(w^T x^{(i)} + b) \geq 1 - \xi_i,\,\, \xi_i \geq 0,\,\,i = 1, ..., m
$$

#### Lagrange Duality

Find alpha:
$$
\max_\alpha\sum_{i = 1}^n \alpha_i - \frac{1}{2}\sum_{i = 1}^n \sum_{j = 1}^n \alpha_i\alpha_j y^{(i)} y^{(j)}(\phi(x^{(i)}\cdot \phi(x^{(j)}))
$$

$$
\theta =  \sum_{i = 1}^m\alpha_iy^{(i)}x^{(i)}\\
\theta \cdot x = \sum_{i = 1}^m \alpha_i y^{(i)} \langle x^{(i)}, x \rangle,
$$



### L7 Kernel

Sometimes data are not linearly separable, so we can map data into a higher dimensional space.

![[Pasted image 20221225182404.png]]

$$
k(\bar u,\bar v) = \phi(\bar u) \cdot \phi(\bar v)
$$
Linear
$$
K(\bar u,\bar v) = \bar u \cdot \bar v
$$
Quadratic
$$
K(\bar u,\bar v) = (\bar u \cdot \bar v+r)^2, r\geq 0
$$
RBF (Gaussian)
$$
K(\bar u,\bar v) = \exp(-\gamma \|\bar u - \bar v\|^2)
$$
##### Mercer's Theorem

A function $K: \mathbb{R}^d \times \mathbb{R}^d \to \mathbb{R}$ is a valid kernel iff the matrix $G$ with $G_{ij}=K(\bar x^{(i)}, \bar x^{(j)})$ is positive-semidefinite:

1. symmetric $G = G^T$
2. $\forall z \in \mathbb{R}, \,z^TG z \geq 0$​​ 

In other words, for such a function $K(\bar u, \bar v)$, there exists a function $\phi$ such that $K(\bar u, \bar v) = \phi(\bar u) \cdot \phi (\bar v)$.

##### Bias-Variance Tradeoff

Variance: estimation error

Bias: structural error

Normally we have to tradeoff between variance and bias.

##### Regularization
$$
J_{n,\lambda}(\theta) = \lambda Z(\theta) + R_n(\theta)
$$
E.g., SVM can be viewed as optimizing the hinge loss function with L2 regularization on $\theta$.



# L8

**Linear Regression**
$$
R_n(\theta) = \frac{1}{n}\sum_{i = 1}^n \text{Loss}(y^{(i)} -(\theta \cdot x^{(i)}))
$$
**Square Loss**
$$
Loss(z) = \frac{z^2}{2}
$$
**SGD with Square Loss**
$$
\theta^{(k+1)} = \theta^{(k)} - \eta (y^{(i)} - \theta^{(k)} \cdot x^{(i)})x^{(i)}
$$
**Closed form solution**
$$
\theta = (X^T X)^{-1} X^T y
$$
**Ridge Regression**
$$
J_{n,\lambda}(\theta) = \frac{1}{2n}\sum_{i = 1}^n (y^{(i)} - \theta^{(k)} \cdot x^{(i)})^2 + \lambda \frac{\norm{\theta}^2}{2}
$$

$$
\theta = (\lambda I + X^T X)^{-1} X^T y
$$

# L9 Decision Tree

==**Feature selection**==

**Embedded Methods**

Incorporate variable selection as part of the training process.

E.g. regularization on parameters.

**Filter Approach**

E.g. correlation with output
$$
r_{x_i,y} = \frac{\sum_{i=1}^n(x_j^i - \bar{x_j})(y^i-\bar{y})}{\sqrt{\sum_{i=1}^n(x_j^i-\bar{x_j})^2}\sqrt{\sum_{i=1}^n(y^i-\bar{y})^2}}
$$
**Wrapper**

Utilize learning algorithm to score subsets according to predictive power.

<img src="./image/9.1.png" style="zoom:50%;" />

## **Decision Tree**

Problem: classification

<img src="./image/9.2.png" style="zoom:50%;" />

Feature space could be continuous.

Decision trees can represent classification and regression problems.
$$
Entropy(S_n) = \sum_{i=1}^n -p_i \log p_i,\\
H(Y) = -\sum_{i=1}^k Pr(Y=y_i) \log Pr(Y=y_i),\\
H(Y|X=x_j) = -\sum_{i=1}^k Pr(Y=y_i|x_j) \log Pr(Y=y_i|x_j),\\
H(Y|X) = \sum_{j=1}^m Pr(X=x_j) H(Y|X=x_j),\\
IG(X,Y) = H(Y)-H(Y|X)
$$
**Greedy Approach**

Split on the "best" feature

**Stopping Criterion**

1. All records have the same label
2. All records have identical feature.
3. All attributes have zero IG.

LAPTOP-P9O77UT7

# L10 Random Forests

**Sources of Error**

1. Bias: structural error (underfitting)
2. Variance: estimation error (overfitting)

Larger hypothesis class can reduce bias, but can also increase variance if data is noisy.

**Ensemble Methods**

Goal: decrease variance without increasing bias.

Average across multiple models to reduce variance.

**Bootstrap Sampling**

Choose samples in the original dataset with uniform distribution with replacement until the size of the generated dataset is the same as original.

**Bagging**

Train each model on bootstrap sample and poll among the model predictions.

<img src="./image/10.1.png" style="zoom:50%;" />

Reduces variance while bias may still remain.

**Random Forest**

Use bagging & random feature subset.

1. Compute IG for all features.
2. Pick top m.
3. Select at random from these.



# L11 Boosting

**AdaBoost**

Idea: during every naive classification, increase the weight of points that are wrongly classifies, and decrease those are correct.

AdaBoost optimizes exponential loss.
$$
h_M(x) = \sum_{i=1}^M \alpha_i h(x;\theta_i)\\
h(x) = \text{sign}(h_M(x))\\
$$

**Loss Function**

Minimize exponential loss by doing gradient descend.
$$
Loss_{exp}(z) = exp(-z)\\
Loss = \sum_{i=1}^M Loss_{exp}(y^{(i)} h_m(x^{(i)}))\\
h(x;\theta) = \text{sign}(\theta_1(x_k - \theta_0))\\
\text{where } \theta = (k,\theta_0, \theta_1)\quad \text{(coordinate, position, direction)}\\
J(\alpha_m, \theta^{(m)}) = \sum_{i=1}^n Loss_{exp}(y{(i)} h_m(x^{(i)})) = \sum_{i=1}^n\exp\{-y^{(i)} \alpha_m h(x^{(i)} \theta_m)\}w_{m-1}(i)
$$

**AdaBoost Algorithm**

1. set $w_0(i) = \frac{1}{n}$ for $i=1,...,n$.
2. for $m=1,...,M$:

$$
\theta_m = \arg \min_{\theta} \epsilon_m = \sum_{i=1}^n w_{m-1}(i)[[y^{(i)} \neq h(x^{(i)}; \theta)]]\\
\hat{\epsilon}_m = \sum_{i=1}^n w_{m-1}(i)[[y^{(i)} \neq h(x^{(i)}; \theta_m)]]\\
\alpha_m = \frac{1}{2} \ln \frac{1- \hat{\epsilon}_m}{\hat{\epsilon}_m}\\
w_m(i) = \frac{\exp \{-y^{(i)} h_m(x^{(i)})\}}{z_m} = \frac{w_{m-1}(i)\exp\{-y^{(i)} \alpha_m h(x^{(i)}; \theta_m)\}}{z_m'}\\
\text{for } i = 1, ..., n
$$

3. Output classifier 
   $$
   h_M(x) = \sum_{m=1}^M \alpha_m h(x;\theta_m)
   $$

**High level view**

1. train a weak classifier parameterized by $\theta_m$ on weighted data.
2. based on its performance assign a classifier weight $\alpha_m$
3. based on ensemble predictions, upweight misclassified, downweigh correctly classified data. 

Weighted error relative to updated weights is exactly 0.5.

Weighted error tends to increase with each boosting iteration.





# L12 Neural Networks

Activation Functions

1. threshold (sign)
2. logistic (0 to 1)
3. hyperbolic tangent (tanh, -1 to 1)
4. ReLU (max(0,z))

**Fully connected NN**

<img src="./image/12.1.png" style="zoom:50%;" />

$w_{ki}^{(j)}$​ is the weight of layer j, unit k, input i.



# L13, 14 Training NN

**SGD**

1. Initialize parameters to small random values.
2. Select a point at random.
3. Update the parameters based on that point and the gradient:

**Backprop**

1. Make a prediction (forward propagation).

2. Measure the loss of the prediction.

3. Go through each layer in reverse to measure the error contribution of each propagation.

4. Tweak weight to reduce error.

Start with 1-2 hidden layers and ramp up until you start to overfit.
$$
w_{ij}^{(k+1)} = w_{ji}^{(k)}+\eta_k y[[(1-yz) > 0]]v_j[[z_j>0]]x_i
$$


**Reducing Overfitting**

1. Early stopping
2. L1 & L2 regularization
3. Dropout



# L15 CNN

## CNN architecture

**Convolution**

input: image, filter

output: convolved feature of feature map

HxWxD

<img src="./image/15.1.png" style="zoom:50%;" />

<img src="./image/15.2.png" style="zoom:50%;" />

A sliding window

**Rectified Linear Unit (ReLU)**

Apply non-linear function on each cell.

**Pooling**

Idea: reduce dimensionality of feature map while retaining significant information.

Control overfitting.

E.g. max pooling

<img src="./image/15.3.png" style="zoom:50%;" />

**Fully Connected Layer**

**Softmax neurons**
$$
y_i = \frac{e^{z_j}}{\sum_k e^{z_k}}
$$
<img src="./image/15.4.png" style="zoom:50%;" />

**Recurrent Neural Network**

<img src="./image/16.1.png" style="zoom:50%;" /> 



# L16 Unsupervised learning

# L17 K-Means Clustering

**Input**
$$
S_n = \{\bar x^{(i)} \}_{i = 1}^n
$$
**Output**

A set of cluster assignments $c_1, ..., c_n$.

**Goal**

Assign similar points to the same clusters.

Partition data into k clusters such that the sum of squares of Euclidean distances of each points to its cluster's mean is minimized.

**Algorithm**

Iteratively do following until convergence. (guaranteed)

1. For each points, reassign x to the cluster with closest distance to cluster mean.
2. Re-compute each cluster mean.

**How to initialize the means**

Pick clustering with lowest objective function value.

**How to pick k**

Elbow method.

k-means performs **coordinate descent** on the objective function.

But not guaranteed to converge to global minimum.

**k-means++ Approximation Guarantee**

The expected value of the objective returned by k-mean++ is never more than $O(\log K)$.

# L18 Hierarchical Clustering

**Idea**

Start with each point in its own cluster and build a hierarchy.

1. Assign each point its own cluster.
2. Find the closest clusters and merge them until convergence.

Find a cut for k clusters.

**Linkage Criteria**

Single-linkage: closest points.

Complete-linkage: farthest points.

Average-linkage: average every combination of points in the two clusters.

Centroid-linkage: distance the centroids of two clusters.



# L19 Spectral Clustering

Reformulated data clustering problem as minimum graph cut problem.

**Gaussian kernel similarity metric**
$$
w_{ij} = \exp (-\frac{||x^{(i)} - x^{(j)}||^2}{2\sigma^2})
$$
**Cost of a cut**
$$
cut(A, \bar A) = \sum_{i \in A, j \in \bar A} w_{ij}
$$
**Ratio cut**
$$
RatioCut(A_1, ..., A_k) = \frac{1}{2}\sum_{i=1}^k\frac{cut(A_i, \bar A_i)}{|A_i|}
$$
**Graph Laplacian**
$$
L = D - W
$$
Goal: minimizing the ratio cut, which is similar to the following problem.

L is symmetric, PSD, has n non-negative real-valued eigenvalues.

The multiplicity of the eigenvalues 0 is the number of connected components in the graph.

**Steps**

1. Build adjacency matrix $W$.
2. Compute graph Laplacian $L = D - W$.
3. Compute eigenvectors of $L$.
4. Build matrix with the first k eigenvectors as columns interpret rows as new data points.
5. Apply k-means to new data representation.

Output: cluster assignments.

 <img src="./image/19.1.png" style="zoom:50%;" />

High performance but not very efficient.



# L20 Collaborative Filtering

## 1 Matrix Factorization

 <img src="./image/20..png" style="zoom:30%;" />

**Idea**

Construct low rank $\hat Y$ to approximate $Y$.

**Theorem**

 <img src="./image/20.2.png" style="zoom:45%;" />
$$
\hat Y = UV^T\\
\hat Y_{ai} = \bar u^{(a)} \cdot \bar v^{(i)} \\ 
J(U,V) = \frac{1}{2}\sum_{(a,i)\in D}(Y_{ai} - \bar u^{(a)} \cdot \bar v^{(i)})^2 + \frac{\lambda}{2}(\sum_a || \bar u^{(a)}||^2 + \sum_i || \bar v^{(i)}|| ^2)
$$
U contains relevant features of the users. (each row)

V contains relevant features of the movie.

Minimize the lost function.

**Algorithm**

1. Fix V and solve U.
2. Fix U and solve V.

## 2 Nearest Neighbor Prediction

$$
\tilde Y_{a:b} = \text{a's average rating of movies rated by both a and b}
$$

**Sample Pearson's correlation**
$$
sim(a, b) = \frac{\sum_{j \in R(a,b)}(Y_{a_j}-\tilde Y_{a:b})(Y_{b_j} - \tilde Y_{b:a})}{\sqrt {\sum_{j \in R(a,b)} (Y_{a_j} - \tilde Y_{a:b})^2 \cdot\sum_{j \in R(a,b)} (Y_{b_j} - \tilde Y_{b:a})^2}}
$$
Measure of linear relationship between two variables, ranging from [-1, 1].

kNN: k nearest neighbors who have rated movie i.
$$
\hat Y_{a_i} = \bar Y_a + \frac{\sum_{b \in kNN(a, i)}sim(a,b)(Y_{b_i} - \bar Y_b)}{\sum_{b \in kNN(a, i)}|sim(a,b)|}
$$

Choose k

Neighbors with low correlation tend to introduce noice.

20-50 is a reasonable range.



# L21 Generative Models

Discriminative: learned a separator to discriminate two classes.

Generative: better understanding of where our data came from.

# L22 - 23 MLE

**Assumption**

Data are generated i.i.d from an unknow distribution having parameter $\theta$.

**Intuition**

Estimate parameter as the one that maximize the probability of observed outcome.

**Gaussian distribution**

The MLE result is:
$$
\mathcal{N}(x|\mu, \sigma^2) = \frac{1}{\sqrt{2\pi\sigma^2}}\exp -\frac{(x - \mu)^2}{2\sigma^2}  \\
\mu_{MLE} = \sum_{i = 1}^n \frac{x^{(i)}}{n}\\
\sigma^2_{MLE} = \sum_{i = 1}^n\frac{(x^{(i)} - \mu_{MLE})^2}{n}
$$
**Multivariate Gaussian Distribution**

<img src="./image/22.1.png" style="zoom:40%;" />
$$
\Sigma = E[(\bar x - \bar \mu) (\bar x - \bar \mu)^T]\\
|\Sigma| = \det \Sigma
$$
$\Sigma$ measures the covariance between x.

**Spherical Gaussian**
$$
\Sigma = \sigma ^2 \mathbf{I}_d\\
\bar{\mu}_{MLE} = \sum_{i = 1}^n \frac{\bar x^{(i)}}{n}\\
\sigma^2_{MLE} = \sum_{i = 1}^n\frac{||\bar x^{(i)} - \bar{\mu}_{MLE}||^2}{nd}
$$
**Gaussian mixture model (GMM)**
$$
\Pr (\bar x^{(i)}| \bar \theta) = \gamma_j N(\bar x | \bar \theta)\\
$$
$j$ is the corresponding cluster and $\gamma_j$ is the probability of that cluster.

MLE solution for known labels.

 <img src="./image/22.2.png" style="zoom:40%;" />

**Unknown labels**

Given the training data, find the model parameters that maximize the log-likelihood.

**Expectation Maximization for GMMs**

Iterate over convergence:

1. E step: use current estimate of mixure model to softly assign examples to clusters
2. M step: re-estimate each cluster model based on the points assigned to it.

**E-Step**
$$
p(j|i)=\frac{\gamma_j N(\bar x^{(i)} | \bar \mu_j, \sigma_j^2)}{\sum_t\gamma_t N(\bar x^{(i)} | \bar \mu_t, \sigma_t ^2)}
$$
**M-Step**

<img src="./image/22.3.png" style="zoom:40%;" />

How to pick k?

**Bayesian Information Criterion (BIC)**

l : log likelihood
$$
BIC(D;\bar \theta) = l(D;\bar \theta) - \frac{\#param}{2} log(n)
$$

# L24 Bayesian Networks

A probabilistic graphical model that represents a set of variables and their dependencies via a directed acyclic graph.

Node: variable.

Given its parent, a node $x$ is conditionally independent from all other nodes in the graph.

Directed edges: dependencies.

**Marginal independence**
$$
\Pr (X_1, X_2) = \Pr (X_1) \Pr (X_2)
$$
**Conditional Independence**
$$
\Pr(X_1, X_2,|X_3) = \Pr(X_1|X_3)\Pr (X_2|X_3)\\
X_1 \perp X_2 | X_3
$$
**d-separation**

1. Keep only "ancestral" graph of the variables of interest.
2. Connect nodes with common child and change graph into undirected.

If no path between variables of interest, then marginally independent.

If all path between variables of interest go through a particular node, then the variables are independent given that node (that node blocks the dependency).

### Learning Bayesian Networks

**Estimate parameters**

Use MLE, probability based estimation.

**Search over possible graph structures**



# L25 HMM

<img src="./image/25.1.png" style="zoom:40%;" /> 

Future depends on past via the present.

Current observation only depends on current state.

Current state only depends on last state.

E.g. observed data: words in a sentence; hidden state: POS tags.
$$
P(x_1, ..., X_T, z_1, ..., z_t) = \pi(z_1)\prod_{t=1}^{T-1}t(z_t, z_{t+1})\prod_{t=1}^T e(z_t, x_t)
$$
$\pi$: initial, $t$: transition, $e$: emission.

**Viterbi Algorithm**

Infer the underlying hidden states.

**Parameter Estimation**

Goal: estimate $\pi, t, e$.

If hidden states are available, use MLE.

**Incomplete Data**

1. M step: given the hidden states and estimate the parameters.
2. E step: fix the parameters and estimate the hidden states.









si
