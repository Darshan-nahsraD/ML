
```python
import numpy as np
```

## 1. Array Creation

### Creating arrays

```python
arr = np.array([1, 2, 3, 4])
print(arr)
```

**Output**
```text
[1 2 3 4]
```

### Creating arrays from lists

```python
a = [1, 2, 3.5]
print(np.array(a))
```

**Output**
```text
[1.  2.  3.5]
```

### Creating a 2D array

```python
arr = np.array([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
])
print(arr)
```

**Output**
```text
[[1 2 3]
 [4 5 6]
 [7 8 9]]
```

## 2. Array Generation Functions

### `arange()`

Generates values using a start, stop, and step. The stop value is excluded.

```python
print(np.arange(1, 11, 2))
```

**Output**
```text
[1 3 5 7 9]
```

### `zeros()`

Creates an array filled with zeros.

```python
print(np.zeros((2, 3), dtype=int))
```

**Output**
```text
[[0 0 0]
 [0 0 0]]
```

### `ones()`

Creates an array filled with ones.

```python
print(np.ones((2, 3), dtype=int))
```

**Output**
```text
[[1 1 1]
 [1 1 1]]
```

### `linspace()`

Generates evenly spaced values between two endpoints, including both endpoints by default.

```python
print(np.linspace(0, 1, 5))
```

**Output**
```text
[0.   0.25 0.5  0.75 1.  ]
```

### Random generation functions

```python
print(np.random.rand(3))
print(np.random.randn(3))
print(np.random.randint(10, 20, 5))
```

**Expected results**

- `rand(3)` — three random values between 0 and 1.
- `randn(3)` — three random values from a standard normal distribution.
- `randint(10, 20, 5)` — five random integers from 10 to 19.

The exact values change each time you run the code.

## 3. Array Attributes

```python
arr = np.array([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
])

print(arr.shape)
print(arr.size)
print(arr.dtype)
```

**Output**
```text
(3, 3)
9
int64
```

`dtype` can vary by platform or array construction, but here the array contains integers.

- `shape` — dimensions of the array.
- `size` — total number of elements.
- `dtype` — data type of the elements.

## 4. Array Methods

```python
arr = np.array([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
])

print(arr.min())
print(arr.max())
print(arr.sum())
print(arr.mean())
print(arr.std())
print(arr.argmax())
print(arr.argmin())
```

**Output**
```text
1
9
45
5.0
2.581988897471611
8
0
```

- `min()` — smallest value.
- `max()` — largest value.
- `sum()` — sum of all elements.
- `mean()` — average.
- `std()` — standard deviation (population by default).
- `argmax()` — flattened index of the maximum value.
- `argmin()` — flattened index of the minimum value.

### Using `axis`

```python
print(np.sum(arr, axis=0))
print(np.sum(arr, axis=1))
```

**Output**
```text
[12 15 18]
[ 6 15 24]
```

- `axis=0` — operates down the rows, producing column-wise results.
- `axis=1` — operates across columns, producing row-wise results.

## 5. Reshaping Arrays

### `reshape()`

Changes the shape without changing the number of elements.

```python
arr = np.arange(1, 13).reshape(3, 4)
print(arr)
```

**Output**
```text
[[ 1  2  3  4]
 [ 5  6  7  8]
 [ 9 10 11 12]]
```

The total number of elements must remain the same: \(3 \times 4 = 12\).

## 6. Array Indexing and Slicing

### 1D array

```python
arr = np.arange(11, 21)

print(arr[4])
print(arr[3::2])
print(arr[:4])
```

**Output**
```text
15
[14 16 18 20]
[11 12 13 14]
```

- `arr[4]` — element at index 4.
- `arr[3::2]` — starts at index 3 and takes every second element.
- `arr[:4]` — elements from index 0 up to, but excluding, index 4.

### 2D array indexing

```python
arr = np.arange(1, 31).reshape(6, 5)

print(arr[0, 0])
print(arr[3:, 3:])
print(arr[:, 2])
```

**Output**
```text
1

[[19 20]
 [24 25]
 [29 30]]

[ 3  8 13 18 23 28]
```

- `arr[row, column]` — selects an individual element.
- `arr[3:, 3:]` — selects rows and columns from index 3 onward.
- `arr[:, 2]` — selects every row in column index 2.

## 7. Boolean Indexing

Select elements using a condition.

```python
arr = np.arange(11, 21)

bool_index = arr % 2 == 0
print(bool_index)
print(arr[bool_index])
```

**Output**
```text
[False  True False  True False  True False  True False  True]

[12 14 16 18 20]
```

### Multiple conditions

```python
arr = np.arange(1, 11)

print(arr[(arr >= 3) & (arr <= 7)])
```

**Output**
```text
[3 4 5 6 7]
```

Use `&` for AND and `|` for OR. Put each condition inside parentheses.

## 8. Arithmetic Operations

```python
a1 = np.array([1, 2, 3, 4, 5])
a2 = np.array([6, 7, 8, 9, 10])

print(a1 + a2)
print(a1 - a2)
print(a1 * a2)
print(a1 / a2)
print(a1 // a2)
print(a1 ** 2)
```

**Output**
```text
[ 7  9 11 13 15]
[-5 -5 -5 -5 -5]
[ 6 14 24 36 50]
[0.16666667 0.28571429 0.375      0.44444444 0.5       ]
[0 0 0 0 0]
[ 1  4  9 16 25]
```

Operations are element-wise when the arrays have compatible shapes.

### Scalar multiplication

```python
print(a2 * 2)
```

**Output**
```text
[12 14 16 18 20]
```

## 9. Broadcasting

Broadcasting lets NumPy perform operations between compatible shapes without manually repeating values.

### Adding a scalar

```python
arr = np.array([10, 20, 30, 40])
print(arr + 10)
```

**Output**
```text
[20 30 40 50]
```

### Adding a scalar to a matrix

```python
arr = np.arange(1, 10).reshape(3, 3)
print(arr + 10)
```

**Output**
```text
[[11 12 13]
 [14 15 16]
 [17 18 19]]
```

The scalar `10` is applied to every element.

## 10. Shallow Views and Deep Copies

### Modifying a slice

Basic slicing usually creates a **view** of the original array.

```python
a = np.arange(1, 11)
view = a[:5]

view[0] = 99
print(a)
```

**Output**
```text
[99  2  3  4  5  6  7  8  9 10]
```

The original array changes because `view` shares its underlying data.

### Using `copy()`

```python
a = np.arange(1, 6)
b = a.copy()

b[0] = 99

print(a)
print(b)
```

**Output**
```text
[1 2 3 4 5]
[99  2  3  4  5]
```

`copy()` creates an independent array, so modifying `b` does not change `a`.

Note: In your notebook, `slice = slice * 10` creates a new result rather than modifying the original array in place. To demonstrate a view changing the original, modify an element directly or use an in-place operation such as `view *= 10`.

## 11. Matrix Operations

```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

print(np.dot(A, B))
print(A.T)
```

**Output**
```text
[[19 22]
 [43 50]]

[[1 3]
 [2 4]]
```

- `np.dot(A, B)` — matrix multiplication for these 2D arrays.
- `A.T` — transpose, which swaps rows and columns.

Remember: `A * B` performs element-wise multiplication, whereas `np.dot(A, B)` performs matrix multiplication.

## 12. Stacking Arrays

```python
a = np.array([1, 2, 3, 4])
b = np.array([5, 6, 7, 8])

print(np.vstack((a, b)))
print(np.hstack((a, b)))
print(np.column_stack((a, b)))
```

**Output**
```text
[[1 2 3 4]
 [5 6 7 8]]

[1 2 3 4 5 6 7 8]

[[1 5]
 [2 6]
 [3 7]
 [4 8]]
```

- `vstack()` — stacks vertically.
- `hstack()` — stacks horizontally.
- `column_stack()` — combines 1D arrays as columns.

## 13. Splitting Arrays

```python
c = np.arange(16).reshape(4, 4)
print(c)
```

**Output**
```text
[[ 0  1  2  3]
 [ 4  5  6  7]
 [ 8  9 10 11]
 [12 13 14 15]]
```

### `hsplit()`

```python
print(np.hsplit(c, 4))
```

**Output**
```text
[array([[ 0],
        [ 4],
        [ 8],
        [12]]),
 array([[ 1],
        [ 5],
        [ 9],
        [13]]),
 array([[ 2],
        [ 6],
        [10],
        [14]]),
 array([[ 3],
        [ 7],
        [11],
        [15]])]
```

Splits the matrix into four column-wise sections.

### `vsplit()`

```python
print(np.vsplit(c, 2))
```

**Output**
```text
[array([[0, 1, 2, 3],
        [4, 5, 6, 7]]),
 array([[ 8,  9, 10, 11],
        [12, 13, 14, 15]])]
```

Splits the matrix into two row-wise sections.

## 14. Practice Exercises

Use the following sample data from your exercises notebook.

```python
data = np.array([
    [18, 85, 78],
    [19, 92, 88],
    [17, 76, 95],
    [18, 65, 70],
    [20, 98, 85]
])
```

Columns: `[Age, Math Marks, Science Marks]`.

### Get the shape

```python
print(data.shape)
```

**Output**
```text
(5, 3)
```

### Find the average age

```python
print(np.mean(data[:, 0]))
```

**Output**
```text
18.4
```

### Extract Math marks

```python
print(data[:, 1])
```

**Output**
```text
[85 92 76 65 98]
```

### Find the highest Science mark

```python
print(data[:, 2].max())
```

**Output**
```text
95
```

### Get students scoring above 90 in Math

```python
print(data[data[:, 1] > 90])
```

**Output**
```text
[[19 92 88]
 [20 98 85]]
```

### Increase Math marks by 5

```python
print(data[:, 1] + 5)
```

**Output**
```text
[ 90  97  81  70 103]
```

This creates a result without modifying `data` itself.

### Count students younger than 19

```python
print(np.sum(data[:, 0] < 19))
```

**Output**
```text
3
```

### Average marks in each subject

```python
print(np.mean(data[:, 1:], axis=0))
```

**Output**
```text
[83.2 83.2]
```

### Students scoring at least 80 in both subjects

```python
print(data[(data[:, 1] >= 80) & (data[:, 2] >= 80)])
```

**Output**
```text
[[19 92 88]
 [20 98 85]]
```

### Replace Science marks below 75 with zero

```python
data[data[:, 2] < 75, 2] = 0
print(data)
```

**Output**
```text
[[18 85 78]
 [19 92 88]
 [17 76 95]
 [18 65  0]
 [20 98 85]]
```

This modifies `data` in place. Your notebook currently uses `> 75` in this exercise, but the stated requirement says `< 75`; the code above follows the requirement.

---

## Revision Checklist

- [ ] Array creation and generation functions
- [ ] Array attributes and methods
- [ ] Reshaping
- [ ] Indexing and slicing
- [ ] Boolean indexing
- [ ] Arithmetic operations
- [ ] Broadcasting
- [ ] Views and copies
- [ ] Matrix multiplication and transpose
- [ ] Stacking and splitting
- [ ] Practice exercises

**Revision tip:** Predict the output before running each example. Change the values, dimensions, and conditions to test your understanding.
