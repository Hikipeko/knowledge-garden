#### 5.1 Basics

![[Pasted image 20230718111123.png|450]]

* Goal: try to find minimal SOP logic with fewest input wires (aka literals).
* **Prime implicants** are cubes that cannot be larger.
* Best solution is composed of cover of primes.
* **Irredundant** means no prime can be removed.

#### 5.2 Reduce-Expand-Irredundant Optimization

* We start with truth table (TT), each row defining a product (cube).
* Do these there steps iteratively, and we get a ==prime, minimal, irredundant== solution.
* EXPRESSO: Reduce with clever cubes order, Expand as covering problem, Irredundant with clever URP algorithm.

##### Irredundant

![[Pasted image 20230718111605.png|250]]

* Remove cube such that we still cover all the 1's.

##### Reduce

![[Pasted image 20230718111642.png|250]]

* Reduce is a heuristic.
* Take each cube, "shrink it" as much as possible.
* When we expand it again, maybe we get a new better solution.

#### 5.3 Expand

![[Pasted image 20230718111615.png|250]]

* Expand is a heuristic, done one cube at a time.
* Make each cube as big as possible (become a prime).
* Represented as [[2 Computational Boolean Algebra#Positional Cube Notation|PCN]] cube lists
* Many of the rest operations are all done as [[2 Computational Boolean Algebra#2.6 Unate Recursive Complement(URP) Implementation|URP]].
* Expand a cube means remove variables form cube.
* Covering problem: cover all the whole set with small number of subsets.
* Algorithm
	1. Given function $F$, build a cube cover of the 0s in $F$ (OFF set), using URP Complement. These are the areas that we cannot touch.
	2. Build the **blocking matrix**, one row for each variable in the cube trying to expand. Put "1" for different polarity.
	3. Find smallest set of rows that covers each column. Product of these row variables is a **legal cube expansion**. By choosing a row, you avoid overlapping with corresponding OFF cubes with "1". 

![[Pasted image 20230718113201.png|600]]
