### L1 Logistics

* Homework * 5 ungraded
* Quizzes * 5 (24hr window, 90min buffer) 50%
* Projects * 3 50%

### L2 Nearest Neighbor Classification

* Datadset: MNIST
* Problem: classify handwritten digit (0 - 9)
* Feature: 28 * 28 pixels, each is a grey scale (0 - 255)
* Algorithm: return the label of nearest (l2 distance) neighbor
* Test error: 3.09%
* K-NN: use k nearest neighbors
* Naive search takes $O(n)$ for training set of size $n$

### L3 Statistlcal Learning Framework

* In what situations does training data generalize to future data?
* Assumption: add data is drawn i.i.d. from some underlying distribution P on X x y.
* Problem: predict infected (0, 1) based on age (0 - 100) and temperature (97 - 105)
* Three ways to sample from P
	* Draw $(x, y) \sim P$
	* Draw y according to its marginal disbribution, then draw x according to the conditional distribution
	* Draw x and then y
* $\mu$: distribution on $\mathscr X$
* $\eta$: conditional disbribution $y|x$

##### Risk

For a classifier $h : \mathscr X \to \mathscr y$, risk is the probability that the classfier predict wrong for real points.

$$
R(h) = \Pr_{(x,y)\sim P}(h(x) \neq y)
$$

* **Bayes-optimal classifier** is the classifier with smallest possible risk.
* When $\mathscr y = \{0,1\},\,h^*(x) = \mathbb{1}[\eta(x) \geq 1/2]$.
* **Bayes's risk** $R^* = R(h^*) = \mathbb{E}[\min (\eta(x), 1 - \eta(x))]$

##### Consistency

* A learning algorithm is consistent iff $R(h_n) \to R^*$ as $n \to \infty$.
* The 1-NN classifier is not consistent. Consider the situation of uniform X and $\eta = 1/4$ on X. $R^* = 1/4, R(h) = \Pr[\text{two coins of bias 1/4 disagree}]=3/8$
* k-NN is consistent if $k$ grows with $n$.

### L4 Linear Regression

* 