### Naïve Bayes

* We want $P[label|data]$.
* Assumes that features are conditionally independent given the label.
* See [[Unsupervised Learning 445#L24 Bayesian Networks]].
	* The assumption is not reasonable.

$$
P[\text{label}|\text{features}]  \propto P(\text{label}) \prod_i P[\text{feature}_i|\text{label}]
$$

### Logistic Regression

* Also models $P[label|data]$.
* Fix the double counting problem of Naïve Bayes

![[Pasted image 20231030182303.png|600]]

### Support Vector Machine

* See [[Supervised Learning 445#L5 SVM]]
* Optimize the number of mislabeled examples.

### Evaluating Classifiers

* What if data are highly imbalanced?
* True positive rate TPR = TP/(TP+FN) (recall)
* True negative rate TNR = TN/(TN+FP)
* **Balanced Error Rate** BER = 1/2(FPR + FNR)
* `model = liner_model.LogisticRegression(C=1.0, class_weight='balanced')`

$$
F_\beta = (1+\beta^2) \cdot \frac{precision \cdot recall}{\beta^2 precision + recall}
$$
