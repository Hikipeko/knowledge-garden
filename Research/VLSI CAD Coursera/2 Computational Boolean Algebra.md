#### 2.1 Basics

Motivation: we need algorithmic, computational strategies for Boolean stuff.

##### Shannon Expansion

![[Pasted image 20230714094453.png|300]]

* Shannon Expansion of $F$ w.r.t. $x_i$
* $F(x_1, x_2, ..., x_i, ..., x_n) = x_i \cdot F(x_i=1) + x_i' \cdot F(x_i=0)$
* Can be applied to multiple variables

##### Shannon Cofactors

* Shannon Cofactor w.r.t. $x_i$ and $x_j$
* $F_{x_i x_j'} := F(x_1, x_2, ..., x_i=1, ..., x_j=0, ..., x_n)$
* Each of the cofactors is a function, not a number

##### Properties of Cofactors

$$
\begin{align}
(F')_x &= (F_x)'\\
(F\cdot G)_x &= F_x \cdot G_x\\
(F+ G)_x &= F_x + G_x\\
(F\oplus G)_x &= F_x \oplus G_x
\end{align}
$$

#### 2.2 Boolean Difference

* $\partial f / \partial x = f_x \oplus f_{\bar x}$
* xor of the Shannon cofactors w.r.t. $x$
* A boolean ==function not== depend on $x$
* Tell how $f$ changes when $x$ changes
* $\partial(f \oplus g) = \partial f / \partial x \oplus \partial g / \partial x$
* When Boolean Difference is 1, then $f$ changes if $x$ changes

#### 2.3 Quantification Operators

* **Universal Quantification** w.r.t. $x_i$
* $(\forall x_i, F) = F_{x_i} \cdot F_{x_i'}$
* And cofactors
* For some input pattern, $F = 1$ for all $x_i$
* **Existential Quantification** w.r.t. $x_i$
* $(\exists x_i, F) = F_{x_i} + F_{x_i'}$
* Or cofactors
* For some input pattern, exist $x_i$ s.t. $F = 1$
* $(\forall xy, F) = F_{xy} \cdot F_{x'y} \cdot F_{xy'} \cdot F_{x'y'}$

#### 2.4 Application to Logic Network Repair

![[Pasted image 20230714110156.png|500]]

* Suppose you got ONE gate wrong
* $Z = G \otimes Z = 1$ always true, solve $(\forall ab, Z) \equiv 1$.

#### 2.5 Recursive Tautology

##### Positional Cube Notation

* Tautology is a ==data structure== for a Boolean function
* Problem: whether $F$ is a tautology, i.e. $F \equiv 1$ for every input
* Any Boolean function can be represented in sum of products expression, list of list.
* Represent each product term as a cube (vector) $v$
* $v[i] =$ 01 if contains $x_i$, 10 if contains $x_i'$, 11 if has no $x_i$ term.
* $f$ is a tautology iff $f_x$ and $f_x'$ are both tautologies.

#### 2.6 Unate Recursive Complement Implementation

* Is a given Boolean function a tautology?
* We solve this problem by recursively split on some $x_i$.
* A kind of URP algorithm.

##### Recursive Rule

* If want cofactor w.r.t. $x=1$, remove 10, change 01 to 11, leave alone 11
* If want cofactor w.r.t. $x=0$, remove 01, change 10 to 11, leave alone 11

##### Termination Rule

* $f$ is **unate** if a SOP representation only has each literal in exactly one polarity. $f$ is otherwise **binate**.
* $f$ is positive unate in var $x$ if changing $x:\;0 \to 1$ keeps $f$ constant or makes $f:\; 0 \to 1$.
* $f$ is negative unate otherwise.
* Unate cube list for $f$ is tautology iff it contains all don't care cube = \[11 11 ... 11\]
* Otherwise the unate cube list is not tautology

##### Split Rule

* Pick binate var with most product terms dependent on it, trying to kill as many cubes as possible.
* If a tie, pick one with min | true var - complement var |, hoping to balance the recursion tree.

