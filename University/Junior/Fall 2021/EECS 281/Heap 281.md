## L8 PQ, Heaps, and Heapsort

### Priority Queue

PQ is an [[EECS 281#ADT|ADT]] which is often implemented with heaps.

**Interface**

```c++
push(object);
pop();
object &top();
size();
empty;
decrease_priority();
```

**Implementation**

```c++
#include <queue>
std::priority_queue<>;
std::priority_queue<T, vector<T>, COMP> // comparitor
// uses std::less<> to determine relative priority of two elements
```

### Heap

| Operation | find-max    | delete-max       | insert      | increase-key     | meld        |
| --------- | ----------- | ---------------- | ----------- | ---------------- | ----------- |
| Binary    | $\Theta(1)$ | $\Theta(\log n)$ | $O(\log n)$ | $O(\log n)$      | $\Theta(n)$ |
| Binomial  | $\Theta(1)$ | $\Theta(\log n)$ | $\Theta(1)$ | $\Theta(\log n)$ | $O(\log n)$ |
| Fibonacci | $\Theta(1)$ | $O(\log n)$      | $\Theta(1)$ | $\Theta(1)$      | $\Theta(1)$ |

A specialized [[Tree 281|tree]]-based data structure which follows heap-order and is almost complete.

**Completeness:** every level of a tree is completely filled.

**Heap-ordered tree:** If each node's priority is not greater than the priority of the node's parent.

**Heapsort**

```c++
void heapsort(Item heap[], int n) {
	heapify(heap, n);
	for (int i = n; i >= 2; --i) {
		swap(heap[i], heap[1]);
		fixDown(heap, i - 1, 1);
	}
}
```

**`std::priority_queue`**

`template <class T, class Container = vector<T>, class Compare = less<typename Container::value_type>>`

#### Binary Heap

A simple type of heap.

Insert/Delete-min: $O(\log n)$

Find-min: $O(1)$

**Heapify**

Use `fixDown()` starting from the bottom

Complexity: O(n)

**Heapsort**

#### Functor

**Two ways of using PQ:**

1.  Implement functor that overloads `operator(const T &a, const T &b)`
2.  Implement `operator<(const T &that)` for the class.
