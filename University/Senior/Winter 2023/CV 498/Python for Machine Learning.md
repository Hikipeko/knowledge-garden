### Jupyter

##### Jupyter Notebook

A web-based interactive computational environment for creating notebook documents.

##### Google Colab

Based on Jupyter and runs computations in cloud.

##### Shortcuts

* `a` adds a cell before the current one
* `b` adds a cell after the current one
* `dd` deletes the current cell
* `shirt + enter` execute current cell

### Fancy Python

```python
# using set
print({int(sqrt(x)) for x in range(30)})
# {0, 1, 2, 3, 4, 5}
```

### Numpy

A library providing a high-performance multidimensional array object, and tools for working with these arrays.

#####  Create Arrays

```python
a = np.array([1, 2, 3])
a = np.zeros((2, 2))
a = np.ones((2, 2))
a = np.full((2, 2), 2) # constant array
a = np.eye(2) # identity matrix
a = np.random.random((2, 2))
```

##### Array Indexing

A slice of an array is a **view** into the same data, so modifying it will modify the original array.

```python
# slicing
a = np.array([[1,2,3,4], [5,6,7,8], [9,10,11,12]])
row_r1 = a[1, :]    # Rank 1 view of the second row of a  
row_r2 = a[1:2, :]  # Rank 2 view of the second row of a
row_r3 = a[[1], :]  # Rank 2 view of the second row of a
# row_r1 is [5 6 7 8]
# row_r2 and row_r3 are [[5 6 7 8]]

# integer indexing
a = np.array([[1,2], [3, 4], [5, 6]])
print(a[[0, 1, 2], [0, 1, 0]])
# [1 4 5]
# select one element form each row of a using the indices in b
a = np.array([[1,2,3], [4,5,6], [7,8,9], [10, 11, 12]])
b = np.array([0, 2, 0, 1])
print(a[np.arange(4), b])
# [1 6 7 11]
# mutate one element from each row of a using the indices in b
a[np.arange(4), b] += 10
# a becomes [[11 2 3] [ 4 5 16] [17 8 9] [10 21 12]]

# Boolean array indexing
a[a > 2] *= 2
```

##### Array Math

```python
# * is elementwise multiplication
# use dot function or @ to compute inner products of vectors
x.dot(y)
np.dot(x, y)
x @ y

# sum
x = np.array([[1,2],[3,4]])
print(np.sum(x))  # Compute sum of all elements; prints "10"
print(np.sum(x, axis=0))  # Compute sum of each column; prints "[4 6]"
print(np.sum(x, axis=1))  # Compute sum of each row; prints "[3 7]"

# transpose
x.T
```

##### Broadcasting

A powerful mechanism that allows numpy to work with arrays of different shapes when performing arithmetic operations. 

Say we want to add a vector to each row of a matrix x. Without broadcasting, we have to implement with a loop. However, we can do it with a single +. The rule is:

1. If the arrays do not have the same rank, prepend the shape of the lower rank array with 1s until both shapes have the same length.
2. The arrays can be broadcast together if they are compatible in all dimensions (either the same or one is 1).
3. Replicate the dimension where one array had size 1.

```python
x = np.array([[1,2,3], [4,5,6], [7,8,9], [10, 11, 12]])
v = np.array([1, 0, 1])
y = x + vv
# y = [[ 2 2 4] [ 5 5 7] [ 8 8 10] [11 11 13]]

v = np.array([1,2,3])  # v has shape (3,)
w = np.array([4,5])    # w has shape (2,)
print(np.reshape(v, (3, 1)) * w)
# [[ 4 5] [ 8 10] [12 15]]

# if we wanto to add a vector to each column, use transpose
x = np.array([[1,2,3], [4,5,6]])
print((x.T + w).T)
print(x + np.reshapre(w, (2, 1)))
# [[ 5 6 7] [ 9 10 11]]
```



### Matplotlib

```python
import matplotlib.pyplot as plt
%matplotlib inline

x = np.arange(0, 3 * np.pi, 0.1)
y = np.sin(x)
plt.plot(x, y)

# subplot
x = np.arange(0, 3 * np.pi, 0.1)
y_sin = np.sin(x)
y_cos = np.cos(x)
y_tan = np.tan(x)
# Set up a subplot grid that has height 2 and width 2,
plt.subplot(2, 2, 1)
plt.plot(x, y_sin)
plt.title('Sine')

# Set the second subplot as active, and make the second plot.
plt.subplot(2, 2, 2)
plt.plot(x, y_cos)
plt.title('Cosine')

plt.subplot(2, 2, 3)
plt.plot(x, y_tan)
plt.title('Tangent')

# Show the figure.
plt.show()
```
