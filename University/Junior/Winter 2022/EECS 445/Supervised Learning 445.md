##### Goal

Given data $X$ and the labels $Y$, we wan to learn to predict $Y$ from $X$. Discrete labels are **classification** problems, and continuous labels are **regression** problems.

## L2 Linear Classifiers

##### Classification

Given the features, predict the labels as a discrete value.

##### Feature Vector

Statistics or attributes that describe the data, represented in terms of vectors.

#### Linear Classifiers

Goal: learn a linear decision boundary that minimizes training error.

Predict result as $sgn(\bar \theta \cdot \bar x + b)$

##### Hyperplane

A plane specified by a parameter vector $\bar{\theta}  \in \mathbb{R}^d$ and offset $b \in \mathbb{R}$. It is the set of point $\bar{x}$ such that $\bar\theta \cdot \bar{x} + b = 0$.

Linear separable vs. not linear separable.



## L3 Perceptron

An algorithm that gives a linear classifier. It repeatedly finds a misclassified point and do gradient descent on that point.

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



## L4 Gradient Descent

Take a small step opposite to which the gradient points.

$$
\bar\theta^{(k+1)} = \bar\theta^{(k)} - \eta \nabla_\bar\theta R_n(\bar\theta)|_{\bar\theta = \bar\theta^k}
$$

#### Stochastic Gradient Descent

Update based on a single points a time instead of on the whole dataset.

Often gets close to minimum faster than GD.

**Problems**

1. Jittering
2. Stuck in local minimum
3. Sample data points can be noisy

##### Momentum

SGD + Momentum. We build up a "velocity" as a funning mean of gradients. $\rho$ gives "friction", typically 0.9 or 0.99.

$$
\begin{align}
v_{t+1} &= \rho v_t + \nabla f(x_t) \\
x_{t+1} &= x_t - \alpha v_{t+1}
\end{align}
$$

##### AdaGrad

AdaGrad + SGD. Adaptive learning rate. Reduce learning rate when the gradient is large. **RMSProp** is a better version of AdaGrad.

##### Adam

![[Pasted image 20230105031416.png]]

(Almost) RMSProp + Momentum. Use *bias correction* to prevent big leaps in the first few iterations. Typically beta1 = 0.9, beta2 = 0.999, and learning rate = 1e-3, 1e-4, 1e-5.

The state-of-art version is **AdamW**.

##### L-BFGS

Use second order derivative. Not very practical.



## L5 SVM

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



## L7 Kernel

Sometimes data are not linearly separable, so we can map data into a higher dimensional space.

![[Pasted image 20221225182404.png]]

$$
k(\bar u,\bar v) = \phi(\bar u) \cdot \phi(\bar v)
$$

**Linear** $K(\bar u,\bar v) = \bar u \cdot \bar v$

**Quadratic** $K(\bar u,\bar v) = (\bar u \cdot \bar v+r)^2, r\geq 0$

**RBF (Gaussian)**$K(\bar u,\bar v) = \exp(-\gamma \|\bar u - \bar v\|^2)$

##### Mercer's Theorem

A function $K: \mathbb{R}^d \times \mathbb{R}^d \to \mathbb{R}$ is a valid kernel iff the matrix $G$ with $G_{ij}=K(\bar x^{(i)}, \bar x^{(j)})$ is positive-semidefinite:

1. symmetric $G = G^T$
2. $\forall z \in \mathbb{R}, \,z^TG z \geq 0$​​ 

In other words, for such a function $K(\bar u, \bar v)$, there exists a function $\phi$ such that $K(\bar u, \bar v) = \phi(\bar u) \cdot \phi (\bar v)$.



## L8 Ridge Regression

Normally we have to tradeoff between variance and bias.

#### Bias-Variance Tradeoff

![[Pasted image 20230102191329.png|500]]

##### Variance

The changes in the model when using different portions of the training data set, causing overfitting.

##### Bias

Structural error, a systematic error that can be caused by incorrect assumptions, leading to underfitting.

#### Regularization

$$
J_{n,\lambda}(\theta) = \lambda Z(\theta) + R_n(\theta)
$$

E.g., SVM can be viewed as optimizing the hinge loss function with L2 regularization on $\theta$.

#### Linear Regression

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

#### Closed Form Solution

To find the minimum loss, we let gradient = 0 and gets the following solution:

$$
\bar \theta^* = (X^T X)^{-1} X^T \bar y
$$

Also note that this is consistent with [[MATH 214#The Normal Equation|Normal Equation]] in MATH 214.

So why SGD? Efficiency.

If not invertible? That means there are [[MATH 214#^3d13bb|redundant vectors]] in $X$, and we need to delete redundant data points. 

#### Ridge Regression

$$
J_{n,\lambda}(\theta) = \frac{1}{2n}\sum_{i = 1}^n (y^{(i)} - \theta^{(k)} \cdot x^{(i)})^2 + \lambda \frac{\| \theta \|^2}{2}
$$

##### Closed Form

$$
\theta = (\lambda I + X^T X)^{-1} X^T y
$$



## L9 Decision Tree

#### Feature selection

##### Embedded Methods

Incorporate variable selection as part of the training process.

E.g. regularization on parameters.

##### Filter Approach

E.g. correlation with output

$$
r_{x_i,y} = \frac{\sum_{i=1}^n(x_j^i - \bar{x_j})(y^i-\bar{y})}{\sqrt{\sum_{i=1}^n(x_j^i-\bar{x_j})^2}\sqrt{\sum_{i=1}^n(y^i-\bar{y})^2}}
$$

##### Wrapper

Utilize learning algorithm to score subsets according to predictive power.

![[9.1.PNG]]

### Decision Tree

Problem: classification. Feature space could be discrete or continuous. Decision trees can represent classification and regression problems.

![[9.2.PNG|450]]

Finding the smallest decision tree that minimizes error is an [[Complexity 376#NP-Hard|NP-hard]] problem.

##### Entropy

The average level of "information", "surprise", or "uncertainty" inherent to a random variable's possible outcomes.

$$
H(Y) = -\sum_{i=1}^k Pr(Y=y_i) \log Pr(Y=y_i)
$$

**Conditional Entropy**

$$
\begin{align}
H(Y|X=x_j) &= -\sum_{i=1}^k Pr(Y=y_i|x_j) \log Pr(Y=y_i|x_j),\\
H(Y|X) &= \sum_{j=1}^m Pr(X=x_j) H(Y|X=x_j)
\end{align}
$$

**Information Gain**

$$
IG(X,Y) = H(Y)-H(Y|X)
$$

##### Greedy Approach

Recursively split on the "best" feature that maximize information gain. But when should we stop?

##### Stopping Criterion

1. All records have the same label
2. All records have identical features
3. All attributes have zero IG (NG, -> 4)
4. Set a max depth
5. Post-prune, grow entire/full tree and then greedily remove nodes that affect validation error the least.



## L10 Random Forests

#### Ensemble Methods

Recall [[Supervised Learning 445#Bias-Variance Tradeoff|bias and variance]], we want to decrease variance without increasing bias. So we average across multiple models to reduce variance. E.g. we can learn multiple decision trees and average their predictions.

##### Bootstrap Sampling

Choose samples in the original dataset with uniform distribution with replacement until the size of the generated dataset is the same as original.

##### Bagging

Train each model on bootstrap sample and poll among the model predictions.

![[10.1.PNG|350]]

Reduces variance while bias may still remain.

##### Random Forest

Use bagging & random feature subset. However, in each repetition, best split is chosen from random subset of $k < d$ features.

```algorithm
for b = 1, ..., B
	draw bootstrap sample Sn(b) of size n from Sn
	grow decision tree DT(b)
```



## L11 Boosting

#### AdaBoost

![[Pasted image 20230102224949.png]]

Idea: during every naïve classification, increase the weight of points that are wrongly classifies, and decrease those are correct.

AdaBoost optimizes exponential loss.

$$
\begin{align}
h_M(x) &= \sum_{i=1}^M \alpha_i h(x;\theta_i)\\
h(x) &= \text{sign}(h_M(x))
\end{align}
$$

##### Loss Function

Minimize exponential loss by doing gradient descend.

$$
\begin{align}
Loss_{\exp}(z) &= \exp(-z)\\
Loss &= \sum_{i=1}^M Loss_{\exp}(y^{(i)} h_m(x^{(i)}))\\
h(x;\theta) &= \text{sign}(\theta_1(x_k - \theta_0))\\
\text{where } \theta &= (k,\theta_0, \theta_1)\quad \text{(coordinate, position, direction)}\\
J(\alpha_m, \theta^{(m)}) &= \sum_{i=1}^n Loss_{\exp}(y{(i)} h_m(x^{(i)}))\\
&= \sum_{i=1}^n\exp\{-y^{(i)} \alpha_m h(x^{(i)} \theta_m)\}w_{m-1}(i)
\end{align}
$$

##### AdaBoost Algorithm

1. set $w_0(i) = \frac{1}{n}$ for $i=1,...,n$.
2. for $m=1,...,M$:

$$
\begin{align}
\theta_m &= \arg \min_{\theta} \epsilon_m = \sum_{i=1}^n w_{m-1}(i)[[y^{(i)} \neq h(x^{(i)}; \theta)]]\\
\hat{\epsilon}_m &= \sum_{i=1}^n w_{m-1}(i)[[y^{(i)} \neq h(x^{(i)}; \theta_m)]]\\
\alpha_m &= \frac{1}{2} \ln \frac{1- \hat{\epsilon}_m}{\hat{\epsilon}_m}\\
w_m(i) &= \frac{\exp \{-y^{(i)} h_m(x^{(i)})\}}{z_m}\\
&= \frac{w_{m-1}(i)\exp\{-y^{(i)} \alpha_m h(x^{(i)}; \theta_m)\}}{z_m'}\\
\text{for } i &= 1, ..., n
\end{align}
$$

3. Output classifier 

$$
h_M(x) = \sum_{m=1}^M \alpha_m h(x;\theta_m)
$$

##### High level view

1. train a weak classifier parameterized by $\theta_m$ on weighted data
2. based on its performance assign a classifier weight $\alpha_m$
3. based on ensemble predictions, upweight misclassified, downweigh correctly classified data

Weighted error relative to updated weights is exactly 0.5.

Weighted error tends to increase with each boosting iteration.



## L12 Neural Networks

##### Activation Functions

![[Pasted image 20230105140120.png]]

1. threshold (sign)
2. logistic (0 to 1)
3. hyperbolic tangent (tanh, -1 to 1)
4. ReLU (max(0,z)) *a good default choice*

**Fully connected NN**

![[12.1.PNG]]

$w_{ki}^{(j)}$​ is the weight of layer $j$, unit $k$, input $i$.

##### Power of NN

![[Pasted image 20230105155412.png]]

##### Universal Approximation

NN with one hidden layer can approximate *any* functions $f: \mathbb{R}^n \to \mathbb{R}^m$ provided it has enough neurons. Each neural can provide a ReLU, and we can combine ReLU functions to approximate nearly any functions (remember from VE 216).

[Demo of ConvNeJS](https://cs.stanford.edu/people/karpathy/convnetjs/demo/classify2d.html)



## L13-14 Training NN

The same as all the other machine learning algorithms, our goal is to minimize the lose function.

##### SGD

1. Initialize parameters to small random values.
2. Select a point at random.
3. Update the parameters based on that point and the gradient:

##### Backpropagation

See [[Deep Learning 498#L6 Backpropagation|backpropagation]] from CV 498.

Start with 1-2 hidden layers and ramp up until you start to overfit.
$$
w_{ij}^{(k+1)} = w_{ji}^{(k)}+\eta_k y[[(1-yz) > 0]]v_j[[z_j>0]]x_i
$$

##### Reducing Overfitting

1. Early stopping
2. L1 & L2 regularization
3. **Dropout**: each unit might be turned off randomly at each iteration



## L15 CNN

See [[Deep Learning 498#L7 Convolutional Networks]].

#### Recurrent Neural Network

![[16.1.PNG]]

#link 487
