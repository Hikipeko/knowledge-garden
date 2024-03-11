#### 7.1 Divisor Extraction Single Cube

How do we extract good divisors?

* For single-cube divisor: look for co-kernels
* For multiple-cube divisor: look for kernels

##### Cube-Literal Matrix

1. Assign a column for each literal
2. Assign a row for each unique product term
3. Put "1" in the matrix if the product term contain the literal
4. Look for prime rectangles: need not be contiguous rows

![[Pasted image 20230721115505.png|500]]

![[Pasted image 20230721120517.png]]

##### Bookkeeping for Literals Saved

* $C$ = \#columns in rectangle
* For each row $r$, let $w_r$ = \#times this product appears in network
* Compute $L = (C - 1) \times \sum_r w_r - C$

#### 7.2 Multiple Cube Case

1. Find co-kernels and kernels of each of the functions (recall [[6 Multilevel Logic 1#Brayton & McMullen Theorem|Brayton-McMullen]]).
2. For each row, take the co-kernel, for each column, take the unique cube among all kernels.
3. Put "1" in columns that are cubes in this kernel.
4. Look for prime rectangle

![[Pasted image 20230721122304.png]]

#### 7.3 Finding Prime Rectangles

##### Greedy Heuristics

* Rudell's Ping Pong heuristic
* <1% of optimal, while 10-100x faster than brute force

##### Reality

* Too expensive to compute all set of kernels
* Use heuristic to find a "quick" set of likely divisors