### 19 Randomized Algorithm

Employs a degree of randomness as part of its logic or procedure.

##### Deterministic Algorithm

Given the same input, will pass through the same sequence of states and produce the same output every time.

#### 19.1 Review of Probability

##### Linearity of Expectation

$$
X = \sum_i c_i \cdot X_i \quad \Rightarrow \quad
\mathbb{E}[X] = \sum_i c_i \cdot \mathbb{E}[X_i]
$$

##### Indicator

AKA 0-1 random variable. If $I$ is an indicator and $\mathbb{P}[I = 1] = p$, we have

$$
\mathbb{E}[X] = \mathbb{P}[X = 1] = p.
$$
#### 19.2 Randomized Approximations

##### Max-3SAT

Problem: satisfy as many clauses as possible given an exact 3CNF formula, an NP-Hard optimization problem.

If we assign variables randomly, the expectation of satisfied clauses is $7/8 m$.

##### Markov's Inequality

Let $X$ be a nonnegative random variable, and let $v > 0$, then

$$
\mathbb{P}[X \geq v] \leq \frac{\mathbb{E}[X]}{v}
$$

#### 19.3 Primality Testing

We need large prime numbers for [[Cryptography 376|cryptography]], so we need a method to test whether a randomly picked large number is prime.

Try from 1 to $\sqrt{m}$ is $O(2^n)$ since the input size is $\log m$.

##### Extended Fermat's Little Theorem

Let $n \leq 2, 1 \leq a \leq n - 1$, then:

* If $n$ is prime, then $a^{n-1} \equiv 1 \pmod{n}$ for any witness $a$.
* If $n$ is composite and $n$ is not a Carmichael number, then $a^{n-1}\equiv 1 \pmod{n}$ for at most half the witnesses $1 \leq a \leq n - 1$.

##### Fermat Primality Test

Test mod for all 

Thus we have Pr\[the Fermat test rejects $m$\] > 1/2.

#### 19.4 Skip Lists

#### 19.5 [[Sort 281#L11 Quicksort|Quick Sort]]

The expected number of comparisons is $O(n \log n)$.



### 20 Probabilistic Complexity Classes

##### Monte Carlo

Have a bound runtime, but they may produce the wrong answer.

##### Las Vegas

Always produces the right answer, but the runtime bound only holds in expectation.



### 21 Monte Carlo Methods and Chernoff Bounds

Monte Carlo methods is a way of approximation by repeated trails. (Different concept form Monte Carlo algorithms)

E.g. estimating pi

![[Pasted image 20221219225859.png|300]]

#### 21.1 Chernoff Bounds

![[Pasted image 20221219230200.png]]

#### 21.2 Hoeffding's Inequality

![[Pasted image 20221219230408.png]]

