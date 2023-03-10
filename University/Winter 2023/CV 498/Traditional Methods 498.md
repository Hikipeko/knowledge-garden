## L1 Introduction

![[Pasted image 20230103203645.png|600]]

#### Brief History of CV and Deep Learning

![[Pasted image 20230103210232.png]]

Convolutional Network appears in 1998!



## L2 Image Classification

**Input**: image

**Output**: assign image to one of a fixed set of categories

##### Dataset

CIFAR10, CIFAR100, ImageNet, MIT Places

##### Nearest Neighbor

![[Pasted image 20230105012448.png]]

We memorize all data and labels and predict the label as the [[Unsupervised Learning 445#Nearest Neighbor Prediction|nearest neighbor]].

**L1 distance**: $d_1(I_1, I_2) = \sum_p |I_1^p - I_2^p|$

**L2 distance**: $d_2(I_1, I_2) = (\sum_p (I_1^p - I_2^p)^2)^{0.5}$

Take $O(N)$ time for prediction, really slow, so seldom use. However, Nearest Neighbor with ConvNet features works well, especially for image captioning.

##### Cross-Validation

![[Pasted image 20230105014125.png]]

Try every fold as validation and average the results. Useful for small datasets.



## L3 Linear Classifiers

See from EECS 445 [[Supervised Learning 445#L2 Linear Classifiers|linear classifier]].

![[Pasted image 20230105015814.png]]

##### Cross-Entropy Loss

$j$ is the correct class. Ranging from 0 to +inf. It is inspired by [[Unsupervised Learning 445#L22 MLE|maximum likelihood estimation]].

![[Pasted image 20230105021739.png]]

##### Multiclass SVM Loss

$$
L_i = \sum_{j \neq y_i} \max(0, s_j - s_{y_i} + 1)
$$

Idea: we want the correct class to be at least lager than other class + 1.



## L4 Regularization and Optimization

How do we get $W$?

Use [[Supervised Learning 445#L4 Gradient Descent|gradient descent]].

**Numeric gradient** is computed via small finite difference.

**Analytic gradient** is computed via calculus.

Normally we use [[Supervised Learning 445#Stochastic Gradient Descent|SGD]] with a *batch* of size 32/64/128.

```python
for t in range(num_steps):
	minibatch = sample_data(data, batch_size)
	w -= a * compute_gradient(loss_fn, minibatch, w)
```

##### Overfitting

A model performs too well on the training data, and has poor performance for unseen data.

We use [[Supervised Learning 445#Regularization|regularization]] to prevent overfitting.
