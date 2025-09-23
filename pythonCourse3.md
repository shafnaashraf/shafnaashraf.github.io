# Python for Data Science: Pandas Tutorial Part 1 

## Table of Contents

  * [What is Pandas?](https://www.google.com/search?q=%23what-is-pandas)
  * [Installation](https://www.google.com/search?q=%23installation)
  * [Understanding Pandas Series](https://www.google.com/search?q=%23understanding-pandas-series)
  * [Working with DataFrames](https://www.google.com/search?q=%23working-with-dataframes)
  * [Data Selection and Indexing](https://www.google.com/search?q=%23data-selection-and-indexing)
  * [Conditional Selection](https://www.google.com/search?q=%23conditional-selection)
  * [Index Management](https://www.google.com/search?q=%23index-management)
  * [Multi-level Indexing](https://www.google.com/search?q=%23multi-level-indexing)
  * [Summary](https://www.google.com/search?q=%23summary)

-----

## What is Pandas?

Pandas is an open-source data analysis and manipulation library built on top of NumPy. Think of it as Python's answer to Excel or R's data frames, but much more powerful and flexible.

**Key Features of Pandas:**

  * Fast analysis and data cleaning: Handle large datasets efficiently
  * Data preparation: Clean, transform, and reshape your data
  * High performance: Built on NumPy for speed
  * User-friendly: Intuitive syntax and methods
  * Built-in visualization: Basic plotting capabilities
  * Versatile data sources: Read from CSV, Excel, SQL, JSON, and more

**Why Learn Pandas?**

  * Essential for data science and analysis
  * Industry standard for data manipulation in Python
  * Excellent performance and productivity
  * Integrates seamlessly with other Python libraries

-----

## Installation

To install Pandas, you have two main options:

  * **Option 1: Using Conda** (Recommended if you have Anaconda)

    ```bash
    conda install pandas
    ```

  * **Option 2: Using pip**

    ```bash
    pip install pandas
    ```

**Verify Installation**

```python
import pandas as pd
import numpy as np
print(pd.__version__)
```

-----

## Understanding Pandas Series

A Series is a one-dimensional labeled array that can hold any data type. It's like a column in a spreadsheet or a single column from a database table.

**Key Characteristics:**

  * Built on top of NumPy arrays
  * Has labeled indices (unlike NumPy arrays)
  * Can hold various data types
  * Supports fast lookups like a hash table

### Creating Series

```python
import pandas as pd
import numpy as np

# Creating Series from different data types
labels = ['A', 'B', 'C']
my_data = [10, 20, 30]
arr = np.array([10, 20, 30])
my_dict = {'A': 10, 'B': 20, 'C': 30}

# Method 1: From a Python list
series1 = pd.Series(data=my_data)
print("Series from list:")
print(series1)
print()
```

Output:

```
Series from list:
0    10
1    20
2    30
dtype: int64
```

```python
# Method 2: From list with custom index
series2 = pd.Series(data=my_data, index=labels)
print("Series with custom labels:")
print(series2)
print()
```

Output:

```
Series with custom labels:
A    10
B    20
C    30
dtype: int64
```

```python
# Method 3: From NumPy array
series3 = pd.Series(arr, index=labels)
print("Series from NumPy array:")
print(series3)
print()
```

Output:

```
Series from NumPy array:
A    10
B    20
C    30
dtype: int64
```

```python
# Method 4: From dictionary (keys become index automatically)
series4 = pd.Series(my_dict)
print("Series from dictionary:")
print(series4)
```

Output:

```
Series from dictionary:
A    10
B    20
C    30
dtype: int64
```

### Series Data Types

```python
# Series can hold different data types
string_series = pd.Series(['A', 'B', 'C'])
print("String series:")
print(string_series)
print()
```

Output:

```
String series:
0    A
1    B
2    C
dtype: object
```

```python
# Even functions (though rarely used)
function_series = pd.Series([sum, print, len])
print("Function series:")
print(function_series)
```

Output:

```
Function series:
0      <built-in function sum>
1    <built-in function print>
2      <built-in function len>
dtype: object
```

### Accessing Series Elements

```python
# Create sample series
countries = pd.Series([1, 2, 3, 4], 
                     index=['USA', 'Germany', 'USSR', 'Japan'])

# Access by label
print("USA value:", countries['USA'])
```

Output:

```
USA value: 1
```

```python
# Access by integer position (if using default index)
simple_series = pd.Series(['A', 'B', 'C'])
print("Element at position 0:", simple_series[0])
```

Output:

```
Element at position 0: A
```

### Series Operations

```python
# Create two series
ser1 = pd.Series([1, 2, 3, 4], index=['USA', 'Germany', 'USSR', 'Japan'])
ser2 = pd.Series([1, 2, 5, 4], index=['USA', 'Germany', 'Italy', 'Japan'])

# Operations are based on index alignment
result = ser1 + ser2
print("Addition result:")
print(result)
# Note: Mismatched indices (USSR, Italy) result in NaN
```

Output:

```
Addition result:
Germany    4.0
Italy      NaN
Japan      8.0
USA        2.0
USSR       NaN
dtype: float64
```

-----

## Working with DataFrames

A DataFrame is a 2-dimensional labeled data structure - think of it as a spreadsheet or SQL table. It's essentially a collection of Series that share the same index.

**Key Characteristics:**

  * 2D structure with rows and columns
  * Each column is a pandas Series
  * Columns can have different data types
  * Both row and column labels
  * Primary data structure for most pandas operations

### Creating DataFrames

```python
from numpy.random import randn
np.random.seed(101)  # For reproducible results

# Create DataFrame with random data
df = pd.DataFrame(randn(5, 4), 
                  index=['A', 'B', 'C', 'D', 'E'],
                  columns=['W', 'X', 'Y', 'Z'])

print("Sample DataFrame:")
print(df)
print()
print("DataFrame shape:", df.shape)  # (rows, columns)
```

Output:

```
Sample DataFrame:
          W         X         Y         Z
A  2.706850  0.628133  0.907969  0.503826
B  0.651118 -0.319318 -0.848077  0.605965
C -2.018168  0.740122  0.528813 -0.589001
D  0.188695 -0.758872 -0.933237  0.955057
E  0.190794  1.978757  2.605967  0.683509

DataFrame shape: (5, 4)
```

### Selecting Columns

```python
# Select single column (returns Series)
print("Column W (Series):")
print(df['W'])
print("Type:", type(df['W']))
print()
```

Output:

```
Column W (Series):
A    2.706850
B    0.651118
C   -2.018168
D    0.188695
E    0.190794
Name: W, dtype: float64
Type: <class 'pandas.core.series.Series'>
```

```python
# Select multiple columns (returns DataFrame)
print("Columns W and Z (DataFrame):")
print(df[['W', 'Z']])
print("Type:", type(df[['W', 'Z']]))
```

Output:

```
Columns W and Z (DataFrame):
          W         Z
A  2.706850  0.503826
B  0.651118  0.605965
C -2.018168 -0.589001
D  0.188695  0.955057
E  0.190794  0.683509
Type: <class 'pandas.core.frame.DataFrame'>
```

### Creating New Columns

```python
# Create new column from existing columns
df['new'] = df['W'] + df['Y']
print("DataFrame with new column:")
print(df)
```

Output:

```
DataFrame with new column:
          W         X         Y         Z       new
A  2.706850  0.628133  0.907969  0.503826  3.614819
B  0.651118 -0.319318 -0.848077  0.605965 -0.196959
C -2.018168  0.740122  0.528813 -0.589001 -1.489355
D  0.188695 -0.758872 -0.933237  0.955057 -0.744542
E  0.190794  1.978757  2.605967  0.683509  2.796761
```

### Dropping Columns

```python
# Drop column (doesn't modify original unless inplace=True)
dropped_df = df.drop('new', axis=1)
print("After dropping 'new' column:")
print(dropped_df)
print()
```

Output:

```
After dropping 'new' column:
          W         X         Y         Z
A  2.706850  0.628133  0.907969  0.503826
B  0.651118 -0.319318 -0.848077  0.605965
C -2.018168  0.740122  0.528813 -0.589001
D  0.188695 -0.758872 -0.933237  0.955057
E  0.190794  1.978757  2.605967  0.683509
```

```python
# To make permanent, use inplace=True
df.drop('new', axis=1, inplace=True)
print("Original DataFrame after permanent drop:")
print(df)
```

Output:

```
Original DataFrame after permanent drop:
          W         X         Y         Z
A  2.706850  0.628133  0.907969  0.503826
B  0.651118 -0.319318 -0.848077  0.605965
C -2.018168  0.740122  0.528813 -0.589001
D  0.188695 -0.758872 -0.933237  0.955057
E  0.190794  1.978757  2.605967  0.683509
```

**Understanding Axes**

  * `axis=0`: Refers to rows (index)
  * `axis=1`: Refers to columns
    This comes from NumPy where shape is `(rows, columns)`, so:
  * Index 0 of shape = number of rows
  * Index 1 of shape = number of columns

-----

## Data Selection and Indexing

### Selecting Rows

There are two main methods for row selection:

**1. `.loc` - Label-based indexing**

```python
# Select row by label
print("Row C using .loc:")
print(df.loc['C'])
print("Type:", type(df.loc['C']))  # Returns Series
```

Output:

```
Row C using .loc:
W   -2.018168
X    0.740122
Y    0.528813
Z   -0.589001
Name: C, dtype: float64
Type: <class 'pandas.core.series.Series'>
```

**2. `.iloc` - Integer position-based indexing**

```python
# Select row by integer position
print("Row at position 2 (which is 'C') using .iloc:")
print(df.iloc[2])
```

Output:

```
Row at position 2 (which is 'C') using .iloc:
W   -2.018168
X    0.740122
Y    0.528813
Z   -0.589001
Name: C, dtype: float64
```

### Selecting Subsets

```python
# Select specific value: row B, column Y
print("Value at row B, column Y:")
print(df.loc['B', 'Y'])
```

Output:

```
Value at row B, column Y:
-0.8480770207843583
```

```python
# Select subset of rows and columns
print("Subset of rows A,B and columns W,Y:")
print(df.loc[['A', 'B'], ['W', 'Y']])
```

Output:

```
Subset of rows A,B and columns W,Y:
         W         Y
A  2.70685  0.907969
B  0.651118 -0.848077
```

-----

## Conditional Selection

One of pandas' most powerful features is conditional selection - filtering data based on conditions.

### Basic Conditional Selection

```python
# Create boolean DataFrame
print("Values greater than 0:")
bool_df = df > 0
print(bool_df)
print()
```

Output:

```
Values greater than 0:
       W      X      Y      Z
A   True   True   True   True
B   True  False  False   True
C  False   True   True  False
D   True  False  False   True
E   True   True   True   True
```

```python
# Use boolean DataFrame to filter (returns NaN for False values)
print("DataFrame with conditional filter (shows NaN for False):")
print(df[bool_df])
```

Output:

```
DataFrame with conditional filter (shows NaN for False):
          W         X         Y         Z
A  2.706850  0.628133  0.907969  0.503826
B  0.651118       NaN       NaN  0.605965
C       NaN  0.740122  0.528813       NaN
D  0.188695       NaN       NaN  0.955057
E  0.190794  1.978757  2.605967  0.683509
```

### Column-based Conditional Selection

```python
# More practical: filter based on column conditions
print("Boolean series for column W > 0:")
w_condition = df['W'] > 0
print(w_condition)
print()
```

Output:

```
Boolean series for column W > 0:
A     True
B     True
C    False
D     True
E     True
Name: W, dtype: bool
```

```python
# Filter entire DataFrame based on column condition
print("Rows where column W > 0:")
filtered_df = df[df['W'] > 0]
print(filtered_df)
print()
```

Output:

```
Rows where column W > 0:
          W         X         Y         Z
A  2.706850  0.628133  0.907969  0.503826
B  0.651118 -0.319318 -0.848077  0.605965
D  0.188695 -0.758872 -0.933237  0.955057
E  0.190794  1.978757  2.605967  0.683509
```

```python
# Get specific column from filtered data
print("Column X where W > 0:")
print(filtered_df['X'])
# Or in one line: df[df['W'] > 0]['X']
```

Output:

```
Column X where W > 0:
A    0.628133
B   -0.319318
D   -0.758872
E    1.978757
Name: X, dtype: float64
```

### Multiple Conditions

```python
# Multiple conditions require & (and) or | (or), not Python's 'and'/'or'
# Each condition must be in parentheses

print("Rows where W > 0 AND Y > 0:")
multiple_condition = df[(df['W'] > 0) & (df['Y'] > 0)]
print(multiple_condition)
print()
```

Output:

```
Rows where W > 0 AND Y > 0:
          W         X         Y         Z
A  2.706850  0.628133  0.907969  0.503826
E  0.190794  1.978757  2.605967  0.683509
```

```python
print("Rows where W > 0 OR Y > 1:")
or_condition = df[(df['W'] > 0) | (df['Y'] > 1)]
print(or_condition)
```

Output:

```
Rows where W > 0 OR Y > 1:
          W         X         Y         Z
A  2.706850  0.628133  0.907969  0.503826
B  0.651118 -0.319318 -0.848077  0.605965
D  0.188695 -0.758872 -0.933237  0.955057
E  0.190794  1.978757  2.605967  0.683509
```

**Why Use & and | Instead of 'and' and 'or'?**
Python's `and/or` work with single boolean values, but pandas works with Series of boolean values. The `&` and `|` operators are designed to handle element-wise operations on arrays/Series.

-----

## Index Management

### Resetting Index

```python
# Reset index to default numerical index
print("Original DataFrame:")
print(df)
print()
```

Output:

```
Original DataFrame:
          W         X         Y         Z
A  2.706850  0.628133  0.907969  0.503826
B  0.651118 -0.319318 -0.848077  0.605965
C -2.018168  0.740122  0.528813 -0.589001
D  0.188695 -0.758872 -0.933237  0.955057
E  0.190794  1.978757  2.605967  0.683509
```

```python
print("After reset_index():")
reset_df = df.reset_index()
print(reset_df)
print()

# To make permanent
# df.reset_index(inplace=True)
```

Output:

```
After reset_index():
  index         W         X         Y         Z
0     A  2.706850  0.628133  0.907969  0.503826
1     B  0.651118 -0.319318 -0.848077  0.605965
2     C -2.018168  0.740122  0.528813 -0.589001
3     D  0.188695 -0.758872 -0.933237  0.955057
4     E  0.190794  1.978757  2.605967  0.683509
```

### Setting New Index

```python
# Add a new column for states
new_index = 'CA NY WY OR CO'.split()
df['states'] = new_index
print("DataFrame with states column:")
print(df)
print()
```

Output:

```
DataFrame with states column:
          W         X         Y         Z states
A  2.706850  0.628133  0.907969  0.503826     CA
B  0.651118 -0.319318 -0.848077  0.605965     NY
C -2.018168  0.740122  0.528813 -0.589001     WY
D  0.188695 -0.758872 -0.933237  0.955057     OR
E  0.190794  1.978757  2.605967  0.683509     CO
```

```python
# Set states column as index
print("Using states as index:")
states_index_df = df.set_index('states')
print(states_index_df)
```

Output:

```
Using states as index:
               W         X         Y         Z
states                                        
CA      2.706850  0.628133  0.907969  0.503826
NY      0.651118 -0.319318 -0.848077  0.605965
WY     -2.018168  0.740122  0.528813 -0.589001
OR      0.188695 -0.758872 -0.933237  0.955057
CO      0.190794  1.978757  2.605967  0.683509
```

-----

## Multi-level Indexing

Multi-level indexing (hierarchical indexing) allows you to have multiple levels of row and/or column labels.

### Creating Multi-level Index

```python
# Create multi-level index data
outside = ['G1', 'G1', 'G1', 'G2', 'G2', 'G2']
inside = [1, 2, 3, 1, 2, 3]
hier_index = list(zip(outside, inside))
hier_index = pd.MultiIndex.from_tuples(hier_index)

print("Hierarchical index:")
print(hier_index)
print()
```

Output:

```
Hierarchical index:
MultiIndex([('G1', 1),
            ('G1', 2),
            ('G1', 3),
            ('G2', 1),
            ('G2', 2),
            ('G2', 3)],
           )
```

```python
# Create DataFrame with multi-level index
np.random.seed(101)
multi_df = pd.DataFrame(randn(6, 2), index=hier_index, columns=['A', 'B'])
print("Multi-level DataFrame:")
print(multi_df)
```

Output:

```
Multi-level DataFrame:
           A         B
G1 1  2.706850  0.628133
   2  0.907969  0.503826
   3  0.651118 -0.319318
G2 1 -0.848077  0.605965
   2 -2.018168  0.740122
   3  0.528813 -0.589001
```

### Accessing Multi-level Data

```python
# Name the index levels
multi_df.index.names = ['Group', 'Num']
print("Multi-level DataFrame with named indices:")
print(multi_df)
print()
```

Output:

```
Multi-level DataFrame with named indices:
             A         B
Group Num              
G1    1    2.706850  0.628133
      2    0.907969  0.503826
      3    0.651118 -0.319318
G2    1   -0.848077  0.605965
      2   -2.018168  0.740122
      3    0.528813 -0.589001
```

```python
# Access outer level
print("All data for Group G1:")
print(multi_df.loc['G1'])
print()
```

Output:

```
All data for Group G1:
          A         B
Num              
1    2.706850  0.628133
2    0.907969  0.503826
3    0.651118 -0.319318
```

```python
# Access specific cell
print("Group G2, Num 2, Column B:")
print(multi_df.loc['G2', 2]['B'])
# Or: multi_df.loc['G2'].loc[2]['B']
```

Output:

```
Group G2, Num 2, Column B:
0.7401220058558049
```

**Cross Sections with `.xs()`**

```python
# Get cross-section: all rows where Num=1 across all groups
print("Cross-section where Num=1:")
print(multi_df.xs(1, level='Num'))
```

Output:

```
Cross-section where Num=1:
            A         B
Group              
G1     2.706850  0.628133
G2    -0.848077  0.605965
```

-----

## Summary

In this tutorial, we covered the fundamental concepts of pandas:

**Key Takeaways:**

  * **Series:** One-dimensional labeled arrays, building blocks of DataFrames
  * **DataFrames:** Two-dimensional labeled data structures, main pandas workhorse
  * **Selection Methods:**
      * **Columns:** `df['column']` or `df[['col1', 'col2']]`
      * **Rows:** `df.loc['label']` or `df.iloc[position]`
  * **Conditional Selection:** Use `&` and `|` for multiple conditions, not `and/or`
  * **Index Management:** `reset_index()` and `set_index()` for index manipulation
  * **Multi-level Indexing:** Hierarchical data organization with `.xs()` for cross-sections

**Common Patterns to Remember:**

```python
# Basic DataFrame operations
df['new_column'] = df['col1'] + df['col2']  # Create column
df.drop('column', axis=1, inplace=True)     # Drop column
df[df['column'] > value]                    # Filter rows
df[(df['col1'] > val1) & (df['col2'] < val2)]  # Multiple conditions

# Index operations
df.reset_index(inplace=True)                # Reset to numerical
df.set_index('column', inplace=True)        # Set column as index
```

**What's Next?**
In the next tutorial, we'll explore:

  * Working with missing data
  * GroupBy operations
  * Merging and joining DataFrames
  * Data input/output (CSV, Excel, SQL)
  * More advanced pandas operations

**Practice Exercises:**

  * Create a DataFrame with student names, grades, and subjects
  * Filter students with grades above 85
  * Add a new column calculating grade percentages
  * Practice multi-condition filtering
  * Experiment with different indexing methods

This foundation will prepare you for more advanced pandas operations and real-world data analysis tasks\!
