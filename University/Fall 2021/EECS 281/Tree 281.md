## L17-18 Tree

Tree is an [[EECS 281#ADT|ADT]] represents a hierarchical tree structure with a set of connected nodes.

### Binary Tree

Ordered tree in which every node has at most two children.

```c++
//Height
height(node) = max(height(left_child), height(right_child) + 1)
//Size
size(node) = size(left_child) + size(right_child) + 1
// Depth
depth(node) = depth(parent) + 1
```

#### Tree Traversal

##### Preorder

Visit node ->visit left -> visit right, depth-first

##### In order

Visit left -> visit node -> visit right, depth-first

##### Post order

Visit left -> visit right -> visit node, depth-first

##### Level order

Use a queue, breath-first search

### Self-Balancing BST

Try to achieve $O(\log n)$ time complexity for search, insert, remove.

#### AVL Tree

For every node, the heights of the children differ by at most 1.

Use rotations (RR & RL) to correct imbalance.

Worst-case search/insert/remove is $O(\log n)$.
