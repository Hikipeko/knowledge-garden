## Disjoint-Set (Union-Find)

**`set_union()`** P25

**Find:** check if an element belongs to a particular set.

**Union:** merge two sets together into a single set.

**Interface**

```c++
union(x,y) 
find(x)
```

**Disjoint-set forest:** use a vector, representing the parent of an element to implement the ADT.

**Path Compression**

```c++
size_t find(size_t x) {
    size_t pathStart = x;
	// Pass 1 - find the ultimate rep
	while (reps[x] != x) {
		x = reps[x];
	}
	// x is now the ultimate rep
	// Pass 2 - path compression
	while (reps[pathStart] != x) {
		size_t tmp = reps[pathStart];
		reps[pathStart] = x; // Update path to ultimate rep
		pathStart = tmp;
	}
	return x;
}
```

**Complexity**

Search/Merge is $O(\alpha(n)) \to O(1)$, the inverse Ackermann function.
