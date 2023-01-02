
A computer program is said to learn form experience E with respect to some task T and some performance measure P, if its performance on T, as measured by P, improves with experience E.

**Supervised Learning**

In supervised learning, we are given a data set and already know what our correct output should look like, having the idea that there is a relationship between the input and the output.

**Unsupervised Learning**

Unsupervised learning allows us to approach problems with little or no idea what our results should look like. We can derive structure from data where we don't necessarily know the effect of the variables.

## 1. Linear Regression with One Variable

**m** = Number of training examples

**x**'s = "input" variable / features

**y**'s = "output" / "target" variable

 <img src="D:\OneDrive\knowledge-garden\Computer Science\Machine Learning\吴恩达\image\1.png" alt="image-20211113012536953" style="zoom:50%;" />

#### cost function

 <img src="D:\OneDrive\knowledge-garden\Computer Science\Machine Learning\吴恩达\image\2.png" alt="image-20211113141612162" style="zoom:50%;" />

#### Gradient descent

The gradient descent algorithm is:

repeat until convergence:

 <img src="D:\OneDrive\knowledge-garden\Computer Science\Machine Learning\吴恩达\image\3.png" alt="image-20211113181956792" style="zoom:50%;" />

 <img src="D:\OneDrive\knowledge-garden\Computer Science\Machine Learning\吴恩达\image\4.png" alt="image-20211113182644075" style="zoom:40%;" />

## 2. Linear Algebra Review



## 3. Linear Regression with Multiple Variables

 <img src="./image/5.png" alt="image-20211115002640448" style="zoom:50%;" />

 <img src="./image/6.png" alt="image-20211115002700039" style="zoom:50%;" />

 <img src="./image/7.png" alt="image-20211115003250080" style="zoom:50%;" />

**Feature Scaling**

Get every feature into approximately a [-1, 1] range.

**Mean Normalization**

 <img src="./image/8.png" alt="image-20211115003925460" style="zoom:50%;" />

**Debugging gradient descent.** Make a plot with *number of iterations* on the x-axis. Now plot the cost function, J(θ) over the number of iterations of gradient descent. If J(θ) ever increases, then you probably need to decrease α.

**Polynomial Regression**

We can **change the behavior or curve** of our hypothesis function by making it a quadratic, cubic or square root function (or any other form).

One important thing to keep in mind is, if you choose your features this way then feature scaling becomes very important.

**Normal Equation**

  <img src="./image/9.png" alt="image-20211117112030272" style="zoom:50%;" />

no need to do feature scaling

Slow if n is very large, Inverse is O(n&^3). Use when n < 10,000.



## 4. Octave/MatLab Tutorial

```matlab
%% Basic
disp(sprintf('%0.2f', pi));
v = 1:0.1:10;
ones(2,3); zeros(2,3);  rand(2,3);  eye(4); randn(3,3); % normal distribution
hist(x); %draw x's distribution
load name.dat;
save name.mat dataName;
save name.txt -ascii; % save as human readable
A([1 3], :);
A'; % transpose
max(A);
A < 3; % element wise operation
[c, r] = find(A >= 7);
sum(A); prod(A); floor(A); ceil(A);
A(:) % turn into vector
sum(A, 1); % sum the columns sum(A, 2) sum the rows;
flipud(A); fliplr(A);
pinv(A); % inverse of A;

%% Plot
plot(x, y); hold; close;
legend('sin', 'cons'); title('plot name');
print -dpng 'name.png'; % save as image
figure(1); figure(2); % create more windows for plot
subplot(1,2,1);
axis([0.5 1 -1 1]); % change the axis range;
imagesec(A); % print a matrix

%% Loop
for i = 1:10,
    A(i) = 2^i;
end;
while i <= 5,
	v(i) = 100;
	i = i + 1;
end;
%% Branch
if v(1) == 1,
	disp('Value is 1');
elseif v(1) == 2,
	disp('Value is 2');
else
	disp('Other values');
end;
%% Function
% filename should be functionName.m
% y: return value, x: inputs
function [y1, y2] = squareAndCube(x)
	y1 = x^2;
	y2 = x^3;

%% Using linera algebra libraries

```



## 5. Logistic Regression

**Classification**

Predict a discrete result: $h_\theta(x) = 0 \text{ or } 1$.

**Logistic Regression**

 <img src="./image/10.png" alt="image-20211118155924419" style="zoom:50%;" />

$h_\theta(x)$ will give us the **probability** that our output is 1.

 <img src="./image/11.png" alt="image-20211118161039958" style="zoom:50%;" />

**cost function**

 <img src="./image/12.png" alt="image-20211118162332216" style="zoom:50%;" />

 <img src="./image/13.png" alt="image-20211118162351841" style="zoom:50%;" /><img src="./image/14.png" alt="image-20211118162420571" style="zoom:50%;" />

 <img src="./image/15.png" alt="image-20211126213515695" style="zoom:50%;" />

 <img src="./image/16.png" alt="image-20211126213526942" style="zoom:50%;" />

 <img src="./image/17.png" alt="image-20211126213627700" style="zoom:50%;" />

 <img src="./image/18.png" alt="image-20211126213734208" style="zoom:50%;" />

 <img src="./image/19.png" alt="image-20211126213750046" style="zoom:50%;" />

 <img src="./image/20.png" alt="image-20211126213807911" style="zoom:50%;" />

**Advanced Optimization**

```matlab
function [jVal, gradient] = costFunction(theta)
  jVal = [...code to compute J(theta)...];
  gradient = [...code to compute derivative of J(theta)...];
end

options = optimset('GradObj', 'on', 'MaxIter', 100);
initialTheta = zeros(2,1);
   [optTheta, functionVal, exitFlag] = fminunc(@costFunction, initialTheta, options);
```



## Week 4

### Neural Networks



 <img src="./image/21.png" style="zoom:50%;" />

$$z^{(j+1)} = \Theta^{(j)}a^{(j)}$$

$$a^{(j)} = g(z^{(j)})$$​

**AND**

 <img src="./image/22.png" style="zoom:50%;" />

 <img src="./image/23.png" style="zoom:50%;" />

 <img src="./image/24.png" style="zoom:50%;" />



## Week5

### Cost Function

<img src="./image/25.png" style="zoom:50%;" />

 **Backpropagation Algorithm**

"Backpropagation" is neural-network terminology for minimizing our cost function, just like what we were doing with gradient descent in logistic and linear regression. Our goal is to compute: $$min_\Theta J(\Theta)$$.

 <img src="./image/26.png" style="zoom:50%;" />

 <img src="./image/27.png" style="zoom:50%;" />

**Unrolling Parameters**

```matlab
thetaVector = [ Theta1(:); Theta2(:); Theta3(:); ];
deltaVector = [ D1(:); D2(:); D3(:) ];
Theta1 = reshape(thetaVector(1:110),10,11);
Theta2 = reshape(thetaVector(111:220),10,11);
Theta3 = reshape(thetaVector(221:231),1,11);
```

 <img src="./image/28.png" style="zoom:50%;" />

**Gradient Checking**

  <img src="./image/29.png" style="zoom:50%;" />

```matlab
epsilon = 1e-4;
for i = 1:n,
  thetaPlus = theta;
  thetaPlus(i) += epsilon;
  thetaMinus = theta;
  thetaMinus(i) -= epsilon;
  gradApprox(i) = (J(thetaPlus) - J(thetaMinus))/(2*epsilon)
end;
```

**Random Initialization**

Initializing all theta weights to zero does not work with neural networks. When we backpropagate, all nodes will update to the same value repeatedly. Instead we can randomly initialize our weights for our \ThetaΘ matrices using the following method:

```matlab
If the dimensions of Theta1 is 10x11, Theta2 is 10x11 and Theta3 is 1x11.

Theta1 = rand(10,11) * (2 * INIT_EPSILON) - INIT_EPSILON;
Theta2 = rand(10,11) * (2 * INIT_EPSILON) - INIT_EPSILON;
Theta3 = rand(1,11) * (2 * INIT_EPSILON) - INIT_EPSILON;
```

**Training a Neural Network**

1. Randomly initialize the weights
2. Implement forward propagation to get h_\Theta(x^{(i)})*h*Θ(*x*(*i*)) for any x^{(i)}*x*(*i*)
3. Implement the cost function
4. Implement backpropagation to compute partial derivatives
5. Use gradient checking to confirm that your backpropagation works. Then disable gradient checking.
6. Use gradient descent or a built-in optimization function to minimize the cost function with the weights in theta.



## Week6

### Evaluating a Learning Algorithm

**Test Overfitting**

A hypothesis may have a low error for the training examples but still be inaccurate (because of overfitting). Thus, to evaluate a hypothesis, given a dataset of training examples, we can split up the data into two sets: a **training set** and a **test set**. Typically, the training set consists of 70 % of your data and the test set is the remaining 30 %. 

 <img src="./image/30.png" style="zoom:50%;" />

**Test Error**

 <img src="./image/31.png" style="zoom:50%;" />

One way to break down our dataset into the three sets is:

1. Training set: 60%
2. Cross validation set: 20%
3. Test set: 20% 

We can now calculate three separate error values for the three different sets using the following method:

1. Optimize the parameters in $$\Theta$$ using the training set for each polynomial degree.
2. Find the polynomial degree d with the least error using the cross validation set.
3. Estimate the generalization error using the test set with $$J_{test}(\Theta^{(d)})$$ (d = theta from polynomial with lower error);

### Bias vs. Variance

In this section we examine the relationship between the degree of the polynomial d and the underfitting or overfitting of our hypothesis.

1. We need to distinguish whether **bias** or **variance** is the problem contributing to bad predictions.

2. High bias is underfitting and high variance is overfitting. Ideally, we need to find a golden mean between these two.

 <img src="./image/32.png" style="zoom:50%;" />

**Regularization**



 <img src="./image/33.png" style="zoom:50%;" />

1. Create a list of lambdas (i.e. λ∈{0,0.01,0.02,0.04,0.08,0.16,0.32,0.64,1.28,2.56,5.12,10.24});
2. Create a set of models with different degrees or any other variants.
3. Iterate through the $$\lambda$$s and for each  $$\lambda$$ go through all the models to learn some $$\Theta$$.
4. Compute the cross validation error using the learned $$\Theta$$ (computed with $$\lambda$$) on the $$J_{CV}(\Theta)$$ **without** regularization or $$\lambda$$  = 0.
5. Select the best combo that produces the lowest error on the cross validation set.
6. Using the best combo $$\Theta$$ and $$\lambda$$, apply it on $$J_{test}(\Theta)$$ to see if it has a good generalization of the problem.

**Learning Curves**

**Experiencing high bias:**

If a learning algorithm is suffering from **high bias**, getting more training data will not **(by itself)** help much.

 <img src="./image/34.png" style="zoom:130%;" />

**Experiencing high variance:**

If a learning algorithm is suffering from **high variance**, getting more training data is likely to help.

<img src="./image/35.png" style="zoom:130%;" /> 

*Question: how should we chose the structure of neural network based on the problem?*

Our decision process can be broken down as follows:

- **Getting more training examples:** Fixes high variance

- **Trying smaller sets of features:** Fixes high variance

- **Adding features:** Fixes high bias

- **Adding polynomial features:** Fixes high bias

- **Decreasing λ:** Fixes high bias

- **Increasing λ:** Fixes high variance.

### Building a Spam Classifier

So how could you spend your time to improve the accuracy of this classifier?

- Collect lots of data (for example "honeypot" project but doesn't always work)
- Develop sophisticated features (for example: using email header data in spam emails)
- Develop algorithms to process your input in different ways (recognizing misspellings in spam).

#### Error Analysis

The recommended approach to solving machine learning problems is to:

- Start with a simple algorithm, implement it quickly, and test it early on your cross validation data.
- Plot learning curves to decide if more data, more features, etc. are likely to help.
- Manually examine the errors on examples in the cross validation set and try to spot a trend where most of the errors were made.

### Handling Skewed Data

Skewed data is the data that has extreme ratio (e.g. cancer rate).

To avoid bad prediction (e.g. predict all 0), we should consider precision $$P$$ and recall $$R$$.

 <img src="./image/36.png" style="zoom:40%;" />

$$F_1 = \frac{2PR}{P + R}$$​​.

### Using Large Data Sets

<img src="./image/37.png" style="zoom:40%;" /> 



## Week 7

### Large Margin Classification (Support Vector Machine)

 <img src="./image/38.png" style="zoom:40%;" />

**SVM hypothesis**

$$\text{min}_\theta \,\, C \sum_{i = 1}^m[y^{(i)}cost_1(\theta^Tx^{(i)})+(1-y^{(i)})cost_0(\theta^Tx^{(i)})] + \frac{1}{2}\sum_{i=1}^n \theta_j^2$$

 <img src="./image/39.png" style="zoom:40%;" />

### Kernels

**Similarity function**

 <img src="./image/40.png" style="zoom:40%;" />

$$f = \text{exp}(-\frac{||x - L||^2}{2\sigma^2})$$

 <img src="./image/41.png" style="zoom:40%;" />

<img src="./image/42.png" style="zoom:40%;" /> 



## Week 8

### Clustering

**K-means algorithm**

Initialize K cluster centroids $$\mu_1, ..., \mu_k \in \mathbb{R}^n $$

Repeat {

​	for $i$ = 1 to $m$, $c^{(i)}$ := index of cluster centroid closest to $x^{(i)}$

​	for k = 1 to K, $\mu_k$ := average of points assigned to cluster k

}

**Cost Function**

$$J(c^{(1)}, ..., c^{(m)}, \mu_1, ..., \mu_K) = \frac{1}{m} \sum_{i = 1}^m ||x^{(i)} - \mu_{c^{(i)}}||^2$$

Also called the distortion function.

The min of cost function is exactly the process above.

**Initialization**

Randomly pick K training examples for 100 times.

Can stuck into local optima.

**Choosing the number of clusters**

 <img src="./image/43.png" style="zoom:50%;" />

### Dimensionality Reduction

Data compression

Visulization

**Principal Component Analysis**

feature scaling & mean normalization

$$\Sigma = \frac{1}{m}\sum_{i = 1}^n (x^{(i)})(x^{(i)})^T$$

```matlab
Sigma = 1/m * X' * X;
[U, S, V] = svd(Sigma);
U_reduce = U(:, 1:k); % n * k
z = X * U % k * m
```

**Reconstruction from Compressed Representation**

```matlab
X = U_reduce * z
```

**Choosing k**

Pick smallest values of $$k$$ for which

$$\frac{\sum_{i=1}^kS_{ii}}{\sum_{i=1}^mS_{ii}} \ge 0.99.$$​

 <img src="./image/44.png" style="zoom:50%;" />

Don't use PCA to address overfitting.

Run PCA only when necessary.



## Week 9

### Density Estimation

**Anomaly Detection**

**Gaussian (Normal) distribution**

$$p(x;\mu, \sigma^2) = \frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$$

 <img src="./image/45.png" style="zoom:50%;" />

 <img src="./image/46.png" style="zoom:50%;" />

Anomaly detection is often used when there are only a small number of positive examples and large number of negative examples.

Supervised learning are often used when there're large number of positive and negative eamples.

**Choose features**

Use functions such as log or ^0.1 to make the distribution into Gaussian

**Error Analysis**

If an anomaly example is not predicted right, find out what features it have, and add this feature into the model.

### Recommender System

 <img src="./image/47.png" style="zoom:50%;" />

**Collaborative Filtering**

















