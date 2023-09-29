#### 8.1-8.2 Implicit Don't Cares

* We lose some optimality when using the algebraic model.
* How do we put it back? **Don't care**, an input pattern that can never happen.
* Don't cares happen as a natural byproduct of Boolean network called Implicit.
* Exploit DCs to simplify inside nodes. ==BUT HOW?==

![[Pasted image 20230721123120.png|450]]

![[Pasted image 20230721123624.png|400]]

* When does the value of the output of node $f$ affect $z$? X = 0
* Can we use this information to simplify $f$? Yes, we don't care X for $f$.

![[Pasted image 20230721123908.png|450]]

There are 3 types of implicit DC, **Satisfiability**, **Controllability**, **Observability**.

#### 8.3 Satisfiability Don't Cares

* Belong to the wires inside the Boolean logic network.
* How to represent DC pattern at a node?
* As a Boolean function that makes a 1 when the pattern is impossible, called a *Don't Care Cover*.
* One SDC for every internal wire in Boolean logic network.
* $\text{SDC}_X = X \oplus (\text{expression for } X)$ .

![[Pasted image 20230724173344.png|500]]

#### 8.4 Controllability Don't Cares

* Patterns that cannot happen at inputs to a network node.
* We can ignore SDC on **primary inputs** (input with not network node for them).

$$
\text{CDC}_F() = (\forall \text{ variables not used in }F)[\sum_{\text{input wires }X_i\text{ to}F} \text{SDC}_{X_i}]
$$

![[Pasted image 20230724175300.png|500]]

#### 8.5 Observability Don't Cares

Patterns input to node that make network outputs insensitive to output of the node.

