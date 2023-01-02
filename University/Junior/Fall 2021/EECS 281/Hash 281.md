## L15 Dictionary and Hash Tables

#### Dictionary

An [[EECS 281#ADT|ADT]], aka associative array, map.

1. insert (key, value) pair
2. search by key
3. remove by key
4. (sort, select kth largest, join two dictionaries)

#### Hashing

vector size M

$h(x) = a \cdot x + b \mod M$, a and b are primes.

## L16 Hash Collision Resolution

### Hash Table

A [[EECS 281#Data Structure|data structure]] that implements a dictionary.

#### Separate Chaining

Maintain M linked lists

**Load Factor**

$\alpha = N / M$

### Open Addressing

#### Linear Probing

Hits: $1/2(1 + 1/(1 - \alpha))$

Misses: $1/2(1 + 1/(1 - \alpha)^2)$

#### Quadratic Probing

Hits: 
$$1 - \frac{\alpha}{2} + \ln(\frac{1}{1 - \alpha})$$

Misses: $$\frac{1}{1-\alpha} - \alpha + \ln(\frac{1}{1 - \alpha})$$
#### Double Hashing

Probe $(h(key) + j * h'(key)) \mod M$

#### Amortized Analysis

Search/Insert/Delete: $O(1)$
