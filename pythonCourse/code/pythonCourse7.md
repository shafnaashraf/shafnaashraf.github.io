# Tutorial 3: Pandas Built-in Data Visualization

## Introduction

Welcome to Tutorial 3 of our Python for Data Science series! In this tutorial, we'll explore Pandas' built-in data visualization capabilities. Pandas provides a convenient way to create quick visualizations directly from your DataFrames, making data exploration much easier.

### What You'll Learn
- How Pandas visualization works with Matplotlib
- Different types of plots available in Pandas
- When to use Pandas plots vs. other visualization libraries
- Practical examples for each plot type

## Prerequisites

Before starting, make sure you have the following libraries installed:
```bash
pip install pandas matplotlib seaborn jupyter
```

## Setting Up Your Environment

Let's start by importing the necessary libraries and setting up our plotting environment:

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# This magic command allows plots to display inline in Jupyter notebooks
%matplotlib inline

# Optional: Import seaborn for better styling
import seaborn as sns
```

### Why Import Seaborn?
Even though we're focusing on Pandas visualization, importing Seaborn automatically improves the visual style of our plots, making them look more professional with minimal effort.

## Understanding Pandas Plotting

Pandas visualization is built on top of **Matplotlib**, which means:
- You get the convenience of Pandas syntax
- You can still use Matplotlib parameters for customization
- The plots integrate seamlessly with your data analysis workflow

### Two Ways to Create Plots in Pandas

There are two main approaches to creating plots with Pandas:

#### Method 1: Using the `kind` parameter
```python
df.plot(kind='hist')  # Creates a histogram
```

#### Method 2: Direct method calls
```python
df.plot.hist()  # Also creates a histogram
```

We'll use **Method 2** throughout this tutorial as it's more readable and intuitive.

## Sample Data Setup

Let's create some sample datasets to work with:

```python
# Creating a time series dataset
import numpy as np
from datetime import datetime, timedelta

# Time series data (df1)
dates = pd.date_range(start='2023-01-01', periods=100, freq='D')
np.random.seed(42)
df1 = pd.DataFrame({
    'A': np.random.randn(100).cumsum(),
    'B': np.random.randn(100).cumsum() + 5,
    'C': np.random.randn(100).cumsum() - 3
}, index=dates)

print("Time Series DataFrame (df1):")
print(df1.head())
```

```python
# Simple sequential data (df2)
df2 = pd.DataFrame({
    'A': [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
    'B': [2, 4, 1, 8, 3, 6, 9, 5, 7, 10],
    'C': [5, 3, 8, 1, 9, 2, 7, 4, 6, 8]
})

print("\nSequential DataFrame (df2):")
print(df2)
```

## Plot Types in Pandas

Now let's explore each type of plot available in Pandas with detailed explanations and examples.

### 1. Histogram

**What it is:** A histogram shows the distribution of numerical data by dividing it into bins and counting frequency.

**When to use:** To understand the distribution pattern of your data, identify outliers, or check if data is normally distributed.

```python
# Basic histogram
df1['A'].plot.hist()
plt.title('Distribution of Column A')
plt.xlabel('Values')
plt.ylabel('Frequency')
plt.show()
```

```python
# Histogram with custom bins
df1['A'].plot.hist(bins=20, alpha=0.7, color='skyblue')
plt.title('Distribution of Column A (20 bins)')
plt.xlabel('Values')
plt.ylabel('Frequency')
plt.show()
```

**Key Parameters:**
- `bins`: Number of bins (default is 10)
- `alpha`: Transparency (0-1)
- `color`: Color of the bars

### 2. Line Plot

**What it is:** Shows trends over time by connecting data points with lines.

**When to use:** Perfect for time series data, showing trends, or comparing multiple variables over time.

```python
# Single line plot
df1['A'].plot.line()
plt.title('Time Series Plot of Column A')
plt.xlabel('Date')
plt.ylabel('Value')
plt.show()
```

```python
# Multiple lines on same plot
df1.plot.line()
plt.title('Multiple Time Series')
plt.xlabel('Date')
plt.ylabel('Value')
plt.legend()
plt.show()
```

```python
# Customized line plot
df1['A'].plot.line(figsize=(12, 6), linewidth=2, color='red', linestyle='--')
plt.title('Customized Line Plot')
plt.xlabel('Date')
plt.ylabel('Value')
plt.grid(True, alpha=0.3)
plt.show()
```

**Key Parameters:**
- `figsize`: Figure size as (width, height)
- `linewidth` or `lw`: Width of the line
- `linestyle` or `ls`: Style of line ('--', ':', '-.')
- `color`: Color of the line

### 3. Bar Plot

**What it is:** Displays categorical data with rectangular bars.

**When to use:** Comparing quantities across different categories.

```python
# Basic bar plot
df2.plot.bar()
plt.title('Bar Plot of All Columns')
plt.xlabel('Index')
plt.ylabel('Values')
plt.xticks(rotation=0)  # Keep x-axis labels horizontal
plt.show()
```

```python
# Stacked bar plot
df2.plot.bar(stacked=True)
plt.title('Stacked Bar Plot')
plt.xlabel('Index')
plt.ylabel('Values')
plt.xticks(rotation=0)
plt.show()
```

```python
# Horizontal bar plot
df2.plot.barh()
plt.title('Horizontal Bar Plot')
plt.xlabel('Values')
plt.ylabel('Index')
plt.show()
```

**Key Parameters:**
- `stacked`: Whether to stack bars (True/False)
- `width`: Width of bars
- `color`: Color(s) of bars

### 4. Scatter Plot

**What it is:** Shows relationships between two numerical variables using dots.

**When to use:** To identify correlations, patterns, or outliers between two variables.

```python
# Basic scatter plot
df1.plot.scatter(x='A', y='B')
plt.title('Scatter Plot: A vs B')
plt.show()
```

```python
# Scatter plot with color mapping (3D information)
df1.plot.scatter(x='A', y='B', c='C', colormap='viridis', s=50)
plt.title('Scatter Plot with Color Mapping')
plt.colorbar()  # Add color bar to show scale
plt.show()
```

```python
# Scatter plot with size mapping
df1.plot.scatter(x='A', y='B', s=df1['C']*50)  # s controls size
plt.title('Scatter Plot with Size Mapping')
plt.show()
```

**Key Parameters:**
- `x`, `y`: Column names for x and y axes
- `c`: Column for color mapping
- `s`: Column for size mapping or fixed size
- `colormap`: Color scheme for color mapping

### 5. Area Plot

**What it is:** Like a line plot but with the area under the line filled with color.

**When to use:** To show cumulative totals or emphasize the magnitude of values over time.

```python
# Basic area plot
df2.plot.area()
plt.title('Area Plot')
plt.xlabel('Index')
plt.ylabel('Values')
plt.show()
```

```python
# Area plot with transparency
df2.plot.area(alpha=0.4)
plt.title('Transparent Area Plot')
plt.xlabel('Index')
plt.ylabel('Values')
plt.show()
```

**Key Parameters:**
- `alpha`: Transparency level
- `stacked`: Whether areas are stacked (default True)

### 6. Box Plot

**What it is:** Shows the distribution of data through quartiles, highlighting outliers.

**When to use:** To compare distributions across categories or identify outliers.

```python
# Box plot of all columns
df1.plot.box()
plt.title('Box Plot of All Columns')
plt.ylabel('Values')
plt.show()
```

```python
# Box plot with custom styling
df1.plot.box(vert=False)  # Horizontal box plot
plt.title('Horizontal Box Plot')
plt.xlabel('Values')
plt.show()
```

**Understanding Box Plots:**
- **Box**: 25th to 75th percentile (IQR)
- **Line in box**: Median (50th percentile)
- **Whiskers**: Extend to 1.5 × IQR
- **Dots**: Outliers beyond whiskers

### 7. Hexagonal Bin Plot

**What it is:** A 2D histogram using hexagonal bins, useful for large datasets.

**When to use:** When you have too many points for a regular scatter plot.

```python
# Create larger dataset for hex plot demonstration
large_df = pd.DataFrame({
    'A': np.random.randn(1000),
    'B': np.random.randn(1000)
})

# Hexagonal bin plot
large_df.plot.hexbin(x='A', y='B', gridsize=25)
plt.title('Hexagonal Bin Plot')
plt.show()
```

```python
# Hex plot with custom color map
large_df.plot.hexbin(x='A', y='B', gridsize=20, cmap='Blues')
plt.title('Hexagonal Bin Plot (Custom Colors)')
plt.show()
```

**Key Parameters:**
- `gridsize`: Size of hexagonal bins
- `cmap`: Color map

### 8. Density Plot (KDE)

**What it is:** Shows the probability density function of your data using Kernel Density Estimation.

**When to use:** To see smooth distribution curves instead of histograms.

```python
# Density plot for single column
df1['A'].plot.density()
plt.title('Density Plot of Column A')
plt.xlabel('Values')
plt.ylabel('Density')
plt.show()
```

```python
# Density plots for all columns
df1.plot.density()
plt.title('Density Plots for All Columns')
plt.xlabel('Values')
plt.ylabel('Density')
plt.legend()
plt.show()
```

**Alternative syntax:**
```python
# 'kde' also works
df1['A'].plot.kde()
plt.title('KDE Plot')
plt.show()
```

## Advanced Customization

### Combining Pandas with Matplotlib

You can enhance Pandas plots with Matplotlib features:

```python
# Create a subplot layout
fig, axes = plt.subplots(2, 2, figsize=(12, 10))

# Different plots in each subplot
df1['A'].plot.hist(ax=axes[0,0], title='Histogram of A')
df1.plot.line(ax=axes[0,1], title='Line Plot')
df1.plot.scatter(x='A', y='B', ax=axes[1,0], title='Scatter Plot')
df1.plot.box(ax=axes[1,1], title='Box Plot')

plt.tight_layout()
plt.show()
```

### Styling Tips

```python
# Set a consistent style
plt.style.use('seaborn-v0_8')  # Use seaborn style

# Create a plot with custom styling
ax = df1['A'].plot.line(figsize=(10, 6), linewidth=3, color='navy')
ax.set_facecolor('#f8f9fa')
ax.grid(True, linestyle='--', alpha=0.7)
ax.set_title('Professional Looking Plot', fontsize=16, fontweight='bold')
ax.set_xlabel('Date', fontsize=12)
ax.set_ylabel('Values', fontsize=12)
plt.show()
```

## When to Use Pandas Plots vs. Other Libraries

### Use Pandas plots when:
- You need quick data exploration
- Working directly with DataFrames
- Creating simple, standard plots
- Doing rapid prototyping

### Use Matplotlib when:
- You need complete control over plot elements
- Creating complex, custom visualizations
- Working with non-DataFrame data

### Use Seaborn when:
- Creating statistical plots
- Working with categorical data
- Need publication-ready plots with minimal code
- Want automatic statistical calculations

## Common Pitfalls and Solutions

### Problem 1: Plots not showing
```python
# Solution: Add plt.show()
df.plot.line()
plt.show()  # Don't forget this!
```

### Problem 2: Overlapping labels
```python
# Solution: Rotate labels or adjust figure size
df.plot.bar(figsize=(12, 6))
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

### Problem 3: Too many legend entries
```python
# Solution: Turn off legend for some plots
df.plot.line(legend=False)
plt.title('Plot without Legend')
plt.show()
```

## Practice Exercises

### Exercise 1: Sales Analysis
Create a dataset representing monthly sales and visualize it:

```python
# Create sample sales data
months = pd.date_range(start='2023-01-01', periods=12, freq='M')
sales_data = pd.DataFrame({
    'Product_A': np.random.randint(100, 500, 12),
    'Product_B': np.random.randint(150, 400, 12),
    'Product_C': np.random.randint(200, 600, 12)
}, index=months)

# Your task: Create different visualizations to analyze this sales data
```

### Exercise 2: Student Grades Analysis
```python
# Create sample student data
students_data = pd.DataFrame({
    'Math': np.random.normal(75, 15, 100),
    'Science': np.random.normal(80, 12, 100),
    'English': np.random.normal(78, 10, 100)
})

# Your task: Analyze the grade distributions
```

## Best Practices

1. **Always add titles and labels** to make plots self-explanatory
2. **Use appropriate plot types** for your data
3. **Keep it simple** - don't overcomplicate quick exploratory plots
4. **Use consistent colors** across related plots
5. **Consider your audience** - adjust complexity accordingly

## Summary

Pandas built-in visualization provides a perfect balance between ease of use and functionality. Here's what we covered:

- **Histograms**: For data distribution
- **Line plots**: For trends over time
- **Bar plots**: For categorical comparisons
- **Scatter plots**: For relationships between variables
- **Area plots**: For cumulative data
- **Box plots**: For distribution summaries
- **Hex plots**: For large datasets
- **Density plots**: For smooth distributions

Remember: Pandas plots are excellent for **quick exploration** and **initial analysis**. For publication-ready or highly customized visualizations, consider using Seaborn or pure Matplotlib.

## Next Steps

In our next tutorial, we'll dive deeper into **Matplotlib fundamentals**, giving you complete control over your visualizations. We'll learn how to create complex, multi-panel plots and customize every aspect of your figures.

Happy plotting! 🎨📊
