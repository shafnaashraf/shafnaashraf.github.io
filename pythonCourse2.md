# NumPy for Data Science - Complete Tutorial

## Table of Contents
1. [Introduction to NumPy](#introduction-to-numpy)
2. [Installation](#installation)
3. [Creating NumPy Arrays](#creating-numpy-arrays)
4. [Array Indexing and Selection](#array-indexing-and-selection)
5. [Array Operations](#array-operations)
6. [Array Attributes and Methods](#array-attributes-and-methods)
7. [Practical Examples](#practical-examples)
8. [Key Takeaways](#key-takeaways)

## Introduction to NumPy

**NumPy** (Numerical Python) is a powerful linear algebra library for Python that serves as the foundation for data science in Python. Here's why NumPy is essential:

- **Speed**: NumPy is incredibly fast due to its C bindings
- **Foundation**: Almost all other Python data science libraries rely on NumPy
- **Efficiency**: Provides efficient storage and operations for large arrays
- **Vectorization**: Enables element-wise operations without explicit loops

### What are NumPy Arrays?

NumPy arrays come in two main flavors:
- **Vectors**: Strictly 1-dimensional arrays
- **Matrices**: 2-dimensional arrays (can have one row or one column)

## Installation

### Recommended Method (Anaconda Distribution)

If you have Anaconda installed (highly recommended for data science):

```bash
conda install numpy
```

### Alternative Method (pip)

If you don't have Anaconda:

```bash
pip install numpy
```

**Note**: Anaconda is recommended because it ensures all underlying dependencies (like linear algebra libraries) are properly synchronized.

## Creating NumPy Arrays

First, let's import NumPy:

```python
import numpy as np
```

### 1. Creating Arrays from Python Lists

#### One-dimensional Arrays (Vectors)

```python
# Create a simple list
my_list = [1, 2, 3]
print(my_list)  # Output: [1, 2, 3]

# Convert list to NumPy array
arr = np.array(my_list)
print(arr)  # Output: array([1, 2, 3])
```

#### Two-dimensional Arrays (Matrices)

```python
# Create a matrix from list of lists
my_matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
matrix = np.array(my_matrix)
print(matrix)
# Output:
# array([[1, 2, 3],
#        [4, 5, 6],
#        [7, 8, 9]])
```

### 2. Built-in Array Generation Methods

#### Using `arange()` - Similar to Python's `range()`

```python
# Basic usage: start at 0, stop before 10
arr = np.arange(10)
print(arr)  # Output: array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

# With start and stop
arr = np.arange(0, 11)
print(arr)  # Output: array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10])

# With step size
arr = np.arange(0, 11, 2)
print(arr)  # Output: array([0, 2, 4, 6, 8, 10])
```

#### Creating Arrays of Zeros and Ones

```python
# 1D array of zeros
zeros_1d = np.zeros(5)
print(zeros_1d)  # Output: array([0., 0., 0., 0., 0.])

# 2D array of zeros (rows, columns)
zeros_2d = np.zeros((2, 3))
print(zeros_2d)
# Output:
# array([[0., 0., 0.],
#        [0., 0., 0.]])

# 2D array of ones
ones_2d = np.ones((3, 4))
print(ones_2d)
# Output:
# array([[1., 1., 1., 1.],
#        [1., 1., 1., 1.],
#        [1., 1., 1., 1.]])
```

#### Using `linspace()` - Evenly Spaced Points

```python
# 10 evenly spaced points between 0 and 5
lin_arr = np.linspace(0, 5, 10)
print(lin_arr)
# Output: array([0.        , 0.55555556, 1.11111111, 1.66666667, 2.22222222,
#                2.77777778, 3.33333333, 3.88888889, 4.44444444, 5.        ])

# 100 evenly spaced points between 0 and 5
lin_arr_100 = np.linspace(0, 5, 100)
print(len(lin_arr_100))  # Output: 100
```

**Key Difference**: 
- `arange()`: Uses step size as the third argument
- `linspace()`: Uses number of points as the third argument

#### Creating Identity Matrices

```python
# 4x4 Identity matrix
identity = np.eye(4)
print(identity)
# Output:
# array([[1., 0., 0., 0.],
#        [0., 1., 0., 0.],
#        [0., 0., 1., 0.],
#        [0., 0., 0., 1.]])
```

### 3. Random Number Generation

#### Random Numbers from Uniform Distribution (0 to 1)

```python
# 1D array of 5 random numbers
rand_arr = np.random.rand(5)
print(rand_arr)  # Output: array([0.37454012, 0.95071431, 0.73199394, 0.59865848, 0.15601864])

# 2D array of random numbers
rand_2d = np.random.rand(5, 5)
print(rand_2d.shape)  # Output: (5, 5)
```

#### Random Numbers from Normal Distribution

```python
# Standard normal distribution (mean=0, std=1)
normal_arr = np.random.randn(5)
print(normal_arr)  # Output: array([ 1.76405235,  0.40015721,  0.97873798,  2.2408932 ,  1.86755799])

# 2D normal distribution
normal_2d = np.random.randn(4, 4)
print(normal_2d.shape)  # Output: (4, 4)
```

#### Random Integers

```python
# Random integers from 1 to 100 (exclusive)
rand_int = np.random.randint(1, 100)
print(rand_int)  # Output: 37 (example)

# Multiple random integers
rand_ints = np.random.randint(1, 100, 10)
print(rand_ints)  # Output: array([37, 12, 72,  9, 75,  5, 79, 64, 16,  1])
```

## Array Indexing and Selection

### 1. Basic Indexing (1D Arrays)

```python
arr = np.arange(0, 11)
print(arr)  # Output: array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10])

# Get single element
print(arr[8])  # Output: 8

# Get range of elements (slice notation)
print(arr[1:5])  # Output: array([1, 2, 3, 4])

# From beginning to index 6
print(arr[:6])  # Output: array([0, 1, 2, 3, 4, 5])

# From index 5 to end
print(arr[5:])  # Output: array([5, 6, 7, 8, 9, 10])
```

### 2. Broadcasting (Modifying Multiple Elements)

```python
arr = np.arange(0, 11)
print("Original:", arr)

# Set first 5 elements to 100
arr[0:5] = 100
print("Modified:", arr)  # Output: array([100, 100, 100, 100, 100, 5, 6, 7, 8, 9, 10])
```

### 3. Array Copies vs Views

**Important**: Slices of arrays are views, not copies!

```python
arr = np.arange(0, 11)
slice_of_arr = arr[0:6]

# Modifying the slice affects the original array
slice_of_arr[:] = 99
print("Original array:", arr)  # Output: array([99, 99, 99, 99, 99, 99, 6, 7, 8, 9, 10])

# To get a copy instead of a view
arr = np.arange(0, 11)
arr_copy = arr.copy()
arr_copy[:] = 100
print("Original array:", arr)  # Output: array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]) - unchanged!
```

### 4. 2D Array Indexing

```python
# Create 2D array
arr_2d = np.array([[5, 10, 15], [20, 25, 30], [35, 40, 45]])
print(arr_2d)
# Output:
# array([[ 5, 10, 15],
#        [20, 25, 30],
#        [35, 40, 45]])
```

#### Double Bracket Format

```python
# Get element at row 0, column 0
print(arr_2d[0][0])  # Output: 5

# Get element at row 1, column 1
print(arr_2d[1][1])  # Output: 25
```

#### Single Bracket with Comma (Recommended)

```python
# Get element at row 0, column 0
print(arr_2d[0, 0])  # Output: 5

# Get element at row 1, column 2
print(arr_2d[1, 2])  # Output: 30

# Get entire row
print(arr_2d[1])  # Output: array([20, 25, 30])
```

#### 2D Array Slicing

```python
# Get top right corner (first 2 rows, columns 1 onwards)
print(arr_2d[:2, 1:])
# Output:
# array([[10, 15],
#        [25, 30]])

# Get bottom rows (rows 1-2, all columns)
print(arr_2d[1:, :])
# Output:
# array([[20, 25, 30],
#        [35, 40, 45]])
```

### 5. Conditional Selection

This is one of the most powerful features of NumPy!

```python
arr = np.arange(1, 11)
print("Original array:", arr)  # Output: array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])

# Create boolean array
bool_arr = arr > 5
print("Boolean array:", bool_arr)  
# Output: array([False, False, False, False, False, True, True, True, True, True])

# Use boolean array to select elements
print("Elements > 5:", arr[bool_arr])  # Output: array([6, 7, 8, 9, 10])

# More commonly, do it in one step
print("Elements > 5:", arr[arr > 5])  # Output: array([6, 7, 8, 9, 10])
print("Elements < 3:", arr[arr < 3])  # Output: array([1, 2])
```

## Array Operations

### 1. Array with Array Operations

```python
arr = np.arange(0, 11)
print("Original array:", arr)

# Addition
print("arr + arr:", arr + arr)  # Each element doubled

# Subtraction
print("arr - arr:", arr - arr)  # All zeros

# Multiplication
print("arr * arr:", arr * arr)  # Element-wise multiplication
```

### 2. Array with Scalar Operations

```python
arr = np.arange(0, 11)

# Add scalar to all elements
print("arr + 100:", arr + 100)

# Multiply all elements by scalar
print("arr * 2:", arr * 2)

# Subtract scalar from all elements
print("arr - 100:", arr - 100)

# Exponentiation
print("arr ** 2:", arr ** 2)  # Square all elements
```

### 3. Warning: Division by Zero

NumPy handles division by zero differently than regular Python:

```python
# Regular Python would raise an error
# print(1/0)  # ZeroDivisionError

# NumPy gives warnings but continues execution
arr = np.arange(0, 3)
print("arr:", arr)  # Output: array([0, 1, 2])
print("arr / arr:", arr / arr)  # Output: [nan  1.  1.] with warning

# Division by zero gives infinity
print("1 / arr:", 1 / arr)  # Output: [inf  1.  0.5] with warning
```

### 4. Universal Array Functions

NumPy provides many mathematical functions that work element-wise:

```python
arr = np.arange(0, 11)

# Square root
print("Square root:", np.sqrt(arr))

# Exponential
print("Exponential:", np.exp(arr))

# Maximum (same as arr.max())
print("Maximum:", np.max(arr))

# Trigonometric functions
print("Sine:", np.sin(arr))
print("Cosine:", np.cos(arr))

# Logarithm
print("Natural log:", np.log(arr))  # Note: log(0) gives -inf with warning
```

## Array Attributes and Methods

### Useful Array Methods

```python
arr = np.arange(25)
print("1D array:", arr)

# Reshape array
reshaped = arr.reshape(5, 5)
print("Reshaped to 5x5:")
print(reshaped)

# Find max and min values
rand_arr = np.random.randint(0, 50, 10)
print("Random array:", rand_arr)
print("Max value:", rand_arr.max())
print("Min value:", rand_arr.min())

# Find index of max and min values
print("Index of max:", rand_arr.argmax())
print("Index of min:", rand_arr.argmin())
```

### Array Attributes

```python
arr = np.arange(25)
print("1D array shape:", arr.shape)  # Output: (25,)

# Reshape to 2D
arr_2d = arr.reshape(5, 5)
print("2D array shape:", arr_2d.shape)  # Output: (5, 5)

# Data type
print("Data type:", arr_2d.dtype)  # Output: int64 (or int32 depending on system)
```

## Practical Examples

### Example 1: Creating a Sample Dataset

```python
# Create a 5x10 array for practice
practice_arr = np.arange(50).reshape(5, 10)
print("Practice array:")
print(practice_arr)

# Select specific elements (e.g., elements 13, 14, 23, 24)
# These are at rows 1-2, columns 3-4
subset = practice_arr[1:3, 3:5]
print("Subset:")
print(subset)
```

### Example 2: Data Analysis Simulation

```python
# Simulate daily temperatures for a month
daily_temps = np.random.normal(25, 5, 30)  # mean=25°C, std=5°C
print("Daily temperatures:", daily_temps)

# Find hot days (> 30°C)
hot_days = daily_temps[daily_temps > 30]
print("Hot days:", hot_days)
print("Number of hot days:", len(hot_days))

# Calculate statistics
print("Average temperature:", daily_temps.mean())
print("Max temperature:", daily_temps.max())
print("Min temperature:", daily_temps.min())
```

### Example 3: Matrix Operations

```python
# Create two matrices
matrix_a = np.random.randint(1, 10, (3, 3))
matrix_b = np.random.randint(1, 10, (3, 3))

print("Matrix A:")
print(matrix_a)
print("Matrix B:")
print(matrix_b)

# Element-wise operations
print("A + B:")
print(matrix_a + matrix_b)
print("A * B (element-wise):")
print(matrix_a * matrix_b)

# Matrix multiplication (dot product)
print("A @ B (matrix multiplication):")
print(matrix_a @ matrix_b)
```

## Key Takeaways

1. **Installation**: Use Anaconda distribution and `conda install numpy` for best results

2. **Array Creation**:
   - From lists: `np.array([1, 2, 3])`
   - Range: `np.arange(start, stop, step)`
   - Evenly spaced: `np.linspace(start, stop, num_points)`
   - Special arrays: `np.zeros()`, `np.ones()`, `np.eye()`
   - Random: `np.random.rand()`, `np.random.randn()`, `np.random.randint()`

3. **Indexing**:
   - 1D: `arr[index]` or `arr[start:stop]`
   - 2D: `arr[row, col]` (recommended) or `arr[row][col]`
   - Conditional: `arr[arr > value]`

4. **Important**: Array slices are views, not copies. Use `.copy()` when needed.

5. **Operations**:
   - Element-wise: `+`, `-`, `*`, `/`, `**`
   - Universal functions: `np.sqrt()`, `np.exp()`, `np.sin()`, etc.
   - Broadcasting works with scalars

6. **Attributes**: `.shape`, `.dtype`, `.max()`, `.min()`, `.argmax()`, `.argmin()`

7. **NumPy handles errors gracefully**: Division by zero gives warnings, not errors

This foundation in NumPy will serve you well as you progress to pandas, matplotlib, and other data science libraries that build upon NumPy's array structure.

## Further Learning

- Explore the [NumPy documentation](https://numpy.org/doc/) for more advanced features
- Practice with real datasets to solidify your understanding
- Learn about advanced indexing, broadcasting rules, and linear algebra operations
- The next tutorial will cover Pandas, which builds heavily on NumPy concepts
