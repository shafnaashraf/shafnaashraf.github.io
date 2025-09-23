# Pandas in Python: Data Analysis - Part 2

-----

## 1\. Handling Missing Data (NaN)

When you're working with real-world data, it's common to encounter missing values. Pandas represents these missing values as `NaN` (Not a Number). You can either drop rows or columns with `NaN` values or fill them in with a replacement value.

Let's start by creating a DataFrame with some missing values to work with. We'll use a dictionary to build our DataFrame.

```python
import numpy as np
import pandas as pd

# Create a dictionary with missing values represented by np.nan
d = {'A': [1, 2, np.nan], 'B': [5, np.nan, np.nan], 'C': [1, 2, 3]}

# Create the DataFrame
df = pd.DataFrame(d)

print(df)
```

**Output:**

```
     A    B  C
0  1.0  5.0  1
1  2.0  NaN  2
2  NaN  NaN  3
```

As you can see, our DataFrame has missing values in columns 'A' and 'B'.

### The `dropna()` Method

The `dropna()` method is used to remove rows or columns that contain missing values.

#### Dropping Rows

By default, `dropna()` removes any **row** that has at least one missing value.

```python
# Drop any rows with a missing value
df.dropna()
```

**Output:**

```
     A    B  C
0  1.0  5.0  1
```

Notice that only row `0` remains because rows `1` and `2` both had at least one `NaN` value.

#### Dropping Columns

You can also drop columns instead of rows by using the `axis=1` argument.

```python
# Drop any columns with a missing value
df.dropna(axis=1)
```

**Output:**

```
   C
0  1
1  2
2  3
```

In this case, only column 'C' remains because columns 'A' and 'B' had missing values.

#### Using the `thresh` Argument

Sometimes, you might not want to drop a row unless it has a certain number of missing values. The `thresh` argument lets you specify a minimum number of **non-`NaN`** values a row must have to be kept.

Let's look at our original DataFrame `df` again.

  - Row `0` has 3 non-`NaN` values.
  - Row `1` has 2 non-`NaN` values.
  - Row `2` has 1 non-`NaN` value.

<!-- end list -->

```python
# Drop rows that have less than 2 non-NaN values
df.dropna(thresh=2)
```

**Output:**

```
     A    B  C
0  1.0  5.0  1
1  2.0  NaN  2
```

The output keeps rows `0` and `1` because they both have at least two non-`NaN` values, while row `2` (with only one non-`NaN` value) is dropped.

### The `fillna()` Method

Instead of removing data, you can replace missing values with a substitute using `fillna()`.

```python
# Fill all missing values with a string
df.fillna(value='FILL VALUE')
```

**Output:**

```
             A           B  C
0          1.0         5.0  1
1          2.0  FILL VALUE  2
2   FILL VALUE  FILL VALUE  3
```

A more practical approach is to fill missing values with a meaningful metric, like the mean of the column. This is a common practice in data analysis.

```python
# Fill missing values in column 'A' with the mean of column 'A'
df['A'].fillna(value=df['A'].mean())
```

**Output:**

```
0    1.0
1    2.0
2    1.5
Name: A, dtype: float64
```

-----

## 2\. Grouping Data with `groupby()`

The `groupby()` method is one of the most powerful and frequently used functions in pandas. It allows you to **group rows** of data based on the values in a particular column and then **apply an aggregate function** to those groups. An aggregate function is any function that takes multiple values and returns a single value, such as `sum()`, `mean()`, `std()`, `min()`, or `max()`.

Let's set up a new DataFrame to demonstrate this.

```python
# Create a new dictionary and DataFrame for groupby examples
data = {'Company': ['GOOG', 'GOOG', 'MSFT', 'MSFT', 'FB', 'FB'],
        'Person': ['Sam', 'Charlie', 'Amy', 'Vanessa', 'Carl', 'Sarah'],
        'Sales': [200, 120, 340, 124, 243, 350]}
df = pd.DataFrame(data)

print(df)
```

**Output:**

```
  Company   Person  Sales
0    GOOG      Sam    200
1    GOOG  Charlie    120
2    MSFT      Amy    340
3    MSFT  Vanessa    124
4      FB     Carl    243
5      FB    Sarah    350
```

### The GroupBy Process

The `groupby()` process involves three steps:

1.  **Splitting:** The data is split into groups based on some criterion.
2.  **Applying:** A function is applied to each group independently.
3.  **Combining:** The results of the applied functions are combined into a new DataFrame.

<!-- end list -->

```python
# Group the DataFrame by the 'Company' column
by_company = df.groupby('Company')
```

When you just call `groupby()`, it returns a `DataFrameGroupBy` object. To get meaningful results, you need to apply an aggregate function to it.

For example, to find the mean sales for each company:

```python
# Calculate the mean sales for each company
by_company.mean()
```

**Output:**

```
         Sales
Company
FB       296.5
GOOG     160.0
MSFT     232.0
```

Pandas automatically ignores non-numeric columns (`Person` in this case) since you can't calculate a mean of strings.

You can chain this process into a single, more common line of code:

```python
# A more common way to perform the operation
df.groupby('Company').mean()
```

### Other Useful Aggregate Functions

You can use a variety of aggregate functions with `groupby()`:

| Function | Description |
|---|---|
| `.sum()` | Sum of values within each group |
| `.std()` | Standard deviation of values within each group |
| `.count()` | Counts the number of occurrences in each group |
| `.min()` / `.max()` | Finds the minimum/maximum value in each group |

Here's an example using `.sum()`:

```python
# Calculate the total sales for each company
df.groupby('Company').sum()
```

**Output:**

```
         Sales
Company
FB         593
GOOG       320
MSFT       464
```

### The `describe()` Method with `groupby()`

The `describe()` method is an excellent way to get a quick summary of your grouped data.

```python
# Get a summary of statistics for each group
df.groupby('Company').describe()
```

**Output:**

```
        Sales
        count   mean         std    min    25%    50%    75%    max
Company
FB        2.0  296.5   75.660426  243.0  269.75  296.5  323.25  350.0
GOOG      2.0  160.0   56.568542  120.0  140.00  160.0  180.00  200.0
MSFT      2.0  232.0  152.735065  124.0  178.00  232.0  286.00  340.0
```

This gives you a comprehensive statistical overview of the numeric columns for each group, including count, mean, standard deviation, and quartile values. You can even transpose this table for better readability using `.T`.

```python
df.groupby('Company').describe().transpose()
```

-----

## 3\. Merging, Joining, and Concatenating DataFrames

In data analysis, you often need to combine multiple DataFrames. Pandas provides three main methods for this: `concat()`, `merge()`, and `join()`.

### Concatenating with `pd.concat()`

**Concatenation** essentially "glues" DataFrames together along an axis. You can think of it as stacking them on top of each other or placing them side-by-side. The dimensions of the DataFrames must match along the axis you are concatenating on.

Let's create three sample DataFrames.

```python
# Creating example DataFrames
df1 = pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
                    'B': ['B0', 'B1', 'B2', 'B3'],
                    'C': ['C0', 'C1', 'C2', 'C3'],
                    'D': ['D0', 'D1', 'D2', 'D3']},
                   index=[0, 1, 2, 3])

df2 = pd.DataFrame({'A': ['A4', 'A5', 'A6', 'A7'],
                    'B': ['B4', 'B5', 'B6', 'B7'],
                    'C': ['C4', 'C5', 'C6', 'C7'],
                    'D': ['D4', 'D5', 'D6', 'D7']},
                   index=[4, 5, 6, 7])

df3 = pd.DataFrame({'A': ['A8', 'A9', 'A10', 'A11'],
                    'B': ['B8', 'B9', 'B10', 'B11'],
                    'C': ['C8', 'C9', 'C10', 'C11'],
                    'D': ['D8', 'D9', 'D10', 'D11']},
                   index=[8, 9, 10, 11])

# Concatenate along the rows (default)
pd.concat([df1, df2, df3])
```

**Output:**

```
      A    B    C    D
0    A0   B0   C0   D0
1    A1   B1   C1   D1
2    A2   B2   C2   D2
3    A3   B3   C3   D3
4    A4   B4   C4   D4
5    A5   B5   C5   D5
6    A6   B6   C6   D6
7    A7   B7   C7   D7
8    A8   B8   C8   D8
9    A9   B9   C9   D9
10  A10  B10  C10  D10
11  A11  B11  C11  D11
```

You can also concatenate along the columns using `axis=1`. This is useful for combining DataFrames with different columns but the same index.

```python
# Concatenate along the columns
pd.concat([df1, df2, df3], axis=1)
```

**Note:** If the indexes don't align, pandas will fill the non-matching areas with `NaN`.

### Merging with `pd.merge()`

**Merging** combines DataFrames based on a shared column, much like a SQL join. It's a powerful way to bring together data from different sources that have a common identifier.

Let's create two DataFrames to merge.

```python
# Create new DataFrames for merging
left = pd.DataFrame({'key': ['K0', 'K1', 'K2', 'K3'],
                     'A': ['A0', 'A1', 'A2', 'A3'],
                     'B': ['B0', 'B1', 'B2', 'B3']})

right = pd.DataFrame({'key': ['K0', 'K1', 'K2', 'K3'],
                      'C': ['C0', 'C1', 'C2', 'C3'],
                      'D': ['D0', 'D1', 'D2', 'D3']})

# Merge the DataFrames on the 'key' column
pd.merge(left, right, on='key')
```

**Output:**

```
  key   A   B   C   D
0  K0  A0  B0  C0  D0
1  K1  A1  B1  C1  D1
2  K2  A2  B2  C2  D2
3  K3  A3  B3  C3  D3
```

You can also merge on multiple keys by passing a list of column names to the `on` argument.

### Joining with `.join()`

**Joining** is a convenient method for combining two DataFrames on their **index**. It's very similar to a merge, but it defaults to using the index as the key.

Let's create two new DataFrames with their own unique indexes.

```python
# Create DataFrames for joining
left = pd.DataFrame({'A': ['A0', 'A1', 'A2'],
                     'B': ['B0', 'B1', 'B2']},
                    index=['K0', 'K1', 'K2'])

right = pd.DataFrame({'C': ['C0', 'C2', 'C3'],
                      'D': ['D0', 'D2', 'D3']},
                     index=['K0', 'K2', 'K3'])

# Join the DataFrames
left.join(right)
```

**Output:**

```
      A   B    C    D
K0   A0  B0   C0   D0
K1   A1  B1  NaN  NaN
K2   A2  B2   C2   D2
```

Notice that since the `K1` index from the `left` DataFrame has no match in the `right` DataFrame, the values for columns `C` and `D` are filled with `NaN`.

-----

## 4\. Other Useful Pandas Operations

### Unique Values

Finding unique values in a column is a common task. Pandas offers three handy methods for this.

```python
df = pd.DataFrame({'col1': [1, 2, 3, 4],
                   'col2': [444, 444, 555, 666],
                   'col3': ['abc', 'def', 'ghi', 'xyz']})

# Find the unique values in 'col2'
df['col2'].unique()
```

**Output:** `array([444, 555, 666])`

If you just want the **number** of unique values, use `nunique()`.

```python
# Count the number of unique values in 'col2'
df['col2'].nunique()
```

**Output:** `3`

To see how many times each unique value appears, use `value_counts()`.

```python
# Count the occurrences of each unique value
df['col2'].value_counts()
```

**Output:**

```
444    2
555    1
666    1
Name: col2, dtype: int64
```

-----

### Applying Functions

The `apply()` method is incredibly powerful. It allows you to apply a custom function to every element in a Series or DataFrame. A common use case is with **lambda expressions** for quick, one-off operations.

Let's say we want to double the values in `col2`.

```python
# Create a simple function
def times2(x):
    return x * 2

# Apply the function to 'col2'
df['col2'].apply(times2)
```

**Output:**

```
0     888
1     888
2    1110
3    1332
Name: col2, dtype: int64
```

Using a lambda expression is much more concise for this.

```python
# Use a lambda expression to double each value
df['col2'].apply(lambda x: x * 2)
```

You can even apply built-in functions, like `len()` to get the length of each string in a column.

```python
# Apply the len() function to 'col3'
df['col3'].apply(len)
```

**Output:**

```
0    3
1    3
2    3
3    3
Name: col3, dtype: int64
```

-----

### Other Operations

  - **Dropping columns:** To permanently drop a column, you need to specify `axis=1` and `inplace=True`.
    `df.drop('col1', axis=1, inplace=True)`

  - **Getting column/index names:** You can access the column or index names as an attribute of the DataFrame.
    `df.columns`
    `df.index`

  - **Sorting values:** Use `sort_values()` to sort a DataFrame by a specific column.
    `df.sort_values(by='col2')`

  - **Checking for null values:** The `isnull()` method returns a DataFrame of booleans indicating where `NaN` values are present.
    `df.isnull()`

  - **Pivot Tables:** Pivot tables are a powerful way to summarize and rearrange data. If you're familiar with them from Excel, you'll find them very useful in pandas.
    `df.pivot_table(values='D', index=['A', 'B'], columns='C')`

-----

## 5\. Reading and Writing Data with Pandas

Pandas is highly versatile and can read and write data from a wide variety of sources. We'll focus on four key formats: CSV, Excel, HTML (from the web), and SQL databases.

**Note:** For HTML and SQL, you may need to install additional libraries.

  - For HTML: `pip install lxml html5lib beautifulsoup4`
  - For SQL: `pip install sqlalchemy` and a specific driver for your database (e.g., `psycopg2` for PostgreSQL or `pymysql` for MySQL).

Make sure your data files (like `Example.csv` and `Example.xlsx`) are in the same directory as your Jupyter Notebook. You can check your notebook's current working directory by running `pwd` in a code cell.

### CSV Files

CSV (Comma-Separated Values) is the most common format for raw data.

#### Reading a CSV File

The `pd.read_csv()` function is used to load data from a CSV file into a DataFrame.

```python
# Read the example CSV file
df = pd.read_csv('Example.csv')

print(df)
```

**Output:**

```
   A  B  C  D
0  0  1  2  3
1  4  5  6  7
2  8  9  1  2
3  5  3  4  5
```

Pandas has a dedicated `read_` function for many file types. You can type `pd.read_` and press `Tab` to see the full list of options, including `read_excel`, `read_json`, `read_sql`, and more.

#### Writing to a CSV File

To save a DataFrame to a CSV file, use the `.to_csv()` method.

```python
# Save the DataFrame to a new CSV file
df.to_csv('My_output.csv', index=False)
```

The `index=False` argument prevents pandas from writing the DataFrame's index as a new column in the CSV file, which is often desired. If you don't use this, the index will be saved as an unnamed column.

### Excel Files

Pandas can read and write Excel files, but it's important to remember that it only handles the data itself, not formulas, macros, or images.

#### Reading an Excel File

Use `pd.read_excel()` and specify the file path and the `sheet_name`.

```python
# Read data from a specific sheet in an Excel file
excel_df = pd.read_excel('Example.xlsx', sheet_name='Sheet1')

print(excel_df)
```

**Output:**

```
   A  B  C  D
0  0  1  2  3
1  4  5  6  7
2  8  9  1  2
3  5  3  4  5
```

#### Writing to an Excel File

To save a DataFrame to an Excel file, use the `.to_excel()` method.

```python
# Save the DataFrame to a new Excel file
excel_df.to_excel('Excel_Sample_2.xlsx', sheet_name='NewSheet')
```

### HTML (Web Scraping)

Pandas can read tables directly from HTML web pages using `pd.read_html()`. This is a quick and easy way to perform basic web scraping of tabular data.

```python
# URL of a web page containing a table
url = 'https://www.fdic.gov/resources/resolutions/bank-failures/failed-bank-list/'

# Read tables from the URL
failed_banks_list = pd.read_html(url)

# pd.read_html() returns a list of DataFrames for each table found on the page
# The first table is usually the one we want
failed_banks_df = failed_banks_list[0]

print(failed_banks_df.head())
```

**Output:**

```
                       Bank Name                 City  ST  ...  Cert  \
0                    The First State                 ...  ...   ...
1                     The First Bank                  ...  ...   ...
2  Almena State Bank                   ...  ...   ...
3  First Virginia Bank & Trust Company ...  ...   ...
4  First Virginia Bank & Trust Company ...  ...   ...
```

You can then proceed with your analysis on this DataFrame.

### SQL Databases

Pandas offers a way to interact with SQL databases, treating tables as DataFrames. A common approach is to use the `sqlalchemy` library to create a connection engine.

Here is a simplified example showing how to create a temporary in-memory database and read/write a DataFrame from/to it.

```python
from sqlalchemy import create_engine

# Create a temporary in-memory SQLite database
engine = create_engine('sqlite:///:memory:')

# Write a DataFrame to a new SQL table called 'my_table'
df.to_sql('my_table', con=engine)

# Read the SQL table back into a new DataFrame
sql_df = pd.read_sql('my_table', con=engine)

print(sql_df)
```

**Output:**

```
   index  A  B  C  D
0      0  0  1  2  3
1      1  4  5  6  7
2      2  8  9  1  2
3      3  5  3  4  5
```

Notice that `pd.to_sql()` saves the index as a new column by default.

