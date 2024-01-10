Modern cryptosystems are invariably based on the assumption that some problem is hard.

![[Pasted image 20230402170410.png|500]]

### 9.1 Preliminaries and Basic Group Theory

#### 9.1.1 Primes and Divisibility

**Proposition 9.1** Let $a$ be an integer and let $b$ be a positive integer, then there exist unique $q, r$ for which $a=q\cdot b+r$ and $0\leq r < b$.

**Proposition 9.2** Let $a,b$ be positive integers. Then there exist integer $X,Y$ such that $X\cdot a + Y \cdot b = \gcd(a,b)$. Further, $\gcd(a,b)$ is the smallest integer that can be expressed in this way.

**Proposition 9.3** If $c \vert ab$ and $\gcd(a,c) =1$, then $c\vert b$.

**Proposition 9.3** If $a \vert N, b \vert N$ and $\gcd(a,b) =1$, then $ab\vert N$.

#### 9.1.2 Modular Arithmetic

**Proposition 9.7** Let $b, N$ be integers, with $b \geq 1$ and $N > 1$. Then $b$ is invertible modulo $N$ if and  only if $\gcd(b, N) = 1$.

#### 9.1.3 Groups

**Definition 9.9** A group is a set $\mathbb G$ along with a binary operation $\circ(\cdot, \cdot)$ for which the following conditions hold:

* **Closure**: For all $g, h \in {\mathbb G}, g \circ h \in {\mathbb G}$.
* **Identity**: $\exists e \in {\mathbb G}$ such that for all $g \in {\mathbb G}, g \circ e = g = e \circ g$.
* **Inverse**: For all $g \in {\mathbb G}, \exists h \in {\mathbb G}$ such that $g \circ h = e = h \circ g$.
* **Associativity**: For all $g_1, g_2, g_3 \in {\mathbb G}, (g_1 \circ g_2) \circ g_3 = g_1 \circ (g_2 \circ g_3)$.
* **Commutativity**: (not necessary, Abelian group)  For all $g, h \in {\mathbb G}$, $g \circ h =h \circ g$.
* $|\mathbb G|$ is the order (number of elements) in $\mathbb G$.

**Theorem 9.14** Let $\mathbb G$ be a finite group with $m = |{\mathbb G}|$. Then for any element $g \in {\mathbb G}$, it holds that $g^m = 1$. Additionally, we have $g^x = g^{[x \mod m]}$.

**Corollary 9.17** Let $\mathbb G$ be a finite group with $m = |{\mathbb G}| > 1$. Let $e > 0$ be an integer, and define the function $f_e : {\mathbb G} \to {\mathbb G}$ by $f_e(g) = g^e$. If $\gcd(e,m) = 1$, then $f_e$ is a permutation. Moreover, if $d = e^{-1} \mod m$ then $f_d$ is the inverse of $f_e$.

#### The Group ${\mathbb Z}^*_N$

${\mathbb Z}_N = \{0, \dots ,N-1\}$. We want to define a group for multiplication.

**Proposition 9.18** Let $N > 1$ be an integer. Then ${\mathbb Z}^*_N := \{b \in \{1, \dots, N-1\} | \gcd(b,N) = 1\}$ is an abelian group under multiplication modulo $n$.

**Euler phi function**
$$
\phi(N) := |{\mathbb Z}^*_N| = \Pi_i p_i^{e_i -1}(p_i-
1)
$$
**Corollary 9.21**
$$
a^{\phi(N)} = 1 \mod N.
$$
**Corollary 9.22** Fix $N > 1$. $f_e(x) = [x^e \mod N]$ is permutation if $\gcd(e, \phi(N)) = 1$. Also we can find the inverse $f_d$ by finding $d = e^{-1} \mod \phi(N)$.



### 9.3 Cryptographic Assumptions in Cyclic Groups

#### 9.3.1 Cyclic Groups and Generators

Define $\langle g \rangle :\ \{g^0, g^1, \dots \}$ for $g \in {\mathbb G}$.

**Definition 9.52** Let ${\mathbb G}$ be a finite group and $g \in {\mathbb G}$. The order of $g$ is the smallest positive integer $i$ with $g^i = 1$.

**Proposition 9.54** Let ${\mathbb G}$ be a finite group and $g \in {\mathbb G}$ an element of order $i$. Then $g^x = g^y$ is and only if $x = y \mod i$.

**Generator** If an element $g \in {\mathbb G}$ has order $m$, then $\langle g \rangle = {\mathbb G}$, and we say $\mathbb G$ is a cyclic group and $g$ is a generator of $\mathbb G$.

**Proposition 9.55** Let ${\mathbb G}$ be a finite group of order $m$, and say $g \in {\mathbb G}$ has order $i$. Then $i \vert m$.

**Corollary 9.56** If $\mathbb G$ is a group of prime order $p$, then $\mathbb G$ is cyclic. Furthermore, all elements except the identity are generators of $\mathbb G$.

**Theorem 9.57** If $p$ is prime then ${\mathbb Z}^*_p$ is a cyclic group of order $p - 1$.

#### 9.3.2 The Diffie-Hellman Assumptions






* 12.1
* 12.1 9.1
* 9.3.1 9.3.2
* 9.3.2 12.2
* 12.4.1
* 9.2.3 9.2.4 12.5.1