## Introduction to Geographical Plotting with Plotly

Geographical plotting is a powerful way to visualize data on a map, revealing patterns and insights that might be hidden in tables. Due to the diverse formats of geographical data, it can be quite challenging. For this tutorial, we'll focus on **Plotly**, a Python library known for its interactive and visually appealing plots. While Matplotlib also offers geographical plotting via its Basemap extension, Plotly's interactive features make it an excellent choice for creating dynamic visualizations.

In this tutorial, we will learn how to create **choropleth maps**. These maps use different shades, patterns, or colors to represent a particular data variable in different regions (e.g., states, countries). We'll cover both national (U.S.) and global-scale maps.

-----

### 1\. Getting Started: Basic U.S. Choropleth Map

To begin, we'll create a simple choropleth map of the United States. This example will help you understand the core components of a Plotly geographical plot.

#### 1.1 Imports and Setup

First, we need to import the necessary libraries and configure Plotly to work offline in a Jupyter Notebook.

```python
import pandas as pd
import plotly.graph_objs as go
from plotly.offline import init_notebook_mode, iplot

# Initialize Plotly for offline use in a Jupyter Notebook
init_notebook_mode(connected=True) 
```

  - `pandas` is used for data manipulation, particularly for reading CSV files.
  - `plotly.graph_objs as go` is the main module for creating plots. `go` is a common alias for this.
  - `plotly.offline` contains functions for plotting directly within a notebook without an internet connection. `init_notebook_mode(connected=True)` ensures the plots are rendered correctly.
  - `iplot` is the function we'll use to display our plots within the notebook.

#### 1.2 The Core Components: `data` and `layout`

Every Plotly geographical plot is built on two main dictionary-like objects: `data` and `layout`.

  - **`data`**: This dictionary specifies the actual data to be plotted on the map. It defines the type of plot, locations, values, colors, and hover text.
  - **`layout`**: This dictionary controls the map's overall appearance, such as the title, scope (e.g., USA, world), and geographical projections.

#### 1.3 Creating a Simple U.S. Map

Let's build a basic choropleth map for three U.S. states: Arizona, California, and New York.

```python
# 1. Create the data object
data = dict(
    type='choropleth',
    locations=['AZ', 'CA', 'NY'],
    locationmode='USA-states',
    colorscale='Portland',
    text=['Text 1', 'Text 2', 'Text 3'],
    z=[1, 2, 3],
    colorbar={'title': 'Color Bar Title Goes Here'}
)

# 2. Create the layout object
layout = dict(geo={'scope': 'usa'})

# 3. Create the plot and display it
choromap = go.Figure(data=[data], layout=layout)
iplot(choromap)
```

**Code Breakdown:**

  - **`data` Dictionary:**

      - `type`: Set to `'choropleth'` to create a choropleth map.
      - `locations`: A list of state abbreviations (`'AZ'`, `'CA'`, `'NY'`). Plotly uses these codes to identify the regions.
      - `locationmode`: Specifies the geographical scope. `'USA-states'` tells Plotly to interpret the locations as U.S. state abbreviations.
      - `colorscale`: Defines the color scheme for the map. `'Portland'` is a predefined color palette. Other options include `'Jet'`, `'Greens'`, `'Greys'`, etc.
      - `text`: A list of strings that will appear when you hover over each state. This list must be in the same order as the `locations` list.
      - `z`: This list of numerical values corresponds to the `locations`. Plotly uses these values to determine the color of each state based on the `colorscale`.
      - `colorbar`: A dictionary for customizing the color bar, with a `'title'` key to set its label.

  - **`layout` Dictionary:**

      - `geo`: A nested dictionary for geographical settings.
      - `scope`: Set to `'usa'` to display a map of the United States.

  - **Plotting:**

      - `go.Figure(data=[data], layout=layout)`: This creates a `Figure` object. It's crucial to pass the `data` dictionary inside a **list** (`[data]`).
      - `iplot(choromap)`: Renders the figure in the notebook.

**Output:**

-----

### 2\. Plotting with Real-World Data: U.S. Agricultural Exports

Now, let's apply these concepts to a real dataset. We will use a CSV file containing U.S. agricultural export data from 2011.

#### 2.1 Loading the Data

First, we'll read the data into a Pandas DataFrame.

```python
import pandas as pd
import plotly.graph_objs as go
from plotly.offline import init_notebook_mode, iplot

# Initialize Plotly for offline use
init_notebook_mode(connected=True)

# Read the CSV file into a DataFrame
df = pd.read_csv('2011_us_agri_exports.csv')

# Display the first few rows of the DataFrame
print(df.head())
```

**DataFrame Snapshot:**

```
  code           state  category  total exports      beef      pork    poultry    dairy
0   AL         Alabama  agr_exp          15.63  0.030580  0.024564   0.009941  0.001920
1   AK          Alaska  agr_exp           0.08  0.000000  0.000000   0.000000  0.000000
2   AZ         Arizona  agr_exp          11.69  0.000000  0.000000   0.000000  0.000000
3   AR         Arkansas  agr_exp          35.86  0.000000  0.000000   0.000000  0.000000
4   CA      California  agr_exp         203.20  0.000000  0.000000   0.000000  0.000000
...
```

#### 2.2 Building the Plot

We'll use the `code` column for locations, `total exports` for the color values (`z`), and the `state` column for the hover text. We'll also customize the map's appearance.

```python
# Create the data dictionary
data = dict(
    type='choropleth',
    locations=df['code'],  # Use the 'code' column for locations
    z=df['total exports'], # Use 'total exports' for the color values
    locationmode='USA-states',
    text=df['state'],     # Use the 'state' column for hover text
    colorscale='YIOrRd',  # A yellow-orange-red color scheme
    marker=dict(
        line=dict(
            color='rgb(12,12,12)',
            width=1
        )
    ),
    colorbar=dict(title='Millions USD')
)

# Create the layout dictionary
layout = dict(
    title='2011 U.S. Agricultural Exports by State',
    geo=dict(
        scope='usa',
        showlakes=True,
        lakecolor='rgb(85,173,240)'
    )
)

# Create the figure and plot
choromap2 = go.Figure(data=[data], layout=layout)
iplot(choromap2)
```

**Additional Arguments Explained:**

  - **`colorscale`**: We've changed the color scale to `'YIOrRd'`. Plotly offers many predefined color scales.
  - **`marker`**: A nested dictionary to customize the appearance of state borders.
      - `line`: A dictionary for the border line.
          - `color`: Sets the border color using an RGB string (e.g., `'rgb(12,12,12)'` for a dark grey).
          - `width`: Sets the border width.
  - **`layout`**:
      - `title`: Sets the title of the entire plot.
      - `geo` (nested):
          - `showlakes=True`: Displays lakes on the map.
          - `lakecolor='rgb(85,173,240)'`: Sets the color of the lakes to a specific shade of blue.

**Output:**

This plot provides a great visual overview, showing states with high exports (like California and Texas) in darker shades of red.

-----

### 3\. Creating Global Choropleth Maps

The process for creating a world map is very similar to a U.S. map, with a few key differences in the `locations` and `layout` dictionaries.

#### 3.1 Loading World GDP Data

Let's load a dataset containing global GDP information from 2014.

```python
import pandas as pd
import plotly.graph_objs as go
from plotly.offline import init_notebook_mode, iplot

# Initialize Plotly for offline use
init_notebook_mode(connected=True)

# Read the CSV file for world GDP data
df_world = pd.read_csv('2014_World_GDP.csv')

# Display the first few rows
print(df_world.head())
```

**DataFrame Snapshot:**

```
  COUNTRY     GDP (BILLIONS)  CODE
0 Afghanistan           20.65  AFG
1      Albania           13.40  ALB
2      Algeria          227.80  DZA
3       Angola          131.40  AGO
4    Argentina          536.20  ARG
...
```

#### 3.2 Building the World Map

We will use the `CODE` column for locations and the `GDP (BILLIONS)` for the color values.

```python
# Create the data dictionary for world GDP
data_world = dict(
    type='choropleth',
    locations=df_world['CODE'], # Use the country codes for locations
    z=df_world['GDP (BILLIONS)'],
    text=df_world['COUNTRY'],
    colorbar={'title': 'GDP in Billions USD'}
)

# Create the layout dictionary
layout_world = dict(
    title='2014 Global GDP',
    geo=dict(
        showframe=False, # Hide the map frame
        projection={'type': 'mercator'} # Set the map projection type
    )
)

# Create the figure and plot
choromap_world = go.Figure(data=[data_world], layout=layout_world)
iplot(choromap_world)
```

**Differences for a World Map:**

  - **`locations`**: We use standardized **three-letter country codes** (e.g., `'USA'`, `'CHN'`) instead of U.S. state abbreviations. Plotly automatically recognizes these.
  - **`locationmode`**: We don't need to specify a `locationmode` because Plotly defaults to world countries if not specified.
  - **`layout`**:
      - `showframe=False`: This hides the frame around the map.
      - `projection={'type': 'mercator'}`: Sets the map's projection. Plotly supports many different projections (e.g., `'natural earth'`, `'stereographic'`). You can explore these to change the map's visual appearance.

**Output:**

This plot instantly shows that countries with the largest economies, such as the U.S. and China, are highlighted in a darker shade.

-----

### 4\. Conclusion and Further Exploration

While the syntax for Plotly's choropleth maps can seem complex at first due to nested dictionaries, the core logic is straightforward:

1.  **Define your `data`**: What information goes on the map?
2.  **Define your `layout`**: How should the map look?
3.  **Combine and Plot**: Use `go.Figure()` and `iplot()` to render the visualization.

The key to mastering Plotly is to **reference the documentation and examples**. It's not about memorizing the syntax, but rather knowing how to find the right arguments for your specific needs. With just a few lines of code, Plotly allows you to create impressive, interactive geographical plots that effectively communicate complex data.
