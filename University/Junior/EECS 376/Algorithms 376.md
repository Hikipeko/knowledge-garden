## 1 Introduction

**A6 (Euclid)**

```Algorithm
Algorithm Euclid(x, y):
	If y = 0 return x
	If y = 1 return 1
	Return Euclid (y, x mod y)
```



## 2 The Potential Method

Define a function $s: A \to \mathbb{R}$ that maps the state of the algorithm to a number. The function $s$ is called potential function if:

1. $s$ is strictly decreasing
2. $s$ has a lower bound

#### 2.1 A Potential Function for Euclid's Algorithm

$s_i = x_i + y_i$

**L10**	For all valid inputs $x,y$ to Euclid's algorithms, $s_{i+1} \leq \frac{2}{3}s_i$ for all iterations $i$ of $Euclid(x,y).$

**T12**	For inputs $x,y,$ the number of iterations of Euclid's algorithm is $O(log(x+y)).$



## 3 Divide and Conquer

Review [[EECS 281#Divide and Conquer|Divide and Conquer]] from EECS 281.

**A14** Merge sort

#### 3.1 Master Theorem

**T15**	$T(n) = kT(\frac{n}{b}) + O(n^d),$

$$ T(n)=\left\{
\begin{array}{lcl}
O(n^d)       	&	& ,{log_bk < d}\\
O(n^dlog n)     &	& ,{log_bk = d}\\
O(n^{log_bk})	&	& ,{log_bk > d}
\end{array} \right. $$

**T17**	$T(n) = kT(\frac{n}{b}) + O(n^dlog^w n),$

$$ T(n)=\left\{
\begin{array}{lcl}
O(n^dlog^wn)       	&	& ,{log_bk < d}\\
O(n^d log^{w+1}n)     &	& ,{log_bk = d}\\
O(n^{log_bk})	&	& ,{log_bk > d}
\end{array} \right. $$

#### 3.2.1 The Karatsuba Algorithm

$m_1 = (a+b)\times(c+d)$

$m_2 = a \times c$

$m_ 3 = b \times d$

$$
\begin{align}
N_1 \times N_2  &= a \times c \cdot 10^n + (a\times d + b\times c) \cdot 10^{n/2} + b \times d \\ &= m_2 \cdot 10^n + (m_1-m_2-m_3) \cdot 10^{n/2} + m_3
\end{align}
$$

$T(n) = 3T(n/2) + O(n) \implies T(n) = O(n^{log_23}).$

#### 3.3 The Closet-Pair Problem

O(log(n)) for 2-D space

![](image-20210914175237968.png)



## 4 Dynamic Programming

Review [[DP 281#L23 Dynamic Programming|Dynamic Programming]] from EECS 281.

**Optimality:** the optimal solution to a larger problem can be constructed from smaller subproblems.

**Overlap:** same subproblem appears many times.

**Solution:** record the result of subproblems.

#### 4.2 Longest Common Subsequence (LCS)

O(MN) time & space

![](image-20210914180942005.png)

![](image-20210914181208938.png)

#### 4.3 Longest Increasing Subsequence

$$L(i)  = \left\{
\begin{array} {lcl}
1	&	& ,{i = 1 \text{ or } S[j] \geq S[i]\text{ for all }0 < j < i}\\
1 + max\{L(j): 0 < j <i  \text{ and }S[j] <S[i]\} & & otherwise.
\end{array} \right.$$

O(n^2) time and O(n) space 

#### 4.4 All-Pairs Shortest Path

![](image-20210914185616773.png)

$O(|V|^3)$



## 5 Greedy

![[Algorithms 281#Greedy|Greedy Algorithms]]

```Algorithm
Algorithm 38 (Kruskal)
S:= empty set
For each edge sorted by weight:
	If S add e doesn't have any circle:
		add e to S
Return S
```
