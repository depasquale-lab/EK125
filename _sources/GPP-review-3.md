# MATLAB Practice Problems for Paper Exam Review

## Week 10B: From Python to MATLAB - Syntax and Plotting

### Basic Syntax Differences

**Problem 1:** Without using MATLAB, predict the output of the following code:
```matlab
x = 5;
y = x * 2
z = y + 3;
```

**Problem 2:** What is the difference between these two indexing operations?
- Python: `my_list[0]`
- MATLAB: `my_array(1)`

**Problem 3:** Given `vec = [2, 4, 6, 8, 10]`, what would be the result of:
- a) `vec(3)`
- b) `vec(end)`
- c) `vec(2:4)`

**Problem 4:** Write MATLAB code to create the following vectors:
- a) A row vector with values 3, 6, 9, 12, 15 using the colon operator
- b) A row vector with values from 10 to 50 with a step of 5
- c) A vector with 5 evenly spaced points from 0 to 20 using linspace

### Plotting

**Problem 5:** Write the MATLAB code to:
- a) Plot `y = x²` for x values from 0 to 10
- b) Add a title "Quadratic Function"
- c) Label the x-axis "x values" and y-axis "y values"
- d) Add a grid to the plot

**Problem 6:** What line style, color, and marker would the following create?
- a) `plot(x, y, 'r--o')`
- b) `plot(x, y, 'b:*')`
- c) `plot(x, y, 'k-s')`
- d) `plot(x, y, 'g-.^')`

**Problem 7:** Write code to create a figure with 2 rows and 1 column of subplots. In the first subplot, plot sin(x), and in the second, plot cos(x), for x from 0 to 2π.

**Problem 8:** Given the following code, what would happen?
```matlab
x = 0:0.5:5;
y1 = x;
y2 = x.^2;
plot(x, y1, x, y2)
legend('Linear', 'Quadratic')
```

**Problem 9:** Write the code to create a scatter plot of 50 random points and give it an appropriate title.

---

## Week 11: Debugging and Loops

### Loop Syntax

**Problem 10:** Convert this Python loop to MATLAB:
```python
for i in range(1, 11):
    print(i * 2)
```

**Problem 11:** What does this MATLAB loop output?
```matlab
sum = 0;
for i = 2:2:10
    sum = sum + i;
end
disp(sum)
```

**Problem 12:** Convert this Python while loop to MATLAB:
```python
count = 10
while count > 0:
    print(count)
    count = count - 2
```

**Problem 13:** What will be the final value of `result` after this loop executes?
```matlab
result = 1;
for i = 1:5
    result = result * 2;
end
```

### Debugging Concepts

**Problem 14:** Given this code:
```matlab
numbers = [10, 25, 30, 15, 40];
threshold = 20;
count = 0;

for i = 1:length(numbers)
    if numbers(i) > threshold
        count = count + 1;
    end
end
```
If you set a breakpoint at line 3 (`count = 0;`) and step through, what would be the value of `count` after:
- a) The 1st iteration?
- b) The 3rd iteration?
- c) The loop completes?

**Problem 15:** This code has a bug. Identify the problem:
```matlab
data = [5, 10, 15, 20];
total = 0;

for i = 1:length(data)
    total = total + data(i);
end

average = total / count;
```

**Problem 16:** What error will occur when running this code, and why?
```matlab
values = [8, 12, 6, 15];
limit = 20;

position = 1;
while values(position) < limit
    position = position + 1;
end
```

### Performance Concepts

**Problem 17:** Which of these two code snippets would run faster, and why?

**Version A:**
```matlab
result = [];
for i = 1:1000
    result = [result, i^2];
end
```

**Version B:**
```matlab
result = zeros(1, 1000);
for i = 1:1000
    result(i) = i^2;
end
```

**Problem 18:** Rewrite this loop using vectorization:
```matlab
x = 1:100;
y = zeros(1, 100);
for i = 1:length(x)
    y(i) = 3 * x(i) + 5;
end
```

---

## Week 12A: Creating Vectors and Matrices

### Creating Matrices

**Problem 19:** Write MATLAB code to create this matrix:
```
1  2  3  4
5  6  7  8
9  10 11 12
```

**Problem 20:** What's the difference between creating these two vectors?
- a) `vec1 = [1, 2, 3, 4]`
- b) `vec2 = [1; 2; 3; 4]`

**Problem 21:** Write MATLAB code to create a column vector with values 2, 4, 6, 8 using:
- a) Direct entry with semicolons
- b) The colon operator and transpose

### Accessing Matrix Elements

**Problem 22:** Given this matrix:
```matlab
mat = [1  2  3  4
       5  6  7  8
       9  10 11 12]
```
What value would be returned by:
- a) `mat(2,3)`
- b) `mat(1,:)`
- c) `mat(:,4)`
- d) `mat(2:3, 2:3)`
- e) `mat(end, end)`

### Special Matrix-Creating Functions

**Problem 23:** Write the MATLAB code to create:
- a) A 3×3 matrix of all zeros
- b) A 2×4 matrix of all ones
- c) A 4×4 identity matrix
- d) A 3×2 matrix of random integers between 5 and 15

**Problem 24:** What's the difference between:
- a) `rand(3, 3)`
- b) `randn(3, 3)`
- c) `randi(10, 3, 3)`

### Concatenating Matrices

**Problem 25:** Given `A = [1 2; 3 4]` and `B = [5 6; 7 8]`, what would be the result of:
- a) `[A, B]` (horizontal concatenation)
- b) `[A; B]` (vertical concatenation)

**Problem 26:** Given `C = [1 2 3]` and `D = [4 5 6]`, can you perform:
- a) `[C, D]`
- b) `[C; D]`
For each, explain why it works or doesn't work, and give the result if it works.

**Problem 27:** What would happen if you tried to concatenate these matrices?
```matlab
A = [1 2 3; 4 5 6];
B = [7 8; 9 10];
C = [A, B]
```

### Modifying Matrices

**Problem 28:** Given `vec = [10 20 30 40 50]`, write code to:
- a) Change the third element to 99
- b) Change elements 2 through 4 to all be 0

**Problem 29:** Given this matrix:
```matlab
mat = [1 2 3
       4 5 6
       7 8 9]
```
Write code to:
- a) Replace the second row with [40 50 60]
- b) Replace the first column with [10; 20; 30]
- c) Replace the 2×2 submatrix in the lower-right corner with zeros

**Problem 30:** Given `vec = [5, 12, 8, 3, 15, 9]`, write code to:
- a) Replace all values greater than 10 with 10
- b) Replace all values less than 5 with 5

### Reshaping Arrays

**Problem 31:** Given `A = [1 2 3 4 5 6 7 8 9 10 11 12]`, what would be the result of:
- a) `reshape(A, [3, 4])`
- b) `reshape(A, [2, 6])`
- c) `reshape(A, [4, 3])`

**Problem 32:** You have a 2×6 matrix and want to reshape it into a 3×4 matrix. Is this possible? Why or why not?

---

## Week 12B: Vector and Matrix Operations

### Scalar Operations

**Problem 33:** Given `vec = [2, 4, 6, 8]`, calculate by hand:
- a) `vec * 3`
- b) `vec + 10`
- c) `vec - 5`
- d) `vec / 2`

**Problem 34:** Given this matrix:
```matlab
mat = [2 4 6
       1 3 5]
```
Calculate by hand:
- a) `mat * 2`
- b) `mat + 10`

### Array Operations

**Problem 35:** Given `vec = [2, 4, 6, 8]`, calculate by hand:
- a) `vec .^ 2`
- b) `vec .* vec`

**Problem 36:** What's the difference between these two operations?
- a) `.*` (element-wise multiplication)
- b) `*` (matrix multiplication)

**Problem 37:** Given `vec1 = [2, 4, 6]` and `vec2 = [1, 2, 3]`, calculate:
- a) `vec1 + vec2`
- b) `vec1 .* vec2`
- c) `vec1 ./ vec2`
- d) `vec1 .^ vec2`

**Problem 38:** Given `A = [1 2; 3 4]` and `B = [2 0; 1 2]`, calculate by hand:
- a) `A .* B`
- b) `A ./ B`
- c) `A .^ 2`

### Matrix Multiplication

**Problem 39:** Given `A = [1 2; 3 4]` and `B = [2 0; 1 2]`, calculate by hand:
- a) `A * B`
- b) `B * A`
- c) Are the results the same? Why or why not?

**Problem 40:** Can you perform matrix multiplication `A * B` for the following cases? If yes, what are the dimensions of the result?
- a) A is 3×4, B is 4×2
- b) A is 2×3, B is 2×3
- c) A is 3×1, B is 1×4
- d) A is 2×2, B is 2×2

**Problem 41:** Given `A = [1 2 3]` (1×3) and `B = [4; 5; 6]` (3×1), calculate:
- a) `A * B`
- b) `B * A`
- c) What are the dimensions of each result?

### Logical Operations and Indexing

**Problem 42:** Given `vec = [5, 12, 8, 3, 15, 9]`, what would be the result of:
- a) `vec < 10` (write the logical array)
- b) `vec > 7` (write the logical array)
- c) `vec == 8` (write the logical array)

**Problem 43:** Given `vec = [5, 12, 8, 3, 15, 9]` and the logical arrays from Problem 42a, what would be returned by:
- a) `vec(vec < 10)`
- b) `vec(vec > 7)`

**Problem 44:** Given this matrix:
```matlab
mat = [2  5  8
       3  1  6
       7  4  9]
```
Write the logical array that would result from:
- a) `mat > 5`
- b) `mat < 4`

### Statistical Functions

**Problem 45:** Given `vec = [3, 7, 2, 9, 5, 8, 1]`, calculate by hand:
- a) `min(vec)`
- b) `max(vec)`
- c) `sum(vec)`
- d) `mean(vec)`

**Problem 46:** Given the matrix:
```matlab
mat = [2 5 8
       3 1 6
       7 4 9]
```
Calculate by hand:
- a) `sum(mat)` (sum of each column)
- b) `max(mat)` (max of each column)
- c) `min(mat, 2)` (min of each row)
- d) `mean(mat, 2)` (mean of each row)
- e) `sum(sum(mat))` (overall sum)

**Problem 47:** Given `vec = [10 20 30 40 50]`, calculate:
- a) `diff(vec)`
- b) `cumsum(vec)`
- c) `prod(vec)`

**Problem 48:** Given `vec = [8, 2, 15, 4, 11, 3, 9]`, what would be returned by:
- a) `sort(vec)`
- b) `median(vec)`

### Matrix Manipulation Functions

**Problem 49:** Given `mat = [1 2 3; 4 5 6]` (a 2×3 matrix), what would be the result of:
- a) `mat'` (transpose)
- b) `fliplr(mat)` (flip left-right)
- c) `flipud(mat)` (flip up-down)
- d) `rot90(mat)` (rotate 90 degrees)

**Problem 50:** Given `vec = [2, 5, 8, 11]`, what would be the result of:
- a) `fliplr(vec)`
- b) `flip(vec)`

**Problem 51:** Given `mymat = [1 2; 3 4]`, what would be the result of:
- a) `repmat(mymat, 2, 1)` (replicate 2 rows, 1 column)
- b) `repmat(mymat, 1, 3)` (replicate 1 row, 3 columns)
- c) `repelem(mymat, 2, 2)` (replicate each element 2×2)

### Square Matrix Functions

**Problem 52:** Given this matrix:
```matlab
mat = [1 0 0
       0 5 0
       0 0 3]
```
- a) What would `diag(mat)` return?
- b) What would `trace(mat)` return?
- c) What would `isdiag(mat)` return?
- d) Is this matrix symmetric? What would `issymmetric(mat)` return?

**Problem 53:** Given `vec = [2, 4, 6]`, what would `diag(vec)` create?

**Problem 54:** What does `eye(5)` create? Write out the matrix.

---

## Week 13A: File I/O and Scripts

### Script Basics

**Problem 55:** What's the difference between running these two lines in a script?
```matlab
x = 5 * 3
y = 5 * 3;
```

**Problem 56:** Write the `fprintf` statement to display "Temperature: 72.5 degrees" where 72.5 is stored in a variable called `temp`.

**Problem 57:** You have Fahrenheit temperature stored in variable `F`. Write the code to:
- a) Convert to Celsius: C = (F - 32) × 5/9
- b) Convert to Kelvin: K = C + 273.15
- c) Display all three with labels using `fprintf`

### Directory Operations

**Problem 58:** Write MATLAB commands to:
- a) Display your current working directory
- b) Check if a folder called "Data" exists
- c) Create the folder "Data" if it doesn't exist

**Problem 59:** Rewrite this Windows-specific path using `fullfile`:
```matlab
path = 'Results\Figures\plot1.png';
```

**Problem 60:** What command would you use to check if a file called `data.csv` exists?

### Reading Data

**Problem 61:** You have a CSV file `scores.csv` with student names and test scores. Write the command to read it into a table variable called `studentData`.

**Problem 62:** After reading an image with `img = imread('photo.png')`, how would you:
- a) Display the image
- b) Extract just the red channel
- c) Save the red channel as a new image

### Writing Data

**Problem 63:** You have variables `time` and `temperature`. Write the code to save both variables to a MAT file called `experiment_data.mat`.

**Problem 64:** Write code using `fopen`, `fprintf`, and `fclose` to create a text file `output.txt` that contains:
```
Experiment Results
Average: 45.6
Maximum: 89.2
```
where 45.6 is stored in variable `avg` and 89.2 is in variable `maxVal`.

**Problem 65:** Write code to save a figure to a file called `results_plot.png` in a folder called `Figures`.

---

## Comprehensive Review Problems (All Readings)

**Problem 66:** Given `A = [2 5; 1 3]` and `B = [4 1; 2 6]`:
- a) Calculate `A + B`
- b) Calculate `A .* B`
- c) Calculate `A * B`
- d) Calculate `A .^ 2`
- e) Calculate `A'`

**Problem 67:** Write MATLAB code that would:
- a) Create a row vector from 1 to 20
- b) Extract every other element (1, 3, 5, ...)
- c) Compute the square of each extracted element
- d) Calculate the sum of all squared values

**Problem 68:** Given `vec = [8, 3, 15, 6, 12, 9]`:
- a) Create a logical array showing which values are greater than 7
- b) Extract only the values greater than 7
- c) Replace all values less than 10 with 10
- d) Calculate the mean of the modified vector

**Problem 69:** Create the following in MATLAB code:
- a) A 3×3 matrix with 1s on the diagonal and 0s elsewhere
- b) Multiply this matrix by the vector [5; 10; 15]
- c) What is special about multiplying by an identity matrix?

**Problem 70:** Write complete MATLAB code to:
- a) Create x values from 0 to 10 with 100 points
- b) Calculate y = sin(x) and z = cos(x)
- c) Create a figure with two subplots (one above the other)
- d) Plot y vs x in the first subplot with a red solid line
- e) Plot z vs x in the second subplot with a blue dashed line
- f) Add appropriate titles and labels to both subplots
- g) Add a grid to both plots

**Problem 71:** Debug this code that's supposed to count how many numbers in an array are positive:
```matlab
numbers = [-5, 3, -2, 8, 0, -1, 6];
count = 0;

for i = 1:length(numbers)
    if numbers(i) > 0
        count = count;
    end
end

disp(count)
```

**Problem 72:** This code is supposed to find the position of the maximum value in an array. Find and fix the bug:
```matlab
data = [10, 25, 15, 30, 20];
maxPos = 0;
maxVal = 0;

for i = 1:length(data)
    if data(i) > maxVal
        maxVal = data(i);
        maxPos = i;
    end
end

disp(['Maximum value is at position: ', num2str(maxPos)])
```

**Problem 73:** Which version would run faster for large arrays, and why?

**Version A:**
```matlab
result = 0;
for i = 1:length(data)
    if data(i) > threshold
        result = result + 1;
    end
end
```

**Version B:**
```matlab
result = sum(data > threshold);
```

**Problem 74:** You need to analyze temperature data. Write a script that:
- a) Creates a row vector of 24 temperatures (one per hour) using `randi` between 60 and 90
- b) Calculates the mean temperature
- c) Finds how many hours were above 75 degrees
- d) Creates a plot of temperature vs hour with appropriate labels
- e) Saves the plot as `temperature_analysis.png`

**Problem 75:** Write a complete script that:
- a) Checks if a folder `Output` exists, creates it if not
- b) Generates random data: `x = linspace(0, 10, 50)` and `y = 2*x + randn(1,50)`
- c) Saves both x and y to `Output/data.mat`
- d) Creates a scatter plot of y vs x
- e) Saves the figure to `Output/scatter_plot.png`
- f) Uses `fullfile` for all paths

---

## Quick Reference for Students

### Key Syntax Reminders:
- MATLAB indexing starts at **1** (not 0)
- Use `;` to **suppress output**
- Use `;` to separate **rows** in matrices
- Use `,` or spaces to separate **columns** in matrices
- Use `'` for **transpose**
- Use `end` for **last element** or to **close loops/conditionals**
- Element-wise operations need a **dot** (`.*, ./, .^`)
- Matrix multiplication uses just `*` (no dot)

### Loop Syntax:
- `for i = 1:n` ... `end`
- `while condition` ... `end`
- No indentation required, but `end` is mandatory

### File I/O:
- `readtable('file.csv')` for CSV files
- `imread('image.png')` for images
- `save('file.mat', 'var1', 'var2')` for MAT files
- `fopen`, `fprintf`, `fclose` for text files
- `fullfile('folder', 'file.ext')` for cross-platform paths

### Common Confusion Points:
- `.*` vs `*` (element-wise vs matrix multiplication)
- `[A, B]` vs `[A; B]` (horizontal vs vertical concatenation)
- `ones(3)` vs `eye(3)` (all ones vs identity matrix)
- `rand` vs `randn` vs `randi` (uniform vs normal vs integer)
- Column-wise vs row-wise operations (default is column-wise)
- Growing arrays in loops is SLOW - preallocate with `zeros` instead
- Vectorization is usually faster than loops