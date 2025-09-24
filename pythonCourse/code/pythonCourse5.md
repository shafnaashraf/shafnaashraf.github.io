# Python Data Visualization with Matplotlib - Complete Beginner's Guide

## Table of Contents
1. [Introduction to Matplotlib](#introduction-to-matplotlib)
2. [Installation](#installation)
3. [Basic Setup and Imports](#basic-setup-and-imports)
4. [Understanding Figures and Axes](#understanding-figures-and-axes)
5. [Functional vs Object-Oriented Approach](#functional-vs-object-oriented-approach)
6. [Creating Basic Plots](#creating-basic-plots)
7. [Working with Subplots](#working-with-subplots)
8. [Customizing Your Plots](#customizing-your-plots)
9. [Saving Figures](#saving-figures)
10. [Best Practices and Tips](#best-practices-and-tips)

---

## Introduction to Matplotlib

Matplotlib (pronounced "Matt-plot-lib") is the most popular plotting library for Python. It gives you complete control over almost every aspect of a figure or plot and is designed to have a very similar feel to MATLAB's plotting capabilities.

### Why Learn Matplotlib?

- **Foundation for other libraries**: Libraries like Seaborn are built on top of Matplotlib
- **Complete control**: Fine-tune every aspect of your visualizations
- **Industry standard**: Widely used in data science and scientific computing
- **Integration**: Works seamlessly with NumPy and Pandas

### Key Resources

The official Matplotlib website (https://matplotlib.org) contains:
- Installation instructions
- **Gallery**: Extensive examples of different plot types (https://matplotlib.org/gallery)
- Documentation and tutorials

**Pro Tip**: The gallery is your best friend when learning Matplotlib. Find a plot similar to what you want, then study the code!

---

## Installation

Before we begin, you need to install Matplotlib:

### Using Anaconda (Recommended):
```bash
conda install matplotlib
```

### Using pip:
```bash
pip install matplotlib
```

### Verify Installation:
```python
import matplotlib
print(matplotlib.__version__)
```

---

## Basic Setup and Imports

Every Matplotlib script starts with these essential imports:

```python
import matplotlib.pyplot as plt
import numpy as np

# For Jupyter Notebooks only - displays plots inline
%matplotlib inline
```

**What each line does:**
- `import matplotlib.pyplot as plt`: Imports the plotting interface (we use 'plt' as shorthand)
- `import numpy as np`: For creating sample data
- `%matplotlib inline`: Jupyter magic command to display plots in the notebook

### Creating Sample Data

Let's create some sample data to work with:

```python
# Create sample data
x = np.linspace(0, 5, 11)  # 11 points from 0 to 5
y = x ** 2                 # y = x squared

print("x:", x)
print("y:", y)
```

**Output:**
```
x: [0.  0.5 1.  1.5 2.  2.5 3.  3.5 4.  4.5 5. ]
y: [ 0.    0.25  1.    2.25  4.    6.25  9.   12.25 16.   20.25 25.  ]
```

---

## Understanding Figures and Axes

Before diving into plotting, it's crucial to understand Matplotlib's anatomy:

### The Hierarchy

```
Figure (The entire window/canvas)
└── Axes (The actual plot area with data)
    ├── X-axis
    ├── Y-axis
    ├── Title
    ├── Legend
    └── Plot elements (lines, points, etc.)
```

**Think of it like a picture frame:**
- **Figure** = The entire frame and matting
- **Axes** = The actual picture area where your data is displayed

### Visual Example

```python
# This creates a figure but doesn't show axes yet
fig = plt.figure()
plt.show()
```

**Output:** An empty figure (blank canvas)

<div style="width: 400px; height: 300px; border: 1px solid #ccc; background: white; margin: 10px 0;">
<p style="text-align: center; color: #666; margin-top: 140px;">Empty Figure Canvas</p>
</div>

```python
# This adds axes to the figure
fig = plt.figure()
ax = fig.add_axes([0.1, 0.1, 0.8, 0.8])  # [left, bottom, width, height]
plt.show()
```

**Output:** A figure with empty axes (coordinate system visible)

<div style="width: 400px; height: 300px; border: 1px solid #ccc; background: white; margin: 10px 0; position: relative;">
<div style="position: absolute; left: 40px; bottom: 40px; right: 40px; top: 40px; border-left: 2px solid black; border-bottom: 2px solid black;">
<div style="position: absolute; bottom: -20px; left: 50%; transform: translateX(-50%); font-size: 12px;">X-axis</div>
<div style="position: absolute; left: -35px; top: 50%; transform: rotate(-90deg) translateX(-50%); font-size: 12px;">Y-axis</div>
</div>
</div>

**Understanding the axes parameters:**
- `[0.1, 0.1, 0.8, 0.8]` means:
  - Start 10% from left edge (0.1)
  - Start 10% from bottom edge (0.1) 
  - Take up 80% of figure width (0.8)
  - Take up 80% of figure height (0.8)

---

## Functional vs Object-Oriented Approach

Matplotlib offers two main approaches. Let's understand both:

### 1. Functional Approach (Simple but Limited)

```python
# Functional approach - quick and easy
plt.plot(x, y)
plt.xlabel('X Label')
plt.ylabel('Y Label')
plt.title('My Plot Title')
plt.show()
```

**Output:** A simple line plot with labels

<div style="width: 500px; height: 400px; border: 1px solid #ccc; background: white; margin: 10px 0; position: relative;">
<div style="position: absolute; left: 60px; bottom: 60px; right: 40px; top: 60px; border-left: 2px solid black; border-bottom: 2px solid black;">
<svg width="100%" height="100%" style="position: absolute;">
<path d="M 0,100% L 10%,90% L 20%,80% L 30%,65% L 40%,48% L 50%,31% L 60%,16% L 70%,5% L 80%,0% L 90%,0% L 100%,0%" stroke="blue" stroke-width="2" fill="none"/>
</svg>
<div style="position: absolute; bottom: -40px; left: 50%; transform: translateX(-50%); font-size: 14px; font-weight: bold;">X Label</div>
<div style="position: absolute; left: -45px; top: 50%; transform: rotate(-90deg) translateX(-50%); font-size: 14px; font-weight: bold;">Y Label</div>
</div>
<div style="position: absolute; top: 10px; left: 50%; transform: translateX(-50%); font-size: 16px; font-weight: bold;">My Plot Title</div>
</div>

### 2. Object-Oriented Approach (Recommended)

```python
# Object-oriented approach - more control
fig = plt.figure()
ax = fig.add_axes([0.1, 0.1, 0.8, 0.8])

ax.plot(x, y)
ax.set_xlabel('X Label')
ax.set_ylabel('Y Label')
ax.set_title('My Plot Title')

plt.show()
```

**Output:** Same plot, but with explicit control over figure and axes

<div style="width: 500px; height: 400px; border: 1px solid #ccc; background: white; margin: 10px 0; position: relative;">
<div style="position: absolute; left: 60px; bottom: 60px; right: 40px; top: 60px; border-left: 2px solid black; border-bottom: 2px solid black;">
<svg width="100%" height="100%" style="position: absolute;">
<path d="M 0,100% L 10%,90% L 20%,80% L 30%,65% L 40%,48% L 50%,31% L 60%,16% L 70%,5% L 80%,0% L 90%,0% L 100%,0%" stroke="blue" stroke-width="2" fill="none"/>
</svg>
<div style="position: absolute; bottom: -40px; left: 50%; transform: translateX(-50%); font-size: 14px; font-weight: bold;">X Label</div>
<div style="position: absolute; left: -45px; top: 50%; transform: rotate(-90deg) translateX(-50%); font-size: 14px; font-weight: bold;">Y Label</div>
</div>
<div style="position: absolute; top: 10px; left: 50%; transform: translateX(-50%); font-size: 16px; font-weight: bold;">My Plot Title</div>
</div>

### Why Choose Object-Oriented?

The object-oriented approach is preferred because:
1. **Explicit control**: You know exactly which axes you're modifying
2. **Multiple plots**: Easy to manage multiple subplots
3. **Customization**: Fine-tune every aspect of your plot
4. **Professional code**: Industry standard approach

---

## Creating Basic Plots

### Simple Line Plot

```python
# Create figure and axes
fig = plt.figure(figsize=(8, 6))
ax = fig.add_axes([0.1, 0.1, 0.8, 0.8])

# Create the plot
ax.plot(x, y)

# Add labels and title
ax.set_xlabel('X values')
ax.set_ylabel('Y values (X squared)')
ax.set_title('Simple Line Plot: Y = X²')

plt.show()
```

**Output:** A parabolic curve showing y = x²

<div style="width: 600px; height: 450px; border: 1px solid #ccc; background: white; margin: 10px 0; position: relative;">
<div style="position: absolute; left: 80px; bottom: 80px; right: 40px; top: 60px; border-left: 2px solid black; border-bottom: 2px solid black;">
<svg width="100%" height="100%" style="position: absolute;">
<path d="M 0,100% L 9%,96% L 18%,84% L 27%,64% L 36%,36% L 45%,19% L 55%,9% L 64%,2.25% L 73%,0% L 82%,0% L 91%,0% L 100%,0%" stroke="#1f77b4" stroke-width="2" fill="none"/>
</svg>
<div style="position: absolute; bottom: -60px; left: 50%; transform: translateX(-50%); font-size: 14px;">X values</div>
<div style="position: absolute; left: -65px; top: 50%; transform: rotate(-90deg) translateX(-50%); font-size: 14px;">Y values (X squared)</div>
</div>
<div style="position: absolute; top: 15px; left: 50%; transform: translateX(-50%); font-size: 16px; font-weight: bold;">Simple Line Plot: Y = X²</div>
</div>

### Adding Multiple Lines

```python
# Create additional data
y2 = x ** 3  # y = x cubed

fig = plt.figure(figsize=(10, 6))
ax = fig.add_axes([0.1, 0.1, 0.8, 0.8])

# Plot both lines
ax.plot(x, y, label='X squared')
ax.plot(x, y2, label='X cubed')

# Customize
ax.set_xlabel('X values')
ax.set_ylabel('Y values')
ax.set_title('Multiple Lines on Same Plot')
ax.legend(loc='upper left')  # Add legend

plt.show()
```

**Output:** Two curves on the same plot with a legend

<div style="width: 700px; height: 450px; border: 1px solid #ccc; background: white; margin: 10px 0; position: relative;">
<div style="position: absolute; left: 80px; bottom: 80px; right: 40px; top: 60px; border-left: 2px solid black; border-bottom: 2px solid black;">
<svg width="100%" height="100%" style="position: absolute;">
<path d="M 0,100% L 9%,96% L 18%,84% L 27%,64% L 36%,36% L 45%,19% L 55%,9% L 64%,2.25% L 73%,0% L 82%,0% L 91%,0% L 100%,0%" stroke="#1f77b4" stroke-width="2" fill="none"/>
<path d="M 0,100% L 9%,99% L 18%,95% L 27%,85% L 36%,68% L 45%,45% L 55%,25% L 64%,10% L 73%,0% L 82%,0% L 91%,0% L 100%,0%" stroke="#ff7f0e" stroke-width="2" fill="none"/>
</svg>
<div style="position: absolute; top: 10px; left: 10px; background: white; border: 1px solid #ccc; padding: 8px; font-size: 12px;">
<div style="margin-bottom: 4px;"><span style="width: 20px; height: 2px; background: #1f77b4; display: inline-block; margin-right: 8px;"></span>X squared</div>
<div><span style="width: 20px; height: 2px; background: #ff7f0e; display: inline-block; margin-right: 8px;"></span>X cubed</div>
</div>
<div style="position: absolute; bottom: -60px; left: 50%; transform: translateX(-50%); font-size: 14px;">X values</div>
<div style="position: absolute; left: -65px; top: 50%; transform: rotate(-90deg) translateX(-50%); font-size: 14px;">Y values</div>
</div>
<div style="position: absolute; top: 15px; left: 50%; transform: translateX(-50%); font-size: 16px; font-weight: bold;">Multiple Lines on Same Plot</div>
</div>

### Understanding the Legend

The legend helps identify different lines:
- `label='X squared'` in the plot command creates legend entries
- `ax.legend()` displays the legend
- `loc='upper left'` positions the legend (options: 'upper left', 'center', 'best', etc.)

---

## Working with Subplots

Subplots allow multiple plots in one figure. There are several ways to create them:

### Method 1: Using subplot()

```python
# Create subplots using plt.subplots()
fig, axes = plt.subplots(nrows=1, ncols=2, figsize=(12, 5))

# Plot on first subplot
axes[0].plot(x, y, 'r')  # 'r' makes it red
axes[0].set_title('First Plot: Y = X²')
axes[0].set_xlabel('X')
axes[0].set_ylabel('Y')

# Plot on second subplot
axes[1].plot(x, y2, 'b')  # 'b' makes it blue
axes[1].set_title('Second Plot: Y = X³')
axes[1].set_xlabel('X')
axes[1].set_ylabel('Y')

# Adjust spacing to prevent overlap
plt.tight_layout()
plt.show()
```

**Output:** Two side-by-side plots

<div style="width: 800px; height: 350px; border: 1px solid #ccc; background: white; margin: 10px 0; position: relative;">
<!-- First subplot -->
<div style="position: absolute; left: 40px; bottom: 60px; width: 300px; height: 200px; border-left: 2px solid black; border-bottom: 2px solid black;">
<svg width="100%" height="100%" style="position: absolute;">
<path d="M 0,100% L 10%,96% L 20%,84% L 30%,64% L 40%,36% L 50%,19% L 60%,9% L 70%,2% L 80%,0% L 90%,0% L 100%,0%" stroke="red" stroke-width="2" fill="none"/>
</svg>
<div style="position: absolute; bottom: -40px; left: 50%; transform: translateX(-50%); font-size: 12px;">X</div>
<div style="position: absolute; left: -25px; top: 50%; transform: rotate(-90deg) translateX(-50%); font-size: 12px;">Y</div>
</div>
<div style="position: absolute; top: 20px; left: 190px; transform: translateX(-50%); font-size: 14px; font-weight: bold;">First Plot: Y = X²</div>

<!-- Second subplot -->
<div style="position: absolute; right: 40px; bottom: 60px; width: 300px; height: 200px; border-left: 2px solid black; border-bottom: 2px solid black;">
<svg width="100%" height="100%" style="position: absolute;">
<path d="M 0,100% L 10%,99% L 20%,95% L 30%,85% L 40%,68% L 50%,45% L 60%,25% L 70%,10% L 80%,0% L 90%,0% L 100%,0%" stroke="blue" stroke-width="2" fill="none"/>
</svg>
<div style="position: absolute; bottom: -40px; left: 50%; transform: translateX(-50%); font-size: 12px;">X</div>
<div style="position: absolute; left: -25px; top: 50%; transform: rotate(-90deg) translateX(-50%); font-size: 12px;">Y</div>
</div>
<div style="position: absolute; top: 20px; right: 190px; transform: translateX(50%); font-size: 14px; font-weight: bold;">Second Plot: Y = X³</div>
</div>

### Understanding Subplots Parameters

- `nrows=1, ncols=2`: Creates 1 row with 2 columns
- `axes[0]`, `axes[1]`: Access individual subplot axes
- `plt.tight_layout()`: Automatically adjusts spacing

### Method 2: Manual Axes Positioning

```python
fig = plt.figure(figsize=(10, 8))

# Create main plot
ax1 = fig.add_axes([0.1, 0.1, 0.8, 0.8])
ax1.plot(x, y)
ax1.set_title('Main Plot')

# Create inset plot
ax2 = fig.add_axes([0.2, 0.5, 0.3, 0.3])  # Smaller, positioned inside
ax2.plot(y, x)
ax2.set_title('Inset Plot')

plt.show()
```

**Output:** A main plot with a smaller plot inserted inside

<div style="width: 600px; height: 500px; border: 1px solid #ccc; background: white; margin: 10px 0; position: relative;">
<!-- Main plot -->
<div style="position: absolute; left: 60px; bottom: 60px; right: 60px; top: 80px; border-left: 2px solid black; border-bottom: 2px solid black;">
<svg width="100%" height="100%" style="position: absolute;">
<path d="M 0,100% L 10%,96% L 20%,84% L 30%,64% L 40%,36% L 50%,19% L 60%,9% L 70%,2% L 80%,0% L 90%,0% L 100%,0%" stroke="#1f77b4" stroke-width="2" fill="none"/>
</svg>

<!-- Inset plot -->
<div style="position: absolute; left: 60px; top: 60px; width: 150px; height: 120px; border-left: 1px solid black; border-bottom: 1px solid black; background: white;">
<svg width="100%" height="100%" style="position: absolute;">
<path d="M 0,100% L 4%,90% L 16%,80% L 36%,70% L 64%,60% L 100%,50%" stroke="#ff7f0e" stroke-width="1" fill="none"/>
</svg>
<div style="position: absolute; top: -20px; left: 50%; transform: translateX(-50%); font-size: 10px; font-weight: bold;">Inset Plot</div>
</div>
</div>
<div style="position: absolute; top: 20px; left: 50%; transform: translateX(-50%); font-size: 16px; font-weight: bold;">Main Plot</div>
</div>

### 2x2 Subplot Grid

```python
fig, axes = plt.subplots(nrows=2, ncols=2, figsize=(10, 8))

# Plot data in each subplot
axes[0,0].plot(x, y)
axes[0,0].set_title('Top Left: Y = X²')

axes[0,1].plot(x, y2)
axes[0,1].set_title('Top Right: Y = X³')

axes[1,0].plot(y, x)
axes[1,0].set_title('Bottom Left: X vs Y²')

axes[1,1].plot(y2, x)
axes[1,1].set_title('Bottom Right: X vs Y³')

plt.tight_layout()
plt.show()
```

**Output:** Four plots arranged in a 2x2 grid

<div style="width: 700px; height: 600px; border: 1px solid #ccc; background: white; margin: 10px 0; position: relative;">
<!-- Top Left -->
<div style="position: absolute; left: 40px; top: 40px; width: 250px; height: 200px; border-left: 1px solid black; border-bottom: 1px solid black;">
<svg width="100%" height="100%"><path d="M 0,100% L 20%,84% L 40%,36% L 60%,9% L 80%,0% L 100%,0%" stroke="blue" stroke-width="1.5" fill="none"/></svg>
<div style="position: absolute; top: -25px; left: 50%; transform: translateX(-50%); font-size: 12px; font-weight: bold;">Top Left: Y = X²</div>
</div>

<!-- Top Right -->
<div style="position: absolute; right: 40px; top: 40px; width: 250px; height: 200px; border-left: 1px solid black; border-bottom: 1px solid black;">
<svg width="100%" height="100%"><path d="M 0,100% L 20%,95% L 40%,68% L 60%,25% L 80%,0% L 100%,0%" stroke="orange" stroke-width="1.5" fill="none"/></svg>
<div style="position: absolute; top: -25px; left: 50%; transform: translateX(-50%); font-size: 12px; font-weight: bold;">Top Right: Y = X³</div>
</div>

<!-- Bottom Left -->
<div style="position: absolute; left: 40px; bottom: 40px; width: 250px; height: 200px; border-left: 1px solid black; border-bottom: 1px solid black;">
<svg width="100%" height="100%"><path d="M 0,100% L 4%,90% L 16%,80% L 36%,70% L 64%,60% L 100%,50%" stroke="green" stroke-width="1.5" fill="none"/></svg>
<div style="position: absolute; top: -25px; left: 50%; transform: translateX(-50%); font-size: 12px; font-weight: bold;">Bottom Left: X vs Y²</div>
</div>

<!-- Bottom Right -->
<div style="position: absolute; right: 40px; bottom: 40px; width: 250px; height: 200px; border-left: 1px solid black; border-bottom: 1px solid black;">
<svg width="100%" height="100%"><path d="M 0,100% L 1%,99% L 8%,95% L 27%,85% L 64%,68% L 100%,45%" stroke="red" stroke-width="1.5" fill="none"/></svg>
<div style="position: absolute; top: -25px; left: 50%; transform: translateX(-50%); font-size: 12px; font-weight: bold;">Bottom Right: X vs Y³</div>
</div>
</div>

---

## Customizing Your Plots

### Colors, Line Styles, and Markers

```python
fig, ax = plt.subplots(figsize=(10, 6))

# Different ways to specify colors and styles
ax.plot(x, y, color='red', linewidth=2, linestyle='--', label='Dashed Red')
ax.plot(x, y2, color='#1f77b4', linewidth=3, linestyle='-', label='Solid Blue')
ax.plot(x, x**1.5, 'go-', linewidth=1, markersize=8, label='Green with Markers')

ax.set_xlabel('X values')
ax.set_ylabel('Y values')
ax.set_title('Customized Plot Styles')
ax.legend()

plt.show()
```

**Output:** Multiple lines with different colors, styles, and markers

<div style="width: 700px; height: 450px; border: 1px solid #ccc; background: white; margin: 10px 0; position: relative;">
<div style="position: absolute; left: 80px; bottom: 80px; right: 40px; top: 60px; border-left: 2px solid black; border-bottom: 2px solid black;">
<svg width="100%" height="100%" style="position: absolute;">
<!-- Dashed Red Line -->
<path d="M 0,100% L 10%,96% L 20%,84% L 30%,64% L 40%,36% L 50%,19% L 60%,9% L 70%,2% L 80%,0% L 90%,0% L 100%,0%" stroke="red" stroke-width="2" stroke-dasharray="5,5" fill="none"/>
<!-- Solid Blue Line -->
<path d="M 0,100% L 10%,99% L 20%,95% L 30%,85% L 40%,68% L 50%,45% L 60%,25% L 70%,10% L 80%,0% L 90%,0% L 100%,0%" stroke="#1f77b4" stroke-width="3" fill="none"/>
<!-- Green Line with Markers -->
<path d="M 0,100% L 10%,97% L 20%,89% L 30%,76% L 40%,58% L 50%,35% L 60%,17% L 70%,5% L 80%,0% L 90%,0% L 100%,0%" stroke="green" stroke-width="1" fill="none"/>
<circle cx="0%" cy="100%" r="4" fill="green"/>
<circle cx="10%" cy="97%" r="4" fill="green"/>
<circle cx="20%" cy="89%" r="4" fill="green"/>
<circle cx="30%" cy="76%" r="4" fill="green"/>
<circle cx="40%" cy="58%" r="4" fill="green"/>
<circle cx="50%" cy="35%" r="4" fill="green"/>
<circle cx="60%" cy="17%" r="4" fill="green"/>
<circle cx="70%" cy="5%" r="4" fill="green"/>
<circle cx="80%" cy="0%" r="4" fill="green"/>
<circle cx="90%" cy="0%" r="4" fill="green"/>
<circle cx="100%" cy="0%" r="4" fill="green"/>
</svg>
<div style="position: absolute; top: 10px; right: 10px; background: white; border: 1px solid #ccc; padding: 8px; font-size: 12px;">
<div style="margin-bottom: 4px;"><span style="width: 20px; height: 2px; background: red; display: inline-block; margin-right: 8px; border-top: 2px dashed red;"></span>Dashed Red</div>
<div style="margin-bottom: 4px;"><span style="width: 20px; height: 3px; background: #1f77b4; display: inline-block; margin-right: 8px;"></span>Solid Blue</div>
<div><span style="color: green; margin-right: 8px;">●—</span>Green with Markers</div>
</div>
<div style="position: absolute; bottom: -60px; left: 50%; transform: translateX(-50%); font-size: 14px;">X values</div>
<div style="position: absolute; left: -65px; top: 50%; transform: rotate(-90deg) translateX(-50%); font-size: 14px;">Y values</div>
</div>
<div style="position: absolute; top: 15px; left: 50%; transform: translateX(-50%); font-size: 16px; font-weight: bold;">Customized Plot Styles</div>
</div>

**Color Options:**
- Named colors: 'red', 'blue', 'green', 'orange', 'purple'
- Hex codes: '#FF0000' (red), '#0000FF' (blue)
- RGB tuples: (1, 0, 0) for red

**Line Style Options:**
- `'-'`: Solid line (default)
- `'--'`: Dashed line
- `'-.'`: Dash-dot line
- `':'`: Dotted line

**Marker Options:**
- `'o'`: Circles
- `'s'`: Squares
- `'^'`: Triangles
- `'*'`: Stars
- `'+'`: Plus signs

### Setting Plot Limits

```python
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 5))

# Default limits
ax1.plot(x, y)
ax1.set_title('Default Limits')

# Custom limits
ax2.plot(x, y)
ax2.set_xlim(1, 4)      # X-axis from 1 to 4
ax2.set_ylim(0, 15)     # Y-axis from 0 to 15
ax2.set_title('Custom Limits (Zoomed)')

plt.tight_layout()
plt.show()
```

**Output:** Two plots showing the effect of custom axis limits

<div style="width: 800px; height: 350px; border: 1px solid #ccc; background: white; margin: 10px 0; position: relative;">
<!-- First subplot - Default limits -->
<div style="position: absolute; left: 40px; bottom: 60px; width: 300px; height: 200px; border-left: 2px solid black; border-bottom: 2px solid black;">
<svg width="100%" height="100%" style="position: absolute;">
<path d="M 0,100% L 10%,96% L 20%,84% L 30%,64% L 40%,36% L 50%,19% L 60%,9% L 70%,2% L 80%,0% L 90%,0% L 100%,0%" stroke="#1f77b4" stroke-width="2" fill="none"/>
</svg>
<div style="position: absolute; bottom: -40px; left: 50%; transform: translateX(-50%); font-size: 12px;">0 → 5</div>
<div style="position: absolute; left: -35px; top: 50%; transform: rotate(-90deg) translateX(-50%); font-size: 12px;">0 → 25</div>
</div>
<div style="position: absolute; top: 20px; left: 190px; transform: translateX(-50%); font-size: 14px; font-weight: bold;">Default Limits</div>

<!-- Second subplot - Custom limits (zoomed) -->
<div style="position: absolute; right: 40px; bottom: 60px; width: 300px; height: 200px; border-left: 2px solid black; border-bottom: 2px solid black;">
<svg width="100%" height="100%" style="position: absolute;">
<!-- Zoomed portion of parabola from x=1 to x=4 -->
<path d="M 0,7% L 33%,27% L 67%,60% L 100%,100%" stroke="#1f77b4" stroke-width="2" fill="none"/>
</svg>
<div style="position: absolute; bottom: -40px; left: 50%; transform: translateX(-50%); font-size: 12px;">1 → 4</div>
<div style="position: absolute; left: -35px; top: 50%; transform: rotate(-90deg) translateX(-50%); font-size: 12px;">0 → 15</div>
</div>
<div style="position: absolute; top: 20px; right: 190px; transform: translateX(50%); font-size: 14px; font-weight: bold;">Custom Limits (Zoomed)</div>
</div>

---

## Saving Figures

### Basic Saving

```python
fig, ax = plt.subplots(figsize=(10, 6))
ax.plot(x, y)
ax.set_title('Plot to Save')

# Save the figure
fig.savefig('my_plot.png')
plt.show()
```

### Advanced Saving Options

```python
fig, ax = plt.subplots(figsize=(10, 6))
ax.plot(x, y, linewidth=2)
ax.set_title('High Quality Plot')

# Save with high DPI and tight layout
fig.savefig('high_quality_plot.png', 
            dpi=300,                    # High resolution
            bbox_inches='tight',        # Remove extra whitespace
            facecolor='white',          # Background color
            edgecolor='none')           # No border

plt.show()
```

**File Format Options:**
- `.png`: Good for web, supports transparency
- `.pdf`: Vector format, perfect for publications
- `.svg`: Vector format, web-friendly
- `.jpg`: Compressed format, smaller file size

**Important Parameters:**
- `dpi`: Dots per inch (higher = better quality, larger file)
- `bbox_inches='tight'`: Removes extra whitespace
- `facecolor`: Background color of the figure

---

## Complete Example: Professional Plot

Let's create a professional-looking plot that combines everything we've learned:

```python
# Create sample data
x = np.linspace(0, 10, 100)
y1 = np.sin(x)
y2 = np.cos(x)
y3 = np.sin(x) * np.exp(-x/10)

# Create figure with specific size and DPI
fig, ax = plt.subplots(figsize=(12, 8), dpi=100)

# Plot multiple lines with different styles
ax.plot(x, y1, 'b-', linewidth=2, label='sin(x)', alpha=0.8)
ax.plot(x, y2, 'r--', linewidth=2, label='cos(x)', alpha=0.8)
ax.plot(x, y3, 'g:', linewidth=3, label='sin(x)⋅exp(-x/10)', alpha=0.8)

# Customize the plot
ax.set_xlabel('X values (radians)', fontsize=14, fontweight='bold')
ax.set_ylabel('Y values', fontsize=14, fontweight='bold')
ax.set_title('Trigonometric Functions Comparison', fontsize=16, fontweight='bold', pad=20)

# Add grid
ax.grid(True, alpha=0.3, linestyle='-', linewidth=0.5)

# Customize legend
ax.legend(loc='upper right', fontsize=12, framealpha=0.9, shadow=True)

# Set axis limits
ax.set_xlim(0, 10)
ax.set_ylim(-1.2, 1.2)

# Add some styling
ax.spines['top'].set_visible(False)    # Remove top border
ax.spines['right'].set_visible(False)  # Remove right border

# Adjust layout and save
plt.tight_layout()
fig.savefig('professional_plot.png', dpi=300, bbox_inches='tight')
plt.show()
```

**Output:** A professional-looking plot with multiple trigonometric functions

<div style="width: 800px; height: 500px; border: 1px solid #ccc; background: white; margin: 10px 0; position: relative;">
<div style="position: absolute; left: 80px; bottom: 80px; right: 40px; top: 80px; border-left: 2px solid black; border-bottom: 2px solid black;">
<!-- Grid -->
<svg width="100%" height="100%" style="position: absolute;">
<defs>
<pattern id="grid" width="10%" height="10%" patternUnits="userSpaceOnUse">
<path d="M 10% 0 L 0 0 0 10%" fill="none" stroke="#e0e0e0" stroke-width="0.5"/>
</pattern>
</defs>
<rect width="100%" height="100%" fill="url(#grid)"/>
<!-- Sin wave -->
<path d="M 0,50% Q 12.5%,20% 25%,50% T 50%,50% T 75%,50% T 100%,50%" stroke="blue" stroke-width="2" fill="none" opacity="0.8"/>
<!-- Cos wave -->
<path d="M 0,20% Q 12.5%,50% 25%,80% T 50%,20% T 75%,80% T 100%,50%" stroke="red" stroke-width="2" stroke-dasharray="8,4" fill="none" opacity="0.8"/>
<!-- Decaying sin wave -->
<path d="M 0,50% Q 12.5%,25% 25%,55% Q 37.5%,60% 50%,52% Q 62.5%,48% 75%,49% Q 87.5%,51% 100%,50%" stroke="green" stroke-width="3" stroke-dasharray="2,3" fill="none" opacity="0.8"/>
</svg>

<!-- Legend -->
<div style="position: absolute; top: 10px; right: 10px; background: rgba(255,255,255,0.9); border: 1px solid #ccc; padding: 10px; font-size: 12px; box-shadow: 2px 2px 4px rgba(0,0,0,0.1);">
<div style="margin-bottom: 6px;"><span style="width: 25px; height: 2px; background: blue; display: inline-block; margin-right: 8px;"></span>sin(x)</div>
<div style="margin-bottom: 6px;"><span style="width: 25px; height: 2px; background: red; display: inline-block; margin-right: 8px; border-top: 2px dashed red;"></span>cos(x)</div>
<div><span style="width: 25px; height: 3px; background: green; display: inline-block; margin-right: 8px; border-top: 3px dotted green;"></span>sin(x)⋅exp(-x/10)</div>
</div>

<div style="position: absolute; bottom: -60px; left: 50%; transform: translateX(-50%); font-size: 14px; font-weight: bold;">X values (radians)</div>
<div style="position: absolute; left: -65px; top: 50%; transform: rotate(-90deg) translateX(-50%); font-size: 14px; font-weight: bold;">Y values</div>
</div>
<div style="position: absolute; top: 15px; left: 50%; transform: translateX(-50%); font-size: 16px; font-weight: bold;">Trigonometric Functions Comparison</div>
</div>

---

## Best Practices and Tips

### 1. Always Use Object-Oriented Approach
```python
# Good ✓
fig, ax = plt.subplots()
ax.plot(x, y)
ax.set_title('My Plot')

# Avoid for complex plots ✗
plt.plot(x, y)
plt.title('My Plot')
```

### 2. Set Figure Size Early
```python
# Set appropriate figure size
fig, ax = plt.subplots(figsize=(10, 6))  # Width, height in inches
```

### 3. Use Descriptive Labels and Titles
```python
ax.set_xlabel('Time (seconds)', fontsize=12)
ax.set_ylabel('Temperature (°C)', fontsize=12)
ax.set_title('Temperature Change Over Time', fontsize=14, fontweight='bold')
```

### 4. Include Units in Labels
```python
ax.set_xlabel('Distance (km)')
ax.set_ylabel('Speed (km/h)')
```

### 5. Use Colors Meaningfully
```python
# Use colors that make sense
ax.plot(time, temperature, 'r-', label='Temperature')  # Red for heat
ax.plot(time, humidity, 'b-', label='Humidity')        # Blue for water
```

### 6. Always Use tight_layout()
```python
plt.tight_layout()  # Prevents overlapping labels
```

### 7. Save High-Quality Figures
```python
fig.savefig('plot.png', dpi=300, bbox_inches='tight')
```

---

## Common Beginner Mistakes and Solutions

### Mistake 1: Confusion Between Figure and Axes
```python
# Wrong ✗
fig = plt.figure()
fig.plot(x, y)  # Figure doesn't have plot method!

# Correct ✓
fig, ax = plt.subplots()
ax.plot(x, y)   # Axes has plot method
```

**Visual Explanation:**

<div style="width: 600px; height: 200px; border: 1px solid #ccc; background: white; margin: 10px 0; position: relative;">
<div style="position: absolute; left: 20px; top: 20px; width: 250px; height: 160px; border: 3px solid red; background: #fff5f5;">
<div style="position: absolute; top: -25px; left: 50%; transform: translateX(-50%); font-size: 14px; font-weight: bold; color: red;">Figure (Canvas)</div>
<div style="position: absolute; left: 20px; top: 20px; right: 20px; bottom: 20px; border: 2px solid blue; background: #f0f8ff;">
<div style="position: absolute; top: -20px; left: 50%; transform: translateX(-50%); font-size: 12px; font-weight: bold; color: blue;">Axes (Plot Area)</div>
<div style="position: absolute; bottom: 5px; left: 5px; font-size: 10px;">This is where plot() works!</div>
</div>
</div>
<div style="position: absolute; right: 50px; top: 50%; transform: translateY(-50%); font-size: 14px;">
<div>✓ ax.plot(x, y)</div>
<div style="color: red;">✗ fig.plot(x, y)</div>
</div>
</div>

### Mistake 2: Overlapping Subplots
```python
# Problem: Overlapping labels
fig, axes = plt.subplots(2, 2)
# ... plotting code ...
plt.show()  # Labels might overlap

# Solution: Use tight_layout
fig, axes = plt.subplots(2, 2)
# ... plotting code ...
plt.tight_layout()
plt.show()
```

**Before and After:**

<div style="display: flex; gap: 20px; margin: 20px 0;">
<!-- Before (overlapping) -->
<div style="width: 300px; height: 200px; border: 1px solid #ccc; background: white; position: relative;">
<div style="position: absolute; top: 5px; left: 50%; transform: translateX(-50%); font-size: 12px; font-weight: bold; color: red;">Before (Overlapping)</div>
<div style="position: absolute; left: 10px; top: 30px; width: 130px; height: 70px; border: 1px solid black; font-size: 8px;">Plot 1<br/>Title overlaps</div>
<div style="position: absolute; right: 10px; top: 30px; width: 130px; height: 70px; border: 1px solid black; font-size: 8px;">Plot 2<br/>Title overlaps</div>
<div style="position: absolute; left: 10px; bottom: 30px; width: 130px; height: 70px; border: 1px solid black; font-size: 8px;">Plot 3<br/>Labels overlap</div>
<div style="position: absolute; right: 10px; bottom: 30px; width: 130px; height: 70px; border: 1px solid black; font-size: 8px;">Plot 4<br/>Labels overlap</div>
</div>

<!-- After (proper spacing) -->
<div style="width: 300px; height: 200px; border: 1px solid #ccc; background: white; position: relative;">
<div style="position: absolute; top: 5px; left: 50%; transform: translateX(-50%); font-size: 12px; font-weight: bold; color: green;">After (tight_layout)</div>
<div style="position: absolute; left: 20px; top: 40px; width: 100px; height: 55px; border: 1px solid black; font-size: 8px;">Plot 1</div>
<div style="position: absolute; right: 20px; top: 40px; width: 100px; height: 55px; border: 1px solid black; font-size: 8px;">Plot 2</div>
<div style="position: absolute; left: 20px; bottom: 40px; width: 100px; height: 55px; border: 1px solid black; font-size: 8px;">Plot 3</div>
<div style="position: absolute; right: 20px; bottom: 40px; width: 100px; height: 55px; border: 1px solid black; font-size: 8px;">Plot 4</div>
</div>
</div>

### Mistake 3: Not Saving Before plt.show()
```python
# Wrong ✗
plt.show()
fig.savefig('plot.png')  # Might save blank figure

# Correct ✓
fig.savefig('plot.png')
plt.show()
```

---

## Interactive Examples to Try

### Example 1: Create Your First Plot
```python
import matplotlib.pyplot as plt
import numpy as np

# Create data
x = np.linspace(0, 10, 50)
y = np.sin(x)

# Create plot
fig, ax = plt.subplots(figsize=(8, 6))
ax.plot(x, y)
ax.set_xlabel('X values')
ax.set_ylabel('sin(X)')
ax.set_title('My First Matplotlib Plot')
plt.tight_layout()
plt.show()
```

### Example 2: Compare Different Functions
```python
# Create data
x = np.linspace(0, 5, 100)
y1 = x
y2 = x ** 2
y3 = x ** 3

# Create subplot
fig, ax = plt.subplots(figsize=(10, 6))

# Plot multiple lines
ax.plot(x, y1, 'b-', label='Linear (x)', linewidth=2)
ax.plot(x, y2, 'r--', label='Quadratic (x²)', linewidth=2)
ax.plot(x, y3, 'g:', label='Cubic (x³)', linewidth=2)

# Customize
ax.set_xlabel('X values', fontsize=12)
ax.set_ylabel('Y values', fontsize=12)
ax.set_title('Comparison of Power Functions', fontsize=14, fontweight='bold')
ax.legend()
ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

### Example 3: Create a 2x2 Dashboard
```python
# Create data
x = np.linspace(0, 10, 100)
y1 = np.sin(x)
y2 = np.cos(x)
y3 = np.tan(x)
y4 = np.exp(-x/5) * np.sin(x)

# Create 2x2 subplots
fig, axes = plt.subplots(2, 2, figsize=(12, 10))

# Plot in each subplot
axes[0,0].plot(x, y1, 'b-')
axes[0,0].set_title('sin(x)')
axes[0,0].grid(True)

axes[0,1].plot(x, y2, 'r-')
axes[0,1].set_title('cos(x)')
axes[0,1].grid(True)

axes[1,0].plot(x, y3, 'g-')
axes[1,0].set_title('tan(x)')
axes[1,0].set_ylim(-5, 5)  # Limit y-axis for tan
axes[1,0].grid(True)

axes[1,1].plot(x, y4, 'm-')
axes[1,1].set_title('Damped sin(x)')
axes[1,1].grid(True)

plt.tight_layout()
plt.show()
```

---

## Summary

In this tutorial, you learned:

1. **What Matplotlib is** and why it's important
2. **Installation** and basic setup
3. **Figure vs Axes** concepts (the foundation of Matplotlib)
4. **Two approaches**: Functional vs Object-Oriented
5. **Basic plotting** with customization
6. **Subplots** for multiple plots
7. **Extensive customization** options
8. **Saving figures** professionally
9. **Best practices** for clean, professional plots

### Key Takeaways:
- **Use the object-oriented approach** for better control
- **Understand the Figure-Axes hierarchy** 
- **Always include labels, titles, and legends**
- **Use `plt.tight_layout()`** to prevent overlapping
- **Save with high DPI** for publication-quality figures

### The Essential Matplotlib Workflow:
```python
# 1. Import libraries
import matplotlib.pyplot as plt
import numpy as np

# 2. Create data
x = np.linspace(0, 10, 100)
y = np.sin(x)

# 3. Create figure and axes
fig, ax = plt.subplots(figsize=(10, 6))

# 4. Plot data
ax.plot(x, y, label='sin(x)')

# 5. Customize
ax.set_xlabel('X values')
ax.set_ylabel('Y values')
ax.set_title('My Plot')
ax.legend()
ax.grid(True)

# 6. Save and show
plt.tight_layout()
fig.savefig('my_plot.png', dpi=300, bbox_inches='tight')
plt.show()
```

### Next Steps:
- Practice with different plot types (scatter, bar, histogram)
- Explore the Matplotlib gallery for inspiration
- Learn about Seaborn for statistical plots
- Study advanced features like animations and 3D plots

Remember: The best way to learn Matplotlib is by doing. Start with simple plots and gradually add complexity as you become more comfortable with the concepts!

---

## Quick Reference Card

### Essential Commands:
```python
# Setup
import matplotlib.pyplot as plt
import numpy as np

# Create figure and axes
fig, ax = plt.subplots(figsize=(width, height))

# Basic plotting
ax.plot(x, y)                    # Line plot
ax.scatter(x, y)                 # Scatter plot
ax.bar(x, y)                     # Bar plot

# Labels and titles
ax.set_xlabel('X Label')
ax.set_ylabel('Y Label')
ax.set_title('Title')

# Customization
ax.legend()                      # Add legend
ax.grid(True)                    # Add grid
ax.set_xlim(min, max)           # Set x limits
ax.set_ylim(min, max)           # Set y limits

# Save and display
plt.tight_layout()               # Fix spacing
fig.savefig('filename.png')      # Save figure
plt.show()                       # Display figure
```

This completes your comprehensive Matplotlib tutorial with visual outputs and step-by-step guidance for beginners!

*Happy plotting!*
