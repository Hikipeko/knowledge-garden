
## L5 Neural Networks

Problem: linear classifiers aren't that powerful.

Solution: use feature transforms.

Neural network of two layers: first layer is bank of templates, while second layer recombines templates.

See [[Supervised Learning 445#L12 Neural Networks|Neural Networks]] from EECS 445.

#### Convex Functions

A function $f: X \subseteq \mathbb{N} \to \mathbb{R}$ is **convex** if for all $x_1, x_2 \in X, t \in [0, 1]$,

$$
f(tx_1 + (1-t)x_2) \leq tf(x_1) + (1-t) f(x_2).
$$

Convex functions are easy to optimize. However, most neural networks need nonconvex optimization.



## L6 Backpropagation

![[Pasted image 20230106153121.png|500]]

Problem: how to calculate the derivate?

Solution: use a computational graph. During the backward pass, each node in the graph receives *upstream gradients* and multiplies them by *local gradients* to compute *downstream gradients*. 

##### Sigmoid

$$
\begin{align}
\sigma(z) &= \frac{1}{1 + e^{-x}} \\
\frac{\partial(\sigma(z))}{\partial z} &= (1- \sigma(z))\sigma(z)
\end{align}
$$

##### Patterns in Gradient Flow

![[Pasted image 20230106154622.png]]

##### Backprop Implementation

```python
def forward(inputs):
	for gate in self.graph.nodes_topological_sorted():
		gate.forward()
	return loss

def backward():
	for gate in reversed(self.graph.nodes_topological_sorted()):
		gate.backward()
	return inputs_gradients
```

##### Backprop with Vectors

If x and y are vectors, then the derivative is **Jacobian**: 

$$(\frac{\partial y }{\partial x})_{i,j} = \frac{\partial y_j}{\partial x_j}$$

Multiplication of derivatives becomes matrix multiplication.

![[Pasted image 20230106160259.png]]

##### Backprop with Matrix

![[Pasted image 20230106160641.png]]

![[Pasted image 20230106161048.png]]



## L7 Convolutional Networks

Problem: the neural network classifiers don't respect the spatial structure of images.

Solution: use convolutional layers!

**Convolve** just means slide over the image spatially and compute the dot product.

![[Pasted image 20230106162451.png]]

##### Padding

Use padding to prevent feature maps from "shrinking". Add zeros around the input.

##### Receptive Fields

For convolution with kernel size K, each element in the output depends on a K x K **receptive field** in the input. Each successive convolution adds K-1 to the receptive field size. So for large images, we need many layers for each output to "see" the whole image.

![[Pasted image 20230106163856.png]]

##### Strided Convolution

To solve this problem, we increase the step size of the slide. E.g. a stride=2 will half the width and height of the input.

![[Pasted image 20230106164540.png]]

There're also 1D Convolution and 3D Convolution.

##### Pooling Layers

Idea: reduce dimensionality of feature map while retaining significant information.

Control overfitting, usually max pooling.

![[15.3.PNG|600]]

![[Pasted image 20230106165440.png|600]]

##### Fully Connected Layer

Usually used as the last few layers.

##### SoftMax Neurons

Used as the final layer for classification.

$$
y_i = \frac{e^{z_j}}{\sum_k e^{z_k}}
$$
##### Classic Architecture

\[Conv, BN, ReLU, Pool\] x N, Flatten, \[FC, BN, ReLU\] x N, FC, SoftMax

#### Batch Normalization

We normalize the outputs of a layer across images in a batch so that they have zero mean and unit variance. This can make the optimization process easier. Also, since the batch normalization is a differentiable function, we can use it as an operator in our networks and do backprop. Invented around 2014.

Problem: this constraint might be too strict. Solution: add a learn $\gamma, \beta \in \mathbb{R}^D$ to "regain" mean and variance.

$$
\begin{align}
\hat x_{i,j} &= \frac{x_{i,j} - \mu_i}{\sqrt{\sigma_j^2 + \epsilon}}\\
y_{i,j} &= \gamma_j \hat x_{i,j} + \beta_j
\end{align}
$$

Problem: we don't have batch when testing, and normalization mixes information from other images in the batch.

Solution: compute a running average of all $\mu_j, \sigma_j$, and use that average when testing.

Batch normalization for fully-connected layer and convolutional layer. Usually put after fc/conv and before nonlinearity.

![[Pasted image 20230106173442.png]]

BN = faster + more robust to initialization + regularization + zero test-time overhead.

##### Layer Normalization

Normalize over the channel dimension. Thus is has the same behavior at train and test. Used in RNNs, Transformers.

##### Instance Normalization

Normalize over both the channel dimension and the batch dimension. It has the same behavior at train and test.

![[Pasted image 20230106191735.png]]



## L8 CNN Architectures1

##### AlexNet

![[Pasted image 20230106192349.png]]

##### VGG-19

Change the trend into designing the whole Network. Fix the convolution layer to 3x3 instead of tuning it.

![[Pasted image 20230106193237.png]]

##### GoogleLeNet

Much less memory, parameters and MFLOP. Use 1x1 convolution layers to reduce MFLOP. Use a small FC layer with *global average pooling* at the end.

![[Pasted image 20230106201527.png|600]]

##### ResNet

Residual Networks implements batch normalization. Finds that deeper model does worse than shallow model! Found that the deeper model seems to be *underfitting*.

**Hypothesis**: deeper models are *harder to optimize*, they don't learn identity functions to emulate shallow models.

Solution: change the network so learning identity functions with extra layers is easy by using  a residual block.

![[Pasted image 20230106202718.png]]

![[Pasted image 20230106203626.png]]

Numbers of blocks for each stage are chosen experimentally. Use *bottleneck residual block*.

Hundreds of millions of learnable parameters are learned from about 10 millions images and they seem to not overfit. How is that even possible e.g. from a Rademacher complexity point of view? An active area for theoretical research.

![[Pasted image 20230106204758.png]]



## L9 Training Neural Networks I

##### Activation Functions

$$
\begin{align}
h_i^{(l)} &= \sum_j w_{i,j}^{(l)} \sigma(h_j^{(l-1)}) + b_i^{(l)} \\
\frac{\partial L}{\partial w_{i,j}^{(l)}} &= \sigma(h_j^{(l-1)}) \cdot \frac {\partial L}{\partial h_i^{(l)}}
\end{align}
$$



![[Pasted image 20230105140120.png]]

* **Sigmoid** is historically popular, but it "kills" the gradient of saturated neurons, slowing down the training process.
* **Tanh** is similar to sigmoid.
* **ReLU** is fast, but also has the gradient problem when $x < 0$. Batch normalization seems to solve this problem.
* **Leaky ReLU** is ReLU that will not "die".
* **ELU** has all benefits of ReLU, except the computation is expensive.
* **SELU** is a scaled ELU with self-normalization property. Batch normalization (expensive) is not needed if SELU is used.
* **GELU** is common in Transformers

![[Pasted image 20230110215956.png|600]]

Don't think too hard, just use ReLU.

##### Data Preprocessing

Typically zero-centering + normalization

PCA and whitening, not commonly used for images

ResNet uses subtract per-channel mean + divide by per-channel std.

#### Weight Initialization

If the initial weight of a layer is identical, it will keep to be identical in the whole training process. 
What if we initialize to small random numbers? For a deep NN, as we get deeper and deeper, the activations tend to be zero, and the gradient are all zero. And if we initialize to large random number, the local gradient will vanish to zero. *Proper initialization is an active area of research.*

![[Pasted image 20230110224524.png]]

##### Kaiming/MSRA

`W = np.random.rand(Din, Dout) / np.sqrt(2/Din)` produces a size that is "just right" for ReLU.

For conv layers, Din is `kernel_size^2 * input_channels`.

##### Residual Networks

Initialize first conv with MSRA and second conv to zero.

#### Regularization

##### Dropout

In each forward pass, randomly set some neurons to zero with a pre-set probability. 0.5 is common.

![[Pasted image 20230110231143.png]]

We want that the network to have a redundant representation rather than relying so much on a single representation.

For ResNet and later, often L2 and BN are the only regularizers.



## L10 Training Neural Networks II

#### Data Augmentation

Another way of regularization. Be aware of the **invariances**, e.g. don't rotate the image 180 degree in a digit recognition task.

* **Horizontal flips**
* **Random crops and scales** (recognize a cat even if only seeing the head)
* **Color Jitter**
* **CutOut**: set random image regions to 0
* **RandAugment**: apply N random augmentation transformations with magnitude M

During testing, we average (e.g. average the probability) a fixed se of augmented images.

#### Regularization Summary

* Training: add some randomness
* Testing: marginalize over randomness

* **Dropout** (used in ViTs)
* **Batch normalization**
* **Data augmentation**
* **DropConnect**: randomly drop connections between neurons (not commonly used)
* **Fractional pooling**: use randomized pooling regions (not commonly used)
* **Stochastic depth**: skip some residual blocks in ResNet
* **Mixup**: train on random blends of images
* **Label smoothing**: set target distribution to slightly lower than 1. Useful for mislabeled data.

#### Learning Rate Schedules

In general, we want to start with high learning rate and decay over time.

* **Step**: reduce learning rate at a few fixed points (e.g. * 0.1 after every 30 epochs)
* **Cosine**: no extra hyperparameter, *recommended by JJ*
* **Linear**
* **Inverse Sqrt**
* **Constant**: choose it as your starting point

![[Pasted image 20230122204253.png|300]] ![[Pasted image 20230122203758.png|300]]

##### Early Stopping

We keep track of the model snapshot that work best on validation set.

#### Choosing Hyperparameters

* **Grid search**: set a grid, and evaluate all possible choices
* **Random search**: sample log-uniform on a range, generally a better choice

![[Pasted image 20230122204811.png]]

![[Pasted image 20230122205046.png]]

What if you don't have tons of GPU?

1. Check initial loss
2. Overfit a small sample (100% training accuracy on a small sample of training data)
3. Find LR that makes loss go down (within the first few iterations)
4. Train 1-5 epochs on a coarse grid
5. Refine grid, train longer
6. Look at learning curves: training loss curve and validation accuracy
7. Go to step 5

![[Pasted image 20230122210116.png]]

![[Pasted image 20230122210609.png|300]] ![[Pasted image 20230122210650.png|300]]

#### After Training

We can use [[Supervised Learning 445#Ensemble Methods|model ensembles]] to get 2% extra performance. Instead of training independent models, we can use multiple snapshots of a single model during training.
