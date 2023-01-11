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



## L9