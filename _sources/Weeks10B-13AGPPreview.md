# Weeks 10B-13A GPP-like Review

Since many of the GPP problems from this section of the course were from MATLAB tutorials, we have generated some similar practice problems for you to work on.  Solutions to these problems will be posted to Blackboard.

## Week 10B: From Python to MATLAB - Syntax and Plotting

### Basic Syntax Differences

**Solution 1:**
```
y =
    10
```
Explanation: The first line has a semicolon, so `x = 5` doesn't display output. The second line lacks a semicolon, so `y = x * 2` displays `y = 10`. The third line has a semicolon, so no output is shown.

**Solution 2:**
- Python: `my_list[0]` accesses the first element (0-indexed)
- MATLAB: `my_array(1)` accesses the first element (1-indexed)
- Python uses square brackets `[]` for indexing
- MATLAB uses parentheses `()` for indexing

**Solution 3:**
- a) `6` (third element)
- b) `10` (last element)
- c) `[4, 6, 8]` (elements from index 2 to 4)

**Solution 4:**
- a) `vec = 3:3:15;`
- b) `vec = 10:5:50;`
- c) `vec = linspace(0, 20, 5);`

### Plotting

**Solution 5:**
```matlab
x = 0:0.1:10;
y = x.^2;
plot(x, y)
xlabel('x values')
ylabel('y values')
title('Quadratic Function')
grid on
```

**Solution 6:**
- a) Red dashed line with circle markers
- b) Blue dotted line with asterisk markers
- c) Black solid line with square markers
- d) Green dash-dot line with upward triangle markers

**Solution 7:**
```matlab
x = 0:0.01:2*pi;

subplot(2, 1, 1)
plot(x, sin(x))
title('Sine Function')
xlabel('x')
ylabel('sin(x)')

subplot(2, 1, 2)
plot(x, cos(x))
title('Cosine Function')
xlabel('x')
ylabel('cos(x)')
```

**Solution 8:**
This code creates a single plot with two lines: a linear line (y1 = x) and a quadratic line (y2 = x²). Both lines are plotted on the same axes with a legend showing "Linear" and "Quadratic".

**Solution 9:**
```matlab
x = randn(50, 1);
y = randn(50, 1);
scatter(x, y)
title('Random Scatter Plot')
xlabel('X values')
ylabel('Y values')
```

---

## Week 11: Debugging and Loops

### Loop Syntax

**Solution 10:**
```matlab
for i = 1:10
    disp(i * 2)
end
```

**Solution 11:**
Output: `30`
Explanation: The loop iterates through [2, 4, 6, 8, 10] and sums them: 2 + 4 + 6 + 8 + 10 = 30

**Solution 12:**
```matlab
count = 10;
while count > 0
    disp(count)
    count = count - 2;
end
```

**Solution 13:**
`result = 32`
Explanation: Starting with 1, the loop doubles it 5 times: 1 → 2 → 4 → 8 → 16 → 32

### Debugging Concepts

**Solution 14:**
- a) After 1st iteration: `count = 1` (25 > 20)
- b) After 3rd iteration: `count = 2` (both 25 and 30 > 20)
- c) After loop completes: `count = 3` (25, 30, and 40 are all > 20)

**Solution 15:**
Bug: The variable `count` is never defined. It should be defined before the loop.
Fix: Add `count = length(data);` before the line `average = total / count;`, or calculate it as `average = total / length(data);`

**Solution 16:**
Error: "Index exceeds array dimensions" or similar.
Why: All values in the array (8, 12, 6, 15) are less than 20, so the while loop keeps incrementing `position`. Eventually, `position` becomes 5, which is beyond the array bounds (array only has 4 elements), causing an index out of bounds error.
Fix: Add a check: `while (position <= length(values)) && (values(position) < limit)`

### Performance Concepts

**Solution 17:**
Version B would run faster.

**Why:** Version A grows the array in each iteration, which requires:
1. Creating new memory for the larger array
2. Copying all existing values
3. Adding the new value
4. Deleting the old array

Version B pre-allocates the array with `zeros(1, 1000)`, so it just stores values directly without any copying or memory reallocation.

**Solution 18:**
```matlab
x = 1:100;
y = 3 * x + 5;
```

---

## Week 12A: Creating Vectors and Matrices

### Creating Matrices

**Solution 19:**
```matlab
mat = [1 2 3 4; 5 6 7 8; 9 10 11 12];
```
or
```matlab
mat = [1:4; 5:8; 9:12];
```

**Solution 20:**
- a) `vec1 = [1, 2, 3, 4]` creates a row vector (1×4)
- b) `vec2 = [1; 2; 3; 4]` creates a column vector (4×1)
The difference is using commas (or spaces) vs semicolons as separators.

**Solution 21:**
- a) `vec = [2; 4; 6; 8];`
- b) `vec = (2:2:8)';` or `vec = [2:2:8]';`

### Accessing Matrix Elements

**Solution 22:**
- a) `7` (element at row 2, column 3)
- b) `[1 2 3 4]` (entire first row)
- c) `[4; 8; 12]` (entire fourth column)
- d) `[6 7; 10 11]` (2×2 submatrix from rows 2-3, columns 2-3)
- e) `12` (last element: row 3, column 4)

### Special Matrix-Creating Functions

**Solution 23:**
- a) `zeros(3, 3)` or `zeros(3)`
- b) `ones(2, 4)`
- c) `eye(4)`
- d) `randi([5, 15], 3, 2)`

**Solution 24:**
- a) `rand(3, 3)` - creates random numbers uniformly distributed between 0 and 1
- b) `randn(3, 3)` - creates random numbers from a normal distribution (mean=0, std=1)
- c) `randi(10, 3, 3)` - creates random integers uniformly distributed between 1 and 10

### Concatenating Matrices

**Solution 25:**
- a) `[A, B]` = `[1 2 5 6; 3 4 7 8]` (side by side, 2×4 matrix)
- b) `[A; B]` = `[1 2; 3 4; 5 6; 7 8]` (stacked vertically, 4×2 matrix)

**Solution 26:**
- a) `[C, D]` = `[1 2 3 4 5 6]` - Works! Both are row vectors (1×3), result is 1×6
- b) `[C; D]` = `[1 2 3; 4 5 6]` - Works! Creates a 2×3 matrix

**Solution 27:**
Error: Dimensions don't match for concatenation. A is 2×3 and B is 2×2. For horizontal concatenation, they must have the same number of rows (which they do), but MATLAB would produce an error because the columns don't align properly. Actually, this would work and produce a 2×5 matrix: `[1 2 3 7 8; 4 5 6 9 10]`. My initial statement was incorrect - horizontal concatenation only requires the same number of rows.

**Correction:** This WORKS and produces:
```
C = [1 2 3 7 8
     4 5 6 9 10]
```

### Modifying Matrices

**Solution 28:**
- a) `vec(3) = 99;`
- b) `vec(2:4) = 0;` or `vec(2:4) = [0 0 0];`

**Solution 29:**
- a) `mat(2,:) = [40 50 60];`
- b) `mat(:,1) = [10; 20; 30];`
- c) `mat(2:3, 2:3) = [0 0; 0 0];` or `mat(2:3, 2:3) = 0;`

**Solution 30:**
- a) `vec(vec > 10) = 10;`
- b) `vec(vec < 5) = 5;`

### Reshaping Arrays

**Solution 31:**
- a) `reshape(A, [3, 4])` = 
```
1  4  7  10
2  5  8  11
3  6  9  12
```
- b) `reshape(A, [2, 6])` = 
```
1  3  5  7  9  11
2  4  6  8  10 12
```
- c) `reshape(A, [4, 3])` = 
```
1  4  7  10
2  5  8  11
3  6  9  12
```
Wait, this is incorrect. Let me recalculate:
- c) `reshape(A, [4, 3])` = 
```
1  5  9
2  6  10
3  7  11
4  8  12
```

**Solution 32:**
Yes, it's possible! A 2×6 matrix has 12 elements, and a 3×4 matrix also has 12 elements. Since the total number of elements is the same, reshape will work.

---

## Week 12B: Vector and Matrix Operations

### Scalar Operations

**Solution 33:**
- a) `[6, 12, 18, 24]`
- b) `[12, 14, 16, 18]`
- c) `[-3, -1, 1, 3]`
- d) `[1, 2, 3, 4]`

**Solution 34:**
- a) `mat * 2` = `[4 8 12; 2 6 10]`
- b) `mat + 10` = `[12 14 16; 11 13 15]`

### Array Operations

**Solution 35:**
- a) `vec .^ 2` = `[4, 16, 36, 64]`
- b) `vec .* vec` = `[4, 16, 36, 64]` (same as squaring)

**Solution 36:**
- a) `.*` performs element-wise multiplication: each element is multiplied by the corresponding element in the other array. Both arrays must have compatible dimensions.
- b) `*` performs matrix multiplication following linear algebra rules. The number of columns in the first matrix must equal the number of rows in the second matrix. The result's dimensions are (rows of first) × (columns of second).

**Solution 37:**
- a) `vec1 + vec2` = `[3, 6, 9]`
- b) `vec1 .* vec2` = `[2, 8, 18]`
- c) `vec1 ./ vec2` = `[2, 2, 2]`
- d) `vec1 .^ vec2` = `[2, 16, 216]` (2¹, 4², 6³)

**Solution 38:**
- a) `A .* B` = `[2 0; 3 8]`
- b) `A ./ B` = `[0.5 Inf; 3 2]` (Note: 2/0 = Inf)
- c) `A .^ 2` = `[1 4; 9 16]`

### Matrix Multiplication

**Solution 39:**
- a) `A * B` = `[1*2+2*1  1*0+2*2; 3*2+4*1  3*0+4*2]` = `[4 4; 10 8]`
- b) `B * A` = `[2*1+0*3  2*2+0*4; 1*1+2*3  1*2+2*4]` = `[2 4; 7 10]`
- c) No, they're not the same. Matrix multiplication is NOT commutative (order matters).

**Solution 40:**
- a) Yes, result is 3×2
- b) No, inner dimensions don't match (3 ≠ 2)
- c) Yes, result is 3×4
- d) Yes, result is 2×2

**Solution 41:**
- a) `A * B` = `[1*4 + 2*5 + 3*6]` = `[32]` (a 1×1 scalar)
- b) `B * A` = `[4*1  4*2  4*3; 5*1  5*2  5*3; 6*1  6*2  6*3]` = `[4 8 12; 5 10 15; 6 12 18]` (a 3×3 matrix)
- c) First result is 1×1, second result is 3×3

### Logical Operations and Indexing

**Solution 42:**
- a) `[1 0 1 1 0 1]` (true, false, true, true, false, true)
- b) `[0 1 1 0 1 1]` (false, true, true, false, true, true)
- c) `[0 0 1 0 0 0]` (false, false, true, false, false, false)

**Solution 43:**
- a) `vec(vec < 10)` = `[5, 8, 3, 9]`
- b) `vec(vec > 7)` = `[12, 8, 15, 9]`

**Solution 44:**
- a) `mat > 5` = 
```
[0 0 1
 0 0 1
 1 0 1]
```
- b) `mat < 4` = 
```
[1 0 0
 1 1 0
 0 0 0]
```

### Statistical Functions

**Solution 45:**
- a) `min(vec)` = `1`
- b) `max(vec)` = `9`
- c) `sum(vec)` = `35`
- d) `mean(vec)` = `5`

**Solution 46:**
- a) `sum(mat)` = `[12 10 23]` (sums each column)
- b) `max(mat)` = `[7 5 9]` (max of each column)
- c) `min(mat, 2)` = `[2; 1; 4]` (min of each row)
- d) `mean(mat, 2)` = `[5; 3.33; 6.67]` (approximately)
- e) `sum(sum(mat))` = `45`

**Solution 47:**
- a) `diff(vec)` = `[10 10 10 10]` (differences between consecutive elements)
- b) `cumsum(vec)` = `[10 30 60 100 150]` (cumulative sum)
- c) `prod(vec)` = `12000000` (product of all elements: 10*20*30*40*50)

**Solution 48:**
- a) `sort(vec)` = `[2, 3, 4, 8, 9, 11, 15]`
- b) `median(vec)` = `8` (middle value when sorted)

### Matrix Manipulation Functions

**Solution 49:**
- a) `mat'` = `[1 4; 2 5; 3 6]` (3×2 matrix)
- b) `fliplr(mat)` = `[3 2 1; 6 5 4]` (columns flipped left-right)
- c) `flipud(mat)` = `[4 5 6; 1 2 3]` (rows flipped up-down)
- d) `rot90(mat)` = `[3 6; 2 5; 1 4]` (rotated 90° counterclockwise)

**Solution 50:**
- a) `fliplr(vec)` = `[11, 8, 5, 2]`
- b) `flip(vec)` = `[11, 8, 5, 2]` (same result for row vector)

**Solution 51:**
- a) `repmat(mymat, 2, 1)` = 
```
[1 2
 3 4
 1 2
 3 4]
```
- b) `repmat(mymat, 1, 3)` = 
```
[1 2 1 2 1 2
 3 4 3 4 3 4]
```
- c) `repelem(mymat, 2, 2)` = 
```
[1 1 2 2
 1 1 2 2
 3 3 4 4
 3 3 4 4]
```

### Square Matrix Functions

**Solution 52:**
- a) `diag(mat)` = `[1; 5; 3]` (column vector of diagonal elements)
- b) `trace(mat)` = `9` (sum of diagonal: 1 + 5 + 3)
- c) `isdiag(mat)` = `1` (true, it is a diagonal matrix)
- d) Yes, it's symmetric. `issymmetric(mat)` = `1` (true)

**Solution 53:**
`diag(vec)` creates:
```
[2 0 0
 0 4 0
 0 0 6]
```

**Solution 54:**
`eye(5)` creates:
```
[1 0 0 0 0
 0 1 0 0 0
 0 0 1 0 0
 0 0 0 1 0
 0 0 0 0 1]
```

---

## Week 13A: File I/O and Scripts

### Script Basics

**Solution 55:**
- `x = 5 * 3` displays the output: `x = 15`
- `y = 5 * 3;` suppresses the output (no display)
The semicolon is the difference.

**Solution 56:**
```matlab
fprintf('Temperature: %.1f degrees\n', temp);
```

**Solution 57:**
```matlab
C = (F - 32) * 5/9;
K = C + 273.15;
fprintf('Fahrenheit: %.2f\n', F);
fprintf('Celsius: %.2f\n', C);
fprintf('Kelvin: %.2f\n', K);
```

### Directory Operations

**Solution 58:**
- a) `pwd`
- b) `isfolder('Data')`
- c) `if ~isfolder('Data'), mkdir('Data'); end`

**Solution 59:**
```matlab
path = fullfile('Results', 'Figures', 'plot1.png');
```

**Solution 60:**
```matlab
isfile('data.csv')
```

### Reading Data

**Solution 61:**
```matlab
studentData = readtable('scores.csv');
```

**Solution 62:**
- a) `imshow(img);`
- b) `redChannel = img(:,:,1);`
- c) `imwrite(redChannel, 'red_channel.png');`

### Writing Data

**Solution 63:**
```matlab
save('experiment_data.mat', 'time', 'temperature');
```

**Solution 64:**
```matlab
fid = fopen('output.txt', 'w');
fprintf(fid, 'Experiment Results\n');
fprintf(fid, 'Average: %.1f\n', avg);
fprintf(fid, 'Maximum: %.1f\n', maxVal);
fclose(fid);
```

**Solution 65:**
```matlab
saveas(gcf, fullfile('Figures', 'results_plot.png'));
```

---

## Comprehensive Review Problems

**Solution 66:**
- a) `A + B` = `[6 6; 3 9]`
- b) `A .* B` = `[8 5; 2 18]`
- c) `A * B` = `[2*4+5*2  2*1+5*6; 1*4+3*2  1*1+3*6]` = `[18 32; 10 19]`
- d) `A .^ 2` = `[4 25; 1 9]`
- e) `A'` = `[2 1; 5 3]`

**Solution 67:**
```matlab
vec = 1:20;                    % a) Create vector
oddVec = vec(1:2:end);         % b) Extract odd-indexed elements
squared = oddVec.^2;           % c) Square each element
total = sum(squared);          % d) Sum all values
```

**Solution 68:**
```matlab
% a) Logical array
logical_arr = vec > 7;         % [1 0 1 0 1 1]

% b) Extract values
values = vec(vec > 7);         % [8, 15, 12, 9]

% c) Replace values < 10 with 10
vec(vec < 10) = 10;            % vec becomes [10, 10, 15, 10, 12, 10]

% d) Calculate mean
meanVal = mean(vec);           % 11.1667
```

**Solution 69:**
```matlab
% a) Identity matrix
I = eye(3);

% b) Multiply by vector
result = I * [5; 10; 15];      % result = [5; 10; 15]

% c) Multiplying by an identity matrix returns the original vector/matrix unchanged.
%    This is because the identity matrix has 1s on the diagonal and 0s elsewhere,
%    so it acts as a multiplicative identity in matrix algebra.
```

**Solution 70:**
```matlab
% a) Create x values
x = linspace(0, 10, 100);

% b) Calculate y and z
y = sin(x);
z = cos(x);

% c) Create figure with subplots
figure

% d) First subplot
subplot(2, 1, 1)
plot(x, y, 'r-')
title('Sine Function')
xlabel('x')
ylabel('sin(x)')
grid on

% e) Second subplot
subplot(2, 1, 2)
plot(x, z, 'b--')
title('Cosine Function')
xlabel('x')
ylabel('cos(x)')
grid on
```

**Solution 71:**
Bug: Line 5 has `count = count;` which doesn't increment the count.
Fix:
```matlab
numbers = [-5, 3, -2, 8, 0, -1, 6];
count = 0;

for i = 1:length(numbers)
    if numbers(i) > 0
        count = count + 1;    % Fixed: added + 1
    end
end

disp(count)  % Output: 3
```

**Solution 72:**
Bug: `maxVal` is initialized to 0, so if all data values are positive and at least one is greater than 0, it will work. However, if all values were negative, it wouldn't find the correct maximum. Additionally, since all values in the array ARE greater than 0, this code actually works correctly for this specific case. But the logic could be improved by initializing `maxVal` to the first element or to `-Inf`.

Actually, looking more carefully: the code will work correctly for this example! It will find position 4 with value 30.

If there's a bug to find, it might be that we should initialize:
```matlab
data = [10, 25, 15, 30, 20];
maxPos = 1;
maxVal = data(1);  % Initialize to first element instead of 0

for i = 2:length(data)  % Start from 2 since we used element 1 for initialization
    if data(i) > maxVal
        maxVal = data(i);
        maxPos = i;
    end
end

disp(['Maximum value is at position: ', num2str(maxPos)])
```

**Solution 73:**
Version B would run much faster.

**Why:**
- Version A uses a loop with conditional checking for each element individually
- Version B uses vectorization:
  - `data > threshold` creates a logical array in one operation
  - `sum()` counts the true values in one operation
- Vectorized operations in MATLAB are highly optimized and execute much faster than loops
- For large arrays, Version B can be 10-100x faster or more

**Solution 74:**
```matlab
% a) Create temperature data
temps = randi([60, 90], 1, 24);

% b) Calculate mean
avgTemp = mean(temps);

% c) Count hours above 75
hoursAbove75 = sum(temps > 75);

% d) Create plot
hours = 1:24;
plot(hours, temps, 'b-o', 'LineWidth', 1.5)
xlabel('Hour of Day')
ylabel('Temperature (°F)')
title('24-Hour Temperature Analysis')
grid on

% e) Save plot
saveas(gcf, 'temperature_analysis.png');

% Display results
fprintf('Average temperature: %.1f°F\n', avgTemp);
fprintf('Hours above 75°F: %d\n', hoursAbove75);
```

**Solution 75:**
```matlab
% a) Check and create Output folder
if ~isfolder('Output')
    mkdir('Output');
end

% b) Generate random data
x = linspace(0, 10, 50);
y = 2*x + randn(1,50);

% c) Save data to MAT file
dataPath = fullfile('Output', 'data.mat');
save(dataPath, 'x', 'y');

% d) Create scatter plot
figure
scatter(x, y, 'filled')
xlabel('x')
ylabel('y')
title('Linear Relationship with Noise')
grid on

% e) Save figure
figPath = fullfile('Output', 'scatter_plot.png');
saveas(gcf, figPath);

disp('Analysis complete! Files saved to Output folder.');
```

---

## Notes on Common Mistakes

### Indexing
- Remember MATLAB is 1-indexed! The first element is at index 1, not 0
- Use `end` to refer to the last element

### Operators
- Element-wise operations need dots: `.* ./ .^`
- Matrix operations don't use dots: `* / ^`
- Addition and subtraction are the same: `+ -`

### Dimensions
- Row vectors: `[1, 2, 3]` or `[1 2 3]`
- Column vectors: `[1; 2; 3]`
- Transpose with `'`

### Loops
- Always close loops with `end`
- Pre-allocate arrays before loops for performance
- Consider vectorization instead of loops when possible

### File I/O
- Always close files: `fclose(fid)`
- Use `fullfile` for cross-platform paths
- Check file/folder existence before operations