# Week 10b: From Python to MATLAB: Syntax and Plotting

## Introduction

Welcome to MATLAB! As you transition from Python, you'll find that many programming concepts you've already learned translate directly. However, MATLAB has some important syntax differences and was specifically designed for matrix operations and numerical computing. This guide will help you bridge your Python knowledge to MATLAB, with a focus on plotting.

## Installing MATLAB at Boston University

Boston University has a site-wide license giving all BU students, faculty, and staff unlimited access to MATLAB, Simulink, and 49 add-on products at no additional cost. Here's how to install it on your personal computer:

### Installation Steps

1. **Create a MathWorks Account**
   - Go to the MathWorks BU Portal and create an account using your BU email address
   - You'll need to authenticate with your BU credentials

2. **Download MATLAB**
   - Log into the MathWorks BU Portal
   - Select the "40523728 Individual Academic – Total Headcount" license
   - Download the installer for your operating system (Windows, Mac, or Linux)

3. **Install the Software**
   - Run the installer you downloaded
   - Follow the on-screen installation prompts
   - Choose which toolboxes you want to install (it's not a bad idea to just install them all)

4. **Activate Your License**
   - After installation, MATLAB will prompt you to activate
   - Select "Activate automatically using the Internet (recommended)"
   - Sign in with your MathWorks account when prompted
   - The activation process will link MATLAB to BU's license

**Important Notes:**
- You can install MATLAB on each computer you personally use (home and office machines)
- The license requires annual renewal (typically in July), but you'll receive notifications
- If you have any installation issues, contact BU IT Help at ithelp@bu.edu

**Resources:**
- Full installation guide: https://www.bu.edu/tech/services/cccs/desktop/distribution/mathsci/matlab/
- FAQ: https://www.bu.edu/tech/services/cccs/desktop/distribution/mathsci/matlab/faqs/

---

## Key Syntax Differences

### Semicolons and Output
In Python, statements automatically suppress output unless you explicitly print. In MATLAB, it's the opposite:

**Python:**
```python
x = 5          # No output
print(x)       # Displays: 5
```

**MATLAB:**
```matlab
x = 5          % Displays: x = 5
x = 5;         % Suppresses output (semicolon!)
disp(x)        % Displays: 5
```

**Key point:** Use semicolons in MATLAB to suppress output, especially in loops!

### Comments
- **Python:** `# This is a comment`
- **MATLAB:** `% This is a comment`

### Indexing
This is one of the most important differences:

**Python:** Zero-indexed (starts at 0)
```python
my_list = [10, 20, 30, 40]
print(my_list[0])      # 10 (first element)
print(my_list[-1])     # 40 (last element)
```

**MATLAB:** One-indexed (starts at 1)
```matlab
my_array = [10, 20, 30, 40];
disp(my_array(1))      % 10 (first element)
disp(my_array(end))    % 40 (last element)
```

**Watch out:** MATLAB uses parentheses `()` for indexing, not square brackets!

### Arrays vs Lists
MATLAB is built around arrays (especially matrices). Creating them is straightforward:

**Python:**
```python
my_list = [1, 2, 3, 4]
import numpy as np
my_array = np.array([1, 2, 3, 4])
```

**MATLAB:**
```matlab
my_array = [1, 2, 3, 4];              % Row vector
my_column = [1; 2; 3; 4];             % Column vector (semicolon!)
my_matrix = [1, 2, 3; 4, 5, 6];       % 2x3 matrix
```

### Ranges and Sequences

**Python:**
```python
range(0, 10)           # 0 to 9
range(0, 10, 2)        # 0, 2, 4, 6, 8
```

**MATLAB:**
```matlab
0:9                    % 0 to 9
0:2:9                  % 0, 2, 4, 6, 8 (start:step:end)
linspace(0, 10, 5)     % 5 evenly spaced points from 0 to 10
```

### Logical Operators

| Operation | Python | MATLAB |
|-----------|--------|--------|
| AND       | `and` or `&` | `&&` or `&` |
| OR        | `or` or `\|` | `\|\|` or `\|` |
| NOT       | `not` or `~` | `~` |

For single comparisons, use `&&` and `||` in MATLAB. For element-wise operations on arrays, use `&` and `|`.

---

## Plotting in MATLAB

If you've used matplotlib in Python, MATLAB's plotting will feel familiar—in fact, matplotlib was inspired by MATLAB's plotting syntax!

### Basic Line Plot

**Python (matplotlib):**
```python
import matplotlib.pyplot as plt
x = [1, 2, 3, 4, 5]
y = [1, 4, 9, 16, 25]
plt.plot(x, y)
plt.xlabel('X values')
plt.ylabel('Y values')
plt.title('My Plot')
plt.show()
```

**MATLAB:**
```matlab
x = [1, 2, 3, 4, 5];
y = [1, 4, 9, 16, 25];
plot(x, y)
xlabel('X values')
ylabel('Y values')
title('My Plot')
```

**Key difference:** No need to import anything or call `show()`—MATLAB automatically displays the plot!

### Customizing Plots

You can customize line style, color, and markers:

```matlab
x = 0:0.1:2*pi;
y = sin(x);
plot(x, y, 'r--', 'LineWidth', 2)  % Red dashed line, thickness 2
xlabel('x')
ylabel('sin(x)')
title('Sine Wave')
grid on                             % Add grid lines
```

Common line styles:
- `'-'` solid line (default)
- `'--'` dashed line
- `':'` dotted line
- `'-.'` dash-dot line

Common markers:
- `'o'` circle
- `'+'` plus sign
- `'*'` asterisk
- `'.'` point
- `'x'` cross
- `'s'` square
- `'d'` diamond
- `'^'` upward triangle
- `'v'` downward triangle

Common colors:
- `'r'` red, `'g'` green, `'b'` blue, `'k'` black, `'m'` magenta, `'c'` cyan, `'y'` yellow

You can combine line style, marker, and color: `plot(x, y, 'r--o')` creates a red dashed line with circle markers.

### Multiple Lines on One Plot

**Python:**
```python
plt.plot(x, y1, label='Line 1')
plt.plot(x, y2, label='Line 2')
plt.legend()
```

**MATLAB:**
```matlab
x = 0:0.1:2*pi;
y1 = sin(x);
y2 = cos(x);
plot(x, y1, x, y2)           % Multiple pairs of x,y
legend('sin(x)', 'cos(x)')
```

Or using `hold on`:

```matlab
plot(x, y1)
hold on                      % Keep current plot, add to it
plot(x, y2)
hold off                     % Return to default behavior
legend('sin(x)', 'cos(x)')
```

### Subplots

**Python:**
```python
plt.subplot(2, 1, 1)  # 2 rows, 1 column, plot 1
plt.plot(x, y1)

plt.subplot(2, 1, 2)  # 2 rows, 1 column, plot 2
plt.plot(x, y2)
```

**MATLAB:**
```matlab
subplot(2, 1, 1)      % 2 rows, 1 column, plot 1
plot(x, y1)
title('First Plot')

subplot(2, 1, 2)      % 2 rows, 1 column, plot 2
plot(x, y2)
title('Second Plot')
```

### Scatter Plots and Other Plot Types

```matlab
% Scatter plot
x = randn(100, 1);    % 100 random numbers
y = randn(100, 1);
scatter(x, y)
title('Scatter Plot')

% Bar plot
categories = 1:5;
values = [23, 45, 31, 52, 38];
bar(categories, values)
title('Bar Chart')

% Histogram
data = randn(1000, 1);
histogram(data, 30)   % 30 bins
title('Histogram')
```

---

---

## MATLAB Documentation: Your Best Resource

One of the biggest advantages of MATLAB is its exceptional documentation. As you learn MATLAB, you'll frequently reference the documentation for built-in functions. Below are summaries of the key functions we'll use in class, followed by links to the full documentation pages.

### Plot Types

**`plot(x, y)`** - 2-D line plot
- Creates line plots from vectors of x and y data
- Can plot multiple lines: `plot(x1, y1, x2, y2, ...)`
- Customize with line specs: `plot(x, y, 'r--')` for red dashed line
- Many name-value pairs available: `'LineWidth'`, `'Color'`, `'Marker'`, etc.

**`scatter(x, y)`** - Scatter plot
- Displays data points without connecting lines
- Useful for showing relationships between variables
- Can control marker size: `scatter(x, y, sizes)`
- Can control marker color: `scatter(x, y, sizes, colors)`
- Optional: `'filled'` makes solid markers instead of hollow

**`bar(x, y)`** - Bar graph
- Creates vertical bar charts (use `barh` for horizontal)
- Great for categorical data or comparing discrete values
- Can create grouped or stacked bars for multiple data series
- Customize with `'FaceColor'`, `'EdgeColor'`, `'BarWidth'`

**`histogram(data)`** - Histogram plot
- Shows distribution of data by grouping into bins
- Can specify number of bins: `histogram(data, 20)`
- Can specify bin edges: `histogram(data, [0, 5, 10, 15])`
- Options for normalization: `'probability'`, `'pdf'`, `'count'`

### Colors and Colormaps

**Specify Plot Colors**
- Short color codes: `'r'` (red), `'g'` (green), `'b'` (blue), `'k'` (black), `'m'` (magenta), `'c'` (cyan), `'y'` (yellow), `'w'` (white)
- RGB triplets: `[0.5, 0.2, 0.8]` for custom colors (values 0-1)
- Hexadecimal: `'#FF5733'` for precise colors
- Named colors: `'crimson'`, `'teal'`, etc.

**`colormap`** - View and set current colormap
- Controls color schemes for images and surface plots
- Built-in colormaps: `'parula'` (default), `'jet'`, `'hot'`, `'cool'`, `'gray'`
- Set with: `colormap(jet)` or `colormap('parula')`
- Create custom colormaps with RGB arrays

### Axes and Labels

**Axes Appearance**
- Control many visual aspects: limits, scale (linear/log), colors, grid
- Set axis limits: `xlim([0, 10])`, `ylim([-5, 5])`
- Toggle grid: `grid on` or `grid off`
- Set aspect ratio: `axis equal`, `axis square`

**`xticks(values)`** - Set or query x-axis tick values
- Define where tick marks appear: `xticks([0, 2, 4, 6, 8, 10])`
- Query current ticks: `current_ticks = xticks`
- Similarly: `yticks` for y-axis

**`xticklabels(labels)`** - Set or query x-axis tick labels
- Change what text appears at tick marks: `xticklabels({'Low', 'Med', 'High'})`
- Useful for categorical data
- Similarly: `yticklabels` for y-axis

**`xtickangle(angle)`** - Rotate x-axis tick labels
- Rotate labels for better readability: `xtickangle(45)` for 45-degree rotation
- Helpful when labels are long or numerous
- Similarly: `ytickangle` for y-axis

**`xlabel(text)`, `ylabel(text)`** - Label axes
- Add descriptive text to axes: `xlabel('Time (seconds)')`
- Can customize font, size, interpreter: `xlabel('Distance (m)', 'FontSize', 14)`

**`title(text)`** - Add title to plot
- Adds title above the plot: `title('Temperature vs Time')`
- Customize: `title('My Plot', 'FontSize', 16, 'FontWeight', 'bold')`

**`legend(labels)`** - Add legend to axes
- Identifies multiple lines/data series: `legend('Data 1', 'Data 2')`
- Control location: `legend('Location', 'northwest')` or `'best'`
- Can specify which objects to include in legend

### Saving Figures

**`saveas(fig, filename)`** - Save figure to file
- Saves current figure: `saveas(gcf, 'myplot.png')`
- Supports many formats: `.png`, `.jpg`, `.pdf`, `.fig`, `.eps`
- Specify format explicitly: `saveas(gcf, 'myplot', 'png')`
- `gcf` means "get current figure"

### Full Documentation Links

For complete details, examples, and advanced usage, refer to these MATLAB documentation pages:

**Plot Types:**
- [2-D line plot - MATLAB](https://www.mathworks.com/help/matlab/ref/plot.html)
- [Scatter plot - MATLAB](https://www.mathworks.com/help/matlab/ref/scatter.html)
- [Bar graph - MATLAB](https://www.mathworks.com/help/matlab/ref/bar.html)
- [Histogram plot - MATLAB](https://www.mathworks.com/help/matlab/ref/histogram.html)

**Colors and Appearance:**
- [Specify Plot Colors - MATLAB & Simulink](https://www.mathworks.com/help/matlab/creating_plots/specify-plot-colors.html)
- [View and set current colormap - MATLAB](https://www.mathworks.com/help/matlab/ref/colormap.html)
- [Axes Appearance - MATLAB & Simulink](https://www.mathworks.com/help/matlab/creating_plots/axes-appearance.html)

**Axes Customization:**
- [xticks - Set or query x-axis tick values - MATLAB](https://www.mathworks.com/help/matlab/ref/xticks.html)
- [xticklabels - Set or query x-axis tick labels - MATLAB](https://www.mathworks.com/help/matlab/ref/xticklabels.html)
- [xtickangle - Rotate x-axis tick labels - MATLAB](https://www.mathworks.com/help/matlab/ref/xtickangle.html)
- [xlabel - Label x-axis - MATLAB](https://www.mathworks.com/help/matlab/ref/xlabel.html)
- [title - Add title - MATLAB](https://www.mathworks.com/help/matlab/ref/title.html)
- [legend - Add legend to axes - MATLAB](https://www.mathworks.com/help/matlab/ref/legend.html)

**Saving:**
- [saveas - Save figure - MATLAB](https://www.mathworks.com/help/matlab/ref/saveas.html)

---

## Try It Yourself

Once you have MATLAB installed, try running this complete example:

```matlab
% Create data
x = linspace(0, 4*pi, 100);
y1 = sin(x);
y2 = sin(x) .* exp(-x/10);  % Note the .* for element-wise multiplication!

% Create figure with two subplots
figure
subplot(2, 1, 1)
plot(x, y1, 'b-', 'LineWidth', 1.5)
title('Simple Sine Wave')
xlabel('x')
ylabel('sin(x)')
grid on

subplot(2, 1, 2)
plot(x, y2, 'r-', 'LineWidth', 1.5)
title('Damped Sine Wave')
xlabel('x')
ylabel('sin(x) * exp(-x/10)')
grid on
```

---

## Quick Reference Table

| Task | Python | MATLAB |
|------|--------|--------|
| Suppress output | Automatic | Add `;` at end |
| Comment | `#` | `%` |
| First index | `0` | `1` |
| Last index | `-1` | `end` |
| Indexing | `arr[i]` | `arr(i)` |
| Range | `range(0, 10)` | `0:9` |
| Plot | `plt.plot(x, y)` | `plot(x, y)` |
| Show plot | `plt.show()` | Automatic |
| Element-wise multiply | `arr1 * arr2` (NumPy) | `arr1 .* arr2` |

---

## Coming to Class Prepared

Before class, make sure you:
1. Have MATLAB installed and can open it
2. Can create a simple array: `x = 1:10;`
3. Can create a basic plot: `plot(x, x.^2)` (try it!)
4. Understand the indexing difference (1-based vs 0-based)

We'll build on these concepts in class with more advanced plotting techniques and real-world examples!
