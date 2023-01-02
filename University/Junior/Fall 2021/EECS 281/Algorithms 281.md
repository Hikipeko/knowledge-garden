## L3-5 Recursion

**Tail recursion**

A function is tail recursive if there is no pending computation at each recursive step.

`return tail_recursion_function(arguments);`

O(1) space

![[image-20210916001505810.png|600]]

```c++
int power(int x, uint32_t n, int result = 1) {
    if (n == 0)
        return result;
    else if (n % 2 == 0) // even
        return power(x * x, n / 2, result);
 	else // odd
 		return power(x * x, n / 2, result * x);
 } // power()
```



## L21 Algorithm Family

#### Brute-Force

#### Greedy

A greedy algorithm computes a solution to a problem by making a locally optimal choice at each step of the algorithm.

E.g. [[University/Junior/Fall 2021/EECS 281/Graphs 281#Prim's Algorithm|Prime's Algorithm]], [[University/Junior/Fall 2021/EECS 281/Graphs 281#Kruskal's Algorithm|Kruskal's Algorithm]], [[Graphs 281#Dijkstra's Algorithm|Dijkstra's Algorithm]]

#### Divide and Conquer

Breaks down a problem into two or more sub-problems of the same or related type, until these become simple enough to be solved directly. The solutions to the sub-problems are then combined to give a solution to the original problem.

E.g. recursion, [[Sort 281#Binary Search|binary search]], [[Sort 281#L11 Quicksort|quicksort]]

#### Combine and Conquer

[[Sort 281#L12 Merge Sort|merge sort]]

#### [[DP 281|Dynamic Programming]]

#### [[Backtracking 281#Backtracking|Backtracking]]

#### [[Backtracking 281#Branch and Bound Algorithms|Branch and Bound]]
