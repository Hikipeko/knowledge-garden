### Review of Probability

* M: month, W: weather, S: sidewalk
* M => W => S
* S and M are not [[Unsupervised Learning 445#Marginal independence|marginally independent]]
* S is [[Unsupervised Learning 445#Conditional Independence|conditionally independent]] of M given W

##### Bayes Rule

$$
\begin{align}
P(X|Y) &= \frac{P(Y|X)P(X)}{P(Y)}\\
P(X|Y,E) &= \frac{P(Y|X,E)P(X|E)}{P(Y|E)}
\end{align}
$$

### Example of Commonsense Reasoning

![[Pasted image 20231013162800.png|300]]

* B: burglary, E: earthquake, A: alarm
* $P(B=1) = 0.001, \quad P(E=1) = 0.002$
* $P(B = 1) = 0.001$
* $P(B=1|A=1) = 0.37$
* $P(B=1|A=1,E=1) = 0.0033$
* **Explaining away**: the earthquake explains away the alarm, diminishing our belief in the burglary.