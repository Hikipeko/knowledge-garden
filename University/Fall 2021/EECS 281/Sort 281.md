## L9 Ordered and Sorted Ranges

#### Binary Search

$O(\log n)$

```c++
int bs(int a[], int val, int left, int right) {
    // return the place of the first element that is no less than val
    while (left < right) {
        int mid = left + (right - left)/2;
        if (a[mid] < val)
            left = mid + 1;
        else
            right = mid;
    }
    return left;
}
```

#### STL

**`lower_bound()`** returns first item not less than target

**`upper_bound()`** returns first item greater than target

**`[lower_bound, upper_bound)`** is where element can be inserted

**`binary_search()`** returns a bool, not the location

## L10 Elementary Sorts

**Swap**

`#include <utility>` and use `std::swap;`

Avoid copying large elements (such as a vector of 10^7 elements)

**Stable**

If data with the same key remains in the same relative locations.

**Adaptive**

Performs different sequences of operations depending on outcomes of comparisons.

#### Bubble Sort

adaptive & stable

```c++
void bubble(Item a[], size_t left, size_t right) {
    for (size_t i = left; i < right; i++) {
        bool swapped = false;
        for (size_t j = right - 1; j > i; j--) {
            if (a[j] < a[j - 1]) {
                swap(a[j], a[j - 1]);
                swap = true;
            }
            if (!swapped) break;
        }
    }
}
```

#### Selection Sort

```c++
void selection(Item a[], size_t left, size_t right) {
    for (size_t i = left; i < right - 1; i++) {
        size_t min = i;
        for (size_t j = i + 1; j < right; j++) {
            if (a[j] < a[i]) i = j;
            if (i != min) swap(a[min], a[i]);
        }
    }
}
```

Good choice when swap is expensive

#### Insertion Sort

```c++
void insertion(Item a[], size_t left, size_t right) {
    // ensure the smallest element is in the beginning
    for (size_t i = right - 1; i > left; i--) {
        if (a[i - 1] > a[i])
            swap(a[i - 1], a[i]);
    }
    for (size_t i = left + 2; i < right; i++) {
        Item v = a[j]; size_t j = i;
        while (v < a[j-1]) {
            a[j] = a[j-1];
            j--;
        }
        a[j] = v;
    }
}
```

About $n^2/4$ compare and $n^2/4$ copies.

Stable, best sort for small values of $n$.

#### Counting Sort

Efficient when the number of different keys is a constant.

## L11 Quicksort

```c++
void quicksort(Item a[], size_t left, size_t right) {
	if (left + 1 >= right) return;
	size_t pivot = partition(a, left, right);
	quicksort(a, left, pivot);
	quicksort(a, pivot + 1, right);
}

// sort smaller region first
// prevent too much recursive calls
void quicksort(Item a[], size_t left, size_t right) {
	if (left + 1 >= right) return;
	size_t pivot = partition(a, left, right);
	if (pivot – left < right – pivot) {
		quicksort(a, left, pivot);
		quicksort(a, pivot + 1, right);
	} else {
		quicksort(a, pivot + 1, right);
		quicksort(a, left, pivot);
	}
}
```

Complexity: $O(n\log n)$

Memory use $O(log(n))$, not stable

## L12 Merge Sort

```c++
void merge(Item a[], int l, int mid, int r) {
    int n = r - l;
    vector<item> c(n);
    for (int i = l, j = mid, k = 0; k < n; k++) {
        if (i == mid) c[k] = a[j++];
        else if (j == right) c[k] = a[i++];
        else c[k] = (a[i] <= a[j]) ? a[i++] : a[j++]; 
    }
    copy(begin(c), end(c), &a[left]);
}
```

Complexity: $O(n\log n)$

$O(n)$ additional memory, stable
