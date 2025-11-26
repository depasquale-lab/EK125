# Week 9B Pre-read: Visualizing Data and Plotting in Python

## Learning Objectives
By the end of this reading, you should understand:
- Why data visualization matters in programming and data analysis
- How to create basic plots using matplotlib
- How to customize plots with labels, titles, and legends
- How to create multiple types of plots (line, scatter, bar, histogram)
- How to work with subplots to show multiple visualizations together
- Best practices for creating clear, informative visualizations

## 1. Why Visualize Data?

Raw numbers tell only part of the story. Consider these two datasets:

**Dataset A**: `[23, 25, 27, 29, 31, 33]`  
**Dataset B**: `[23, 23, 23, 33, 33, 33]`

Both have the same average (28), but they tell very different stories. Visualization makes patterns, trends, and outliers immediately apparent in ways that tables of numbers cannot.

Programming allows you to create visualizations automatically, which is essential when:
- You have too much data to examine manually
- You need to update visualizations as data changes
- You want to compare multiple datasets or scenarios
- You need reproducible, publication-quality figures

## 2. Introduction to matplotlib

Matplotlib is Python's most widely-used plotting library. It provides both simple commands for quick plots and detailed control for publication-quality figures.

### Importing matplotlib
```python
# Import at the start of your script
import matplotlib.pyplot as plt
```

The convention is to import matplotlib's pyplot module as `plt`. You'll see this pattern everywhere in Python data visualization code. In Google Colab, matplotlib is already available and ready to use.

## 3. Creating Your First Plot

### Basic Line Plot
```python
import matplotlib.pyplot as plt

# Data
xValues = [1, 2, 3, 4, 5]
yValues = [2, 4, 6, 8, 10]

# Create plot
plt.plot(xValues, yValues)
plt.show()
```

This creates a simple line plot connecting the points (1,2), (2,4), (3,6), (4,8), and (5,10).

### Adding Labels and Titles
Good plots always include context:

```python
import matplotlib.pyplot as plt

xValues = [1, 2, 3, 4, 5]
yValues = [2, 4, 6, 8, 10]

plt.plot(xValues, yValues)
plt.xlabel('Time (seconds)')
plt.ylabel('Distance (meters)')
plt.title('Object Position Over Time')
plt.show()
```

**Critical principle**: Every axis needs a label with units. Every plot needs a title that describes what is being shown.

## 4. Customizing Plot Appearance

### Line Styles and Colors
```python
import matplotlib.pyplot as plt

time = [0, 1, 2, 3, 4]
temperature = [20, 22, 21, 23, 25]

# Customize line appearance
plt.plot(time, temperature, color='red', linestyle='--', linewidth=2, marker='o')
plt.xlabel('Time (hours)')
plt.ylabel('Temperature (°C)')
plt.title('Temperature Variation')
plt.show()
```

Common options:
- **Colors**: `'red'`, `'blue'`, `'green'`, `'black'`, or hex codes like `'#FF5733'`
- **Line styles**: `'-'` (solid), `'--'` (dashed), `':'` (dotted), `'-.'` (dash-dot)
- **Markers**: `'o'` (circle), `'s'` (square), `'^'` (triangle), `'*'` (star)

### Grid Lines
```python
plt.plot(xValues, yValues)
plt.xlabel('X axis')
plt.ylabel('Y axis')
plt.title('Plot with Grid')
plt.grid(True)
plt.show()
```

Grid lines help readers extract quantitative values from your plots.

## 5. Different Plot Types

### Scatter Plots
Use scatter plots when you want to show individual data points without connecting them:

```python
import matplotlib.pyplot as plt

heights = [160, 165, 170, 175, 180, 185]
weights = [60, 65, 70, 75, 80, 85]

plt.scatter(heights, weights, color='blue', s=100)
plt.xlabel('Height (cm)')
plt.ylabel('Weight (kg)')
plt.title('Height vs Weight Relationship')
plt.show()
```

The `s` parameter controls the size of the markers.

### Bar Charts
Use bar charts for categorical data or discrete comparisons:

```python
import matplotlib.pyplot as plt

students = ['Alice', 'Bob', 'Charlie', 'Diana']
scores = [85, 92, 78, 95]

plt.bar(students, scores, color='green')
plt.xlabel('Student')
plt.ylabel('Test Score')
plt.title('Test Scores by Student')
plt.show()
```

### Histograms
Use histograms to show the distribution of a dataset:

```python
import matplotlib.pyplot as plt

# Simulated test scores
testScores = [78, 82, 85, 88, 90, 92, 85, 87, 83, 89, 
              91, 86, 84, 88, 90, 87, 85, 82, 79, 94]

plt.hist(testScores, bins=5, color='purple', edgecolor='black')
plt.xlabel('Score Range')
plt.ylabel('Number of Students')
plt.title('Distribution of Test Scores')
plt.show()
```

The `bins` parameter controls how many ranges to divide the data into.

### Radar Charts
Use radar charts (also called spider charts) to compare multiple variables for one or more subjects:

```python
import matplotlib.pyplot as plt
import numpy as np

# Skills assessment data
categories = ['Math', 'Physics', 'Programming', 'Writing', 'Communication']
values = [85, 90, 78, 82, 88]

# Number of variables
numVars = len(categories)

# Compute angle for each axis
angles = np.linspace(0, 2 * np.pi, numVars, endpoint=False).tolist()

# Complete the circle by appending the first value
values += values[:1]
angles += angles[:1]

# Create the plot
fig, ax = plt.subplots(figsize=(6, 6), subplot_kw=dict(projection='polar'))
ax.plot(angles, values, 'o-', linewidth=2, color='blue')
ax.fill(angles, values, alpha=0.25, color='blue')

# Set category labels
ax.set_xticks(angles[:-1])
ax.set_xticklabels(categories)

# Set value range
ax.set_ylim(0, 100)
ax.set_yticks([20, 40, 60, 80, 100])
ax.set_yticklabels(['20', '40', '60', '80', '100'])

ax.set_title('Student Skills Assessment', pad=20)
plt.show()
```

Radar charts are particularly useful when you want to show performance across multiple dimensions on the same scale. They work best with 3-8 variables.

### Comparing Multiple Subjects on a Radar Chart
```python
import matplotlib.pyplot as plt
import numpy as np

categories = ['Math', 'Physics', 'Programming', 'Writing', 'Communication']
student1Values = [85, 90, 78, 82, 88]
student2Values = [75, 85, 95, 78, 80]

numVars = len(categories)
angles = np.linspace(0, 2 * np.pi, numVars, endpoint=False).tolist()

# Complete the circle
student1Values += student1Values[:1]
student2Values += student2Values[:1]
angles += angles[:1]

fig, ax = plt.subplots(figsize=(6, 6), subplot_kw=dict(projection='polar'))

# Plot both students
ax.plot(angles, student1Values, 'o-', linewidth=2, color='blue', label='Student 1')
ax.fill(angles, student1Values, alpha=0.15, color='blue')
ax.plot(angles, student2Values, 's-', linewidth=2, color='red', label='Student 2')
ax.fill(angles, student2Values, alpha=0.15, color='red')

ax.set_xticks(angles[:-1])
ax.set_xticklabels(categories)
ax.set_ylim(0, 100)
ax.set_yticks([20, 40, 60, 80, 100])
ax.set_yticklabels(['20', '40', '60', '80', '100'])

ax.set_title('Skills Comparison: Two Students', pad=20)
ax.legend(loc='upper right', bbox_to_anchor=(1.3, 1.1))
plt.show()
```

### Geographic Heatmaps
Use geographic heatmaps to visualize spatial data on real maps. The `folium` library integrates OpenStreetMap with Python and includes heatmap capabilities:

```python
import folium
from folium.plugins import HeatMap

# Create base map centered on Boston
bostonMap = folium.Map(location=[42.3601, -71.0589], zoom_start=13)

# Sample data: locations of coffee shops with popularity scores
# Format: [latitude, longitude, intensity]
coffeeShopData = [
    [42.3601, -71.0589, 0.8],
    [42.3611, -71.0599, 0.6],
    [42.3621, -71.0609, 0.9],
    [42.3595, -71.0575, 0.7],
    [42.3615, -71.0585, 0.5],
    [42.3605, -71.0595, 0.8],
    [42.3625, -71.0605, 0.4],
]

# Add heatmap layer
HeatMap(coffeeShopData, radius=15, blur=25, max_zoom=1).add_to(bostonMap)

# Display map (works in Colab and Jupyter notebooks)
bostonMap
```

The intensity value (third element in each list) is optional. If omitted, all points are weighted equally:

```python
import folium
from folium.plugins import HeatMap

# Map centered on Boston University campus
buMap = folium.Map(location=[42.3505, -71.1054], zoom_start=15)

# Student study locations (without intensity weighting)
studyLocations = [
    [42.3505, -71.1054],  # Mugar Library
    [42.3498, -71.1065],  # GSU Student center
    [42.3512, -71.1048],  # Academic building
    [42.3495, -71.1070],  # Coffee shop on Comm Ave
    [42.3508, -71.1045],  # Engineering building
]

# Add heatmap
HeatMap(studyLocations, radius=20).add_to(buMap)

buMap
```

Parameters for `HeatMap()`:
- `radius`: Size of each data point's influence (default 25)
- `blur`: Amount of blur applied (default 15)
- `max_zoom`: Maximum zoom level where heatmap is visible (default 18)
- `min_opacity`: Minimum opacity of the heatmap (default 0.0)
- `max_opacity`: Maximum opacity of the heatmap (default 1.0)

Geographic heatmaps are particularly useful for:
- Visualizing crime statistics by location
- Showing population density
- Mapping sensor data (temperature, pollution, etc.)
- Analyzing customer locations or business foot traffic
- Displaying event attendance patterns

### Parallel Coordinates Plots
Use parallel coordinates plots to visualize multivariate data, where each variable gets its own vertical axis and each data point is represented as a line connecting its values across all axes. This is excellent for comparing multiple subjects across several dimensions:

```python
import matplotlib.pyplot as plt
import pandas as pd
from pandas.plotting import parallel_coordinates

# Student performance data
studentData = {
    'Student': ['Alice', 'Bob', 'Charlie', 'Diana', 'Eve', 'Frank', 'Grace'],
    'Math': [85, 78, 92, 88, 95, 72, 90],
    'Physics': [90, 82, 88, 85, 91, 75, 87],
    'Programming': [88, 95, 85, 90, 87, 88, 92],
    'Writing': [82, 76, 90, 92, 85, 80, 88],
    'Communication': [87, 80, 85, 95, 90, 78, 91]
}

df = pd.DataFrame(studentData)

# Create parallel coordinates plot
plt.figure(figsize=(12, 6))
parallel_coordinates(df, 'Student', colormap='viridis', linewidth=2)
plt.title('Student Performance Across Subjects')
plt.ylabel('Score')
plt.xlabel('Subject')
plt.legend(bbox_to_anchor=(1.05, 1), loc='upper left')
plt.grid(axis='y', alpha=0.3)
plt.tight_layout()
plt.show()
```

This plot makes it easy to identify patterns:
- Students with consistently high or low performance appear as relatively flat lines near the top or bottom
- Students with varying performance show lines that cross many others
- Correlations between subjects become visible (if lines tend to move together)

**Note**: Matplotlib's parallel coordinates implementation has limitations, particularly when trying to use student names as a proper categorical axis. For more sophisticated parallel coordinates plots with better interactivity (hovering to highlight lines, filtering by dragging on axes), see the Plotly example in the Appendix, which handles this visualization much better.

Key features of parallel coordinates plots:
- Each vertical axis represents one variable (subject)
- Each line represents one observation (student)
- Lines that cluster together indicate similar patterns
- Crossing lines indicate different patterns across variables
- Color coding helps distinguish groups or individual cases

Parallel coordinates are especially valuable when:
- Comparing multiple subjects across many dimensions (5+ variables)
- Looking for patterns or clusters in multivariate data
- Identifying outliers that behave differently across dimensions
- Comparing groups while preserving individual variation

## 6. Multiple Lines on One Plot

Often you need to compare multiple datasets:

```python
import matplotlib.pyplot as plt

time = [0, 1, 2, 3, 4, 5]
object1Position = [0, 2, 4, 6, 8, 10]
object2Position = [0, 3, 6, 9, 12, 15]

plt.plot(time, object1Position, label='Object 1', color='blue', marker='o')
plt.plot(time, object2Position, label='Object 2', color='red', marker='s')
plt.xlabel('Time (seconds)')
plt.ylabel('Position (meters)')
plt.title('Position of Two Objects Over Time')
plt.legend()
plt.show()
```

The `legend()` function creates a legend box showing which line corresponds to which dataset. Always use legends when plotting multiple series.

## 7. Subplots: Multiple Plots in One Figure

Sometimes you need to show several related plots side by side:

```python
import matplotlib.pyplot as plt

time = [0, 1, 2, 3, 4, 5]
position = [0, 2, 4, 6, 8, 10]
velocity = [2, 2, 2, 2, 2, 2]

# Create a figure with 2 subplots arranged in 1 row, 2 columns
plt.figure(figsize=(10, 4))

# First subplot
plt.subplot(1, 2, 1)
plt.plot(time, position, 'b-o')
plt.xlabel('Time (s)')
plt.ylabel('Position (m)')
plt.title('Position vs Time')

# Second subplot
plt.subplot(1, 2, 2)
plt.plot(time, velocity, 'r-s')
plt.xlabel('Time (s)')
plt.ylabel('Velocity (m/s)')
plt.title('Velocity vs Time')

plt.tight_layout()  # Prevents overlapping labels
plt.show()
```

The `subplot(rows, columns, plotNumber)` function specifies the layout. For example, `subplot(1, 2, 1)` creates a layout with 1 row and 2 columns, and selects the first subplot.

## 8. Saving Figures

To save a plot instead of displaying it:

```python
import matplotlib.pyplot as plt

xValues = [1, 2, 3, 4, 5]
yValues = [2, 4, 6, 8, 10]

plt.plot(xValues, yValues)
plt.xlabel('X axis')
plt.ylabel('Y axis')
plt.title('My Plot')

# Save the figure
plt.savefig('myPlot.png', dpi=300, bbox_inches='tight')
plt.show()
```

Parameters:
- `dpi=300`: Sets resolution (300 is good for publications)
- `bbox_inches='tight'`: Removes excess white space around the plot

Common formats: `.png`, `.pdf`, `.jpg`, `.svg`

## 9. Working with NumPy Arrays

While you can plot lists, matplotlib works seamlessly with NumPy arrays, which you'll use increasingly as datasets grow:

```python
import matplotlib.pyplot as plt
import numpy as np

# Create evenly-spaced data
xValues = np.linspace(0, 10, 100)  # 100 points from 0 to 10
yValues = np.sin(xValues)          # Calculate sine for each x

plt.plot(xValues, yValues)
plt.xlabel('x')
plt.ylabel('sin(x)')
plt.title('Sine Function')
plt.grid(True)
plt.show()
```

This creates smooth curves because we use 100 points instead of just a few.

## 10. Working with Pandas DataFrames

As datasets grow larger and more complex, you'll often work with data stored in Pandas DataFrames. Matplotlib integrates seamlessly with Pandas, making it easy to visualize tabular data.

### Basic DataFrame Plotting

```python
import matplotlib.pyplot as plt
import pandas as pd

# Create a simple DataFrame
data = {
    'Month': ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
    'Sales': [150, 200, 175, 220, 240, 260],
    'Expenses': [100, 120, 110, 130, 140, 150]
}
df = pd.DataFrame(data)

# Plot directly from DataFrame
plt.plot(df['Month'], df['Sales'], marker='o', label='Sales')
plt.plot(df['Month'], df['Expenses'], marker='s', label='Expenses')
plt.xlabel('Month')
plt.ylabel('Amount ($1000s)')
plt.title('Monthly Sales and Expenses')
plt.legend()
plt.grid(True)
plt.show()
```

### Using DataFrame's Built-in Plot Method

Pandas DataFrames have a convenient `.plot()` method that simplifies many common plotting tasks:

```python
import matplotlib.pyplot as plt
import pandas as pd

data = {
    'Month': ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
    'ProductA': [150, 200, 175, 220, 240, 260],
    'ProductB': [180, 190, 200, 210, 230, 250],
    'ProductC': [120, 140, 130, 160, 170, 180]
}
df = pd.DataFrame(data)

# Set Month as the index for better x-axis labels
df.set_index('Month', inplace=True)

# Create line plot
df.plot(kind='line', marker='o', figsize=(10, 6))
plt.xlabel('Month')
plt.ylabel('Sales ($1000s)')
plt.title('Product Sales by Month')
plt.grid(True)
plt.legend(title='Product')
plt.show()
```

### Different Plot Types with DataFrames

```python
import matplotlib.pyplot as plt
import pandas as pd

# Student grades data
grades = {
    'Student': ['Alice', 'Bob', 'Charlie', 'Diana', 'Eve'],
    'Math': [85, 78, 92, 88, 95],
    'Physics': [90, 82, 88, 85, 91],
    'Programming': [88, 95, 85, 90, 87]
}
df = pd.DataFrame(grades)
df.set_index('Student', inplace=True)

# Create bar chart
df.plot(kind='bar', figsize=(10, 6))
plt.xlabel('Student')
plt.ylabel('Score')
plt.title('Student Grades by Subject')
plt.legend(title='Subject')
plt.xticks(rotation=0)
plt.grid(axis='y')
plt.show()
```

### Scatter Plots from DataFrames

```python
import matplotlib.pyplot as plt
import pandas as pd

# Study time vs test score data
studyData = {
    'HoursStudied': [2, 3, 4, 5, 6, 7, 8, 9, 10],
    'TestScore': [65, 70, 75, 78, 82, 85, 88, 90, 92],
    'HoursSlept': [6, 7, 7, 8, 7, 8, 8, 9, 8]
}
df = pd.DataFrame(studyData)

# Create scatter plot
plt.scatter(df['HoursStudied'], df['TestScore'], s=df['HoursSlept']*20, alpha=0.6)
plt.xlabel('Hours Studied')
plt.ylabel('Test Score')
plt.title('Test Score vs Study Time\n(bubble size shows hours of sleep)')
plt.grid(True)
plt.show()
```

### Plotting Multiple Subplots from a DataFrame

```python
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np

# Generate sample time series data
dates = pd.date_range('2024-01-01', periods=30, freq='D')
data = {
    'Date': dates,
    'Temperature': 20 + 5 * np.sin(np.linspace(0, 2*np.pi, 30)) + np.random.normal(0, 1, 30),
    'Humidity': 60 + 10 * np.cos(np.linspace(0, 2*np.pi, 30)) + np.random.normal(0, 2, 30),
    'Pressure': 1013 + 5 * np.sin(np.linspace(0, 4*np.pi, 30)) + np.random.normal(0, 1, 30)
}
df = pd.DataFrame(data)
df.set_index('Date', inplace=True)

# Create subplots
fig, axes = plt.subplots(3, 1, figsize=(10, 10))

df['Temperature'].plot(ax=axes[0], color='red', title='Temperature Over Time')
axes[0].set_ylabel('Temperature (°C)')
axes[0].grid(True)

df['Humidity'].plot(ax=axes[1], color='blue', title='Humidity Over Time')
axes[1].set_ylabel('Humidity (%)')
axes[1].grid(True)

df['Pressure'].plot(ax=axes[2], color='green', title='Pressure Over Time')
axes[2].set_ylabel('Pressure (hPa)')
axes[2].grid(True)

plt.tight_layout()
plt.show()
```

### Grouped Bar Charts from DataFrames

```python
import matplotlib.pyplot as plt
import pandas as pd

# Quarterly sales data
salesData = {
    'Quarter': ['Q1', 'Q2', 'Q3', 'Q4'],
    'Region_A': [120, 150, 140, 180],
    'Region_B': [100, 130, 145, 160],
    'Region_C': [90, 110, 120, 140]
}
df = pd.DataFrame(salesData)
df.set_index('Quarter', inplace=True)

# Create grouped bar chart
df.plot(kind='bar', figsize=(10, 6))
plt.xlabel('Quarter')
plt.ylabel('Sales ($1000s)')
plt.title('Regional Sales by Quarter')
plt.legend(title='Region')
plt.xticks(rotation=0)
plt.grid(axis='y')
plt.show()
```

### Key Points About Pandas Integration

1. **Direct column access**: Use `df['columnName']` to extract data for plotting
2. **Built-in plot method**: `df.plot()` provides quick access to common plot types
3. **Index as x-axis**: Setting a DataFrame index makes it the default x-axis for plots
4. **Multiple columns automatically**: Plotting a DataFrame plots all numeric columns
5. **Convenient for time series**: Pandas handles datetime indices elegantly

## 11. Common Mistakes to Avoid

### Missing Labels or Units
```python
# BAD: No labels or units
plt.plot(time, distance)
plt.show()

# GOOD: Clear labels with units
plt.plot(time, distance)
plt.xlabel('Time (seconds)')
plt.ylabel('Distance (meters)')
plt.title('Distance Traveled Over Time')
plt.show()
```

### Too Many Colors or Styles
```python
# BAD: Confusing and hard to read
plt.plot(x1, y1, color='red', linestyle='--', marker='o')
plt.plot(x2, y2, color='blue', linestyle=':', marker='s')
plt.plot(x3, y3, color='green', linestyle='-.', marker='^')
plt.plot(x4, y4, color='purple', linestyle='-', marker='*')

# GOOD: Consistent style, vary one aspect
plt.plot(x1, y1, 'o-', label='Series 1')
plt.plot(x2, y2, 's-', label='Series 2')
plt.plot(x3, y3, '^-', label='Series 3')
plt.legend()
```

### Forgetting to Call show()
```python
# BAD: Plot won't display
plt.plot(xValues, yValues)

# GOOD: Plot displays
plt.plot(xValues, yValues)
plt.show()
```

## 12. Best Practices Summary

1. **Always label axes** with descriptive names and units
2. **Always include a title** that describes what the plot shows
3. **Use legends** when plotting multiple series
4. **Keep it simple**: Don't use too many colors or styles
5. **Choose the right plot type** for your data:
   - Line plots for continuous data and trends over time
   - Scatter plots for relationships between two variables
   - Bar charts for categorical comparisons
   - Histograms for distributions
6. **Use grids** when readers need to extract quantitative values
7. **Save figures** at high resolution (300 dpi) for reports and papers

## 13. A Complete Example

Here's a complete example incorporating multiple concepts:

```python
import matplotlib.pyplot as plt
import numpy as np

# Generate sample data
time = np.linspace(0, 10, 50)
experimentalData = 2.0 * time + np.random.normal(0, 1, 50)
theoreticalData = 2.0 * time

# Create figure with subplots
plt.figure(figsize=(12, 5))

# Left subplot: Comparison of experimental and theoretical
plt.subplot(1, 2, 1)
plt.scatter(time, experimentalData, label='Experimental', color='blue', alpha=0.6)
plt.plot(time, theoreticalData, label='Theoretical', color='red', linewidth=2)
plt.xlabel('Time (seconds)')
plt.ylabel('Distance (meters)')
plt.title('Experimental vs Theoretical Results')
plt.legend()
plt.grid(True)

# Right subplot: Residuals (difference between experimental and theoretical)
plt.subplot(1, 2, 2)
residuals = experimentalData - theoreticalData
plt.scatter(time, residuals, color='green', alpha=0.6)
plt.axhline(y=0, color='black', linestyle='--', linewidth=1)
plt.xlabel('Time (seconds)')
plt.ylabel('Residual (meters)')
plt.title('Residuals (Experimental - Theoretical)')
plt.grid(True)

plt.tight_layout()
plt.savefig('experimentAnalysis.png', dpi=300, bbox_inches='tight')
plt.show()
```

## Questions to Consider

1. Why might you choose a scatter plot over a line plot?
2. What makes a visualization "good" versus "bad"?
3. How does visualization help you identify errors in your data or code?
4. When would you use subplots instead of separate figures?

## Key Terms to Remember

- **matplotlib**: Python's primary plotting library
- **plt.plot()**: Creates a line plot
- **plt.scatter()**: Creates a scatter plot
- **plt.bar()**: Creates a bar chart
- **plt.hist()**: Creates a histogram
- **plt.xlabel()**, **plt.ylabel()**: Add axis labels
- **plt.title()**: Adds a title to the plot
- **plt.legend()**: Creates a legend for multiple data series
- **plt.subplot()**: Creates multiple plots in one figure
- **plt.savefig()**: Saves the plot to a file
- **plt.show()**: Displays the plot on screen
- **dpi**: Dots per inch, controls image resolution

## Appendix: Advanced Plotting Libraries in Colab

While matplotlib is the foundation of Python plotting and what you'll use most in this course, Google Colab provides access to several more advanced plotting libraries. These libraries offer features like interactivity, 3D visualization, and more sophisticated default styling. You're not required to use these for coursework, but they're worth knowing about for your own projects.

### Plotly: Interactive Plots

Plotly creates interactive plots that allow zooming, panning, hovering for data values, and toggling series on and off. This is particularly valuable for exploratory data analysis and presentations.

#### Basic Plotly Line Plot
```python
import plotly.graph_objects as go

# Data
time = [0, 1, 2, 3, 4, 5]
distance = [0, 2, 4, 6, 8, 10]

# Create figure
fig = go.Figure()
fig.add_trace(go.Scatter(x=time, y=distance, mode='lines+markers', name='Position'))

fig.update_layout(
    title='Interactive Position vs Time',
    xaxis_title='Time (seconds)',
    yaxis_title='Distance (meters)',
    hovermode='x unified'
)

fig.show()
```

#### Multiple Series with Plotly
```python
import plotly.graph_objects as go

time = [0, 1, 2, 3, 4, 5]
object1 = [0, 2, 4, 6, 8, 10]
object2 = [0, 3, 6, 9, 12, 15]

fig = go.Figure()

fig.add_trace(go.Scatter(x=time, y=object1, mode='lines+markers', 
                         name='Object 1', line=dict(color='blue', width=2)))
fig.add_trace(go.Scatter(x=time, y=object2, mode='lines+markers', 
                         name='Object 2', line=dict(color='red', width=2)))

fig.update_layout(
    title='Two Objects in Motion',
    xaxis_title='Time (seconds)',
    yaxis_title='Position (meters)',
    hovermode='x unified',
    template='plotly_white'
)

fig.show()
```

#### Plotly 3D Surface Plot
```python
import plotly.graph_objects as go
import numpy as np

# Create mesh grid
x = np.linspace(-5, 5, 50)
y = np.linspace(-5, 5, 50)
xGrid, yGrid = np.meshgrid(x, y)

# Calculate z values (a 3D surface)
zGrid = np.sin(np.sqrt(xGrid**2 + yGrid**2))

fig = go.Figure(data=[go.Surface(x=xGrid, y=yGrid, z=zGrid, colorscale='Viridis')])

fig.update_layout(
    title='3D Surface Plot',
    scene=dict(
        xaxis_title='X',
        yaxis_title='Y',
        zaxis_title='Z'
    ),
    autosize=False,
    width=800,
    height=800
)

fig.show()
```

#### Plotly with Pandas DataFrames
```python
import plotly.express as px
import pandas as pd

# Sample data
data = {
    'Month': ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
    'Sales': [150, 200, 175, 220, 240, 260],
    'Expenses': [100, 120, 110, 130, 140, 150]
}
df = pd.DataFrame(data)

# Create interactive line plot
fig = px.line(df, x='Month', y=['Sales', 'Expenses'], 
              title='Monthly Sales and Expenses',
              labels={'value': 'Amount ($1000s)', 'variable': 'Category'})

fig.show()
```

#### Interactive Parallel Coordinates Plot
Plotly excels at parallel coordinates plots with full interactivity. Hovering over a line highlights it and shows all values:

```python
import plotly.graph_objects as go
import pandas as pd

# Student performance data
studentData = {
    'Student': ['Alice', 'Bob', 'Charlie', 'Diana', 'Eve', 'Frank', 'Grace'],
    'Math': [85, 78, 92, 88, 95, 72, 90],
    'Physics': [90, 82, 88, 85, 91, 75, 87],
    'Programming': [88, 95, 85, 90, 87, 88, 92],
    'Writing': [82, 76, 90, 92, 85, 80, 88],
    'Communication': [87, 80, 85, 95, 90, 78, 91]
}

df = pd.DataFrame(studentData)

# Create color mapping for each student
colors = list(range(len(df)))

# Create parallel coordinates plot
fig = go.Figure(data=
    go.Parcoords(
        line = dict(
            color = colors,
            colorscale = 'Viridis',
            showscale = False
        ),
        dimensions = [
            dict(
                label = 'Student', 
                values = df.index,
                tickvals = list(range(len(df))),
                ticktext = df['Student'].tolist(),
                constraintrange = None
            ),
            dict(label = 'Math', values = df['Math'], range = [0, 100]),
            dict(label = 'Physics', values = df['Physics'], range = [0, 100]),
            dict(label = 'Programming', values = df['Programming'], range = [0, 100]),
            dict(label = 'Writing', values = df['Writing'], range = [0, 100]),
            dict(label = 'Communication', values = df['Communication'], range = [0, 100])
        ],
        labelfont = dict(size = 12),
        tickfont = dict(size = 10),
        rangefont = dict(size = 12),
        unselected = dict(line = dict(color = 'lightgray', opacity = 0.3))
    )
)

fig.update_layout(
    title='Interactive, click and drag on any axis to filter (you'll see a crosshair))',
    font=dict(size=12),
    width=1000,
    height=600,
    margin=dict(l=100, r=100, t=80, b=100)
)

# Move axis labels to bottom
fig.update_traces(
    dimensions=[
        dict(
            label = 'Student', 
            values = df.index,
            tickvals = list(range(len(df))),
            ticktext = df['Student'].tolist()
        ),
        dict(label = 'Math', values = df['Math'], range = [0, 100]),
        dict(label = 'Physics', values = df['Physics'], range = [0, 100]),
        dict(label = 'Programming', values = df['Programming'], range = [0, 100]),
        dict(label = 'Writing', values = df['Writing'], range = [0, 100]),
        dict(label = 'Communication', values = df['Communication'], range = [0, 100])
    ],
    labelfont=dict(size=12),
    tickfont=dict(size=10),
    rangefont=dict(size=12)
)

fig.show()
```

This interactive plot allows you to:
- Click and drag on any axis to filter the data range (selected lines stay colored, others fade to gray)
- See student names evenly distributed on the leftmost axis
- Visualize all student scores across subjects simultaneously
- Filter by score ranges to identify students meeting certain criteria

Note: Plotly's parallel coordinates plot uses brush selection (click and drag on axes) rather than hover highlighting. This is actually more useful as it allows you to filter and explore the data interactively by selecting ranges on any axis.

### Seaborn: Statistical Visualization

Seaborn builds on matplotlib but provides more sophisticated statistical visualizations with better default aesthetics. It's particularly good for statistical relationships and distributions.

#### Seaborn Distribution Plot
```python
import seaborn as sns
import matplotlib.pyplot as plt
import numpy as np

# Generate sample data
testScores = np.random.normal(75, 10, 100)

# Create distribution plot
plt.figure(figsize=(10, 6))
sns.histplot(testScores, kde=True, bins=15)
plt.xlabel('Test Score')
plt.ylabel('Frequency')
plt.title('Distribution of Test Scores with Density Curve')
plt.show()
```

#### Seaborn Correlation Heatmap
```python
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np

# Create correlated data
data = {
    'StudyHours': np.random.uniform(1, 10, 50),
    'AttendanceRate': np.random.uniform(60, 100, 50),
    'SleepHours': np.random.uniform(5, 9, 50),
    'TestScore': np.random.uniform(60, 95, 50)
}
df = pd.DataFrame(data)

# Calculate correlation matrix
correlation = df.corr()

# Create heatmap
plt.figure(figsize=(8, 6))
sns.heatmap(correlation, annot=True, cmap='coolwarm', center=0, 
            square=True, linewidths=1)
plt.title('Correlation Between Study Factors and Test Scores')
plt.tight_layout()
plt.show()
```

#### Seaborn Pair Plot
```python
import seaborn as sns
import pandas as pd
import numpy as np

# Sample student data
np.random.seed(42)
data = {
    'Math': np.random.normal(80, 10, 50),
    'Physics': np.random.normal(75, 12, 50),
    'Programming': np.random.normal(85, 8, 50),
    'Year': np.random.choice(['Junior', 'Senior'], 50)
}
df = pd.DataFrame(data)

# Create pair plot
sns.pairplot(df, hue='Year', diag_kind='kde', height=2.5)
plt.suptitle('Relationships Between Subject Scores', y=1.02)
plt.show()
```

### Altair: Declarative Visualization

Altair uses a declarative approach where you describe what you want to visualize rather than how to draw it. It's excellent for creating complex visualizations with minimal code.

```python
import altair as alt
import pandas as pd

# Sample data
data = {
    'Month': ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'] * 2,
    'Amount': [150, 200, 175, 220, 240, 260, 100, 120, 110, 130, 140, 150],
    'Category': ['Sales']*6 + ['Expenses']*6
}
df = pd.DataFrame(data)

# Create interactive chart
chart = alt.Chart(df).mark_line(point=True).encode(
    x='Month:O',
    y='Amount:Q',
    color='Category:N',
    tooltip=['Month', 'Category', 'Amount']
).properties(
    width=600,
    height=400,
    title='Monthly Sales and Expenses'
).interactive()

chart
```

### Bokeh: Interactive Web-Ready Plots

Bokeh is designed for creating interactive visualizations for web browsers. It's particularly good for building dashboards and applications.

```python
from bokeh.plotting import figure, show, output_notebook
from bokeh.models import HoverTool

# Enable output to notebook
output_notebook()

# Data
x = [1, 2, 3, 4, 5]
y = [2, 4, 6, 8, 10]

# Create figure with tools
p = figure(title='Interactive Plot with Bokeh',
           x_axis_label='Time (seconds)',
           y_axis_label='Distance (meters)',
           width=600, height=400)

# Add line and scatter glyphs
p.line(x, y, legend_label='Position', line_width=2, color='blue')
p.scatter(x, y, size=10, color='red', alpha=0.5)

# Add hover tool
hover = HoverTool(tooltips=[('Time', '@x'), ('Distance', '@y')])
p.add_tools(hover)

show(p)
```

### Comparison: When to Use Each Library

| Library | Best For | Key Strength | Learning Curve |
|---------|----------|--------------|----------------|
| **matplotlib** | General-purpose plotting, publication figures | Complete control, reproducibility | Medium |
| **Plotly** | Interactive exploration, presentations | Built-in interactivity, 3D plots | Low-Medium |
| **Seaborn** | Statistical analysis, distributions | Statistical visualizations, attractive defaults | Low |
| **Altair** | Quick exploration, declarative style | Concise syntax, grammar of graphics | Low |
| **Bokeh** | Web applications, dashboards | Browser-based interactivity | Medium-High |

### Why Start with matplotlib?

Despite these alternatives, matplotlib remains the foundation because:

1. **Ubiquity**: Nearly all Python plotting libraries build on or integrate with matplotlib
2. **Control**: Provides precise control over every aspect of a figure
3. **Reproducibility**: Creates identical outputs every time (important for scientific work)
4. **Publication quality**: Industry standard for academic and professional publications
5. **Stability**: Mature library with extensive documentation and community support

The advanced libraries are tools to add to your toolkit once you understand the fundamentals. Many professional data scientists use matplotlib for final figures but Plotly or Seaborn for exploration.

### Getting Started with These Libraries

All these libraries are pre-installed in Google Colab. To use any of them:

```python
# Plotly
import plotly.express as px
import plotly.graph_objects as go

# Seaborn
import seaborn as sns

# Altair
import altair as alt

# Bokeh
from bokeh.plotting import figure, show, output_notebook
```

For your coursework, focus on mastering matplotlib first. Once you're comfortable with the fundamentals, experiment with these libraries for personal projects or when their specific features (like interactivity) provide clear advantages for your analysis.

