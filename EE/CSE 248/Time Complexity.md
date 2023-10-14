https://en.wikipedia.org/wiki/Time_complexity#Strongly_and_weakly_polynomial_time

#### Common Time Complexities

* $O(1)$
* $O(\log \log n)$
* $O(\log n)$
* $\text{poly}(\log n)$
* $O(n^c), \, 0 < c < 1$
* $O(n)$
* $O(n \log n)$
* $n \cdot \text{poly}(\log n)$
* $O(n^2)$
* P: $2^{O(\log n)}$
* QP: $2^{\text{poly} (\log n)}$

#### Polynomial Time

##### Strong Polynomial Time

1. The number of operations is bounded by a polynomial in the number of integers in the input instance.
2. The space used by the algorithm is bounded by a polynomial in the size of the input.

E.g. Dinic's algorithm

An algorithm that runs in polynomial time but that is not strongly polynomial run in **weakly polynomial time**. E.g. Euclidean algorithm for GCD, linear programming.