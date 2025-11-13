# Week 12a: Creating Vectors and Matrices in MATLAB

## Introduction to MATLAB's Matrix-Based Structure

MATLAB® is short for Matrix Laboratory. Everything in MATLAB is written to work with vectors and matrices. Vectors and matrices are used to store sets of values, all of which are the same type. A matrix can be visualized as a table of values. The dimensions of a matrix are r × c, where r is the number of rows and c is the number of columns. This is pronounced "r by c".

A vector can be either a row vector or a column vector. If a vector has n elements, a row vector would have the dimensions 1 × n, and a column vector would have the dimensions n × 1. A scalar (one value) has the dimensions 1 × 1. Therefore, vectors and scalars are actually just special cases of matrices.

### Visual Examples

Here are some diagrams showing, from left to right, a scalar, a column vector, a row vector, and a matrix:

3       5       [8 3 11 9]      [6 3]
        8                       [5 7]
        4

- The scalar is 1 × 1
- The column vector is 3 × 1 (three rows by one column)
- The row vector is 1 × 4 (one row by four columns)
- The matrix is 2 × 3 (two rows by three columns)

All of the values stored in these matrices are stored in what are called **elements**.

## Creating Row Vectors

There are several ways to create row vector variables:

### Direct Entry Method

The most direct way is to put the values that you want in the vector in square brackets, separated by either spaces or commas:

```matlab
>> v = [1 2 3 4]
v =
    1  2  3  4

>> v = [1,2,3,4]
v =
    1  2  3  4
```

### Colon Operator Method

If the values in the vector are regularly spaced, the colon operator can be used to iterate through these values:

```matlab
>> vec = 2:6
vec =
    2  3  4  5  6
```

With the colon operator, a step value can also be specified by using another colon, in the form (first:step:last):

```matlab
>> nv = 1:2:9
nv =
    1  3  5  7  9
```

### linspace Function

The linspace function creates a linearly spaced vector; linspace(x,y,n) creates a vector with n values in the inclusive range from x to y. If n is omitted, the default is 100 elements:

```matlab
>> ls = linspace(3,15,5)
ls =
    3  6  9  12  15
```

## Accessing Vector Elements

The elements in a vector are numbered sequentially; each element number is called the **index** or **subscript**. In MATLAB, the indices start at 1. For example:

```
newvec
  1    2    3    4    5    6    7    8    9   10
[1    3    5    7    9    3    6    9   12   15]
```

A particular element in a vector is accessed using the name of the vector variable and the index in parentheses:

```matlab
>> newvec(5)
ans =
    9
```

The expression newvec(5) would be pronounced "newvec sub 5", where sub is short for the word subscript.

### Extracting Subsets

A subset of a vector, which would be a vector itself, can also be obtained using the colon operator:

```matlab
>> b = newvec(4:6)
b =
    7  9  3
```

## Creating Column Vectors

### Direct Entry Method

One way to create a column vector is to explicitly put the values in square brackets, separated by **semicolons** (rather than commas or spaces):

```matlab
>> c = [1; 2; 3; 4]
c =
    1
    2
    3
    4
```

### Transpose Method

There is no direct way to use the colon operator to get a column vector. However, any row vector created using any method can be transposed to result in a column vector. In general, the transpose of a matrix is a new matrix in which the rows and columns are interchanged.

In MATLAB, the **apostrophe** (or single quote) is built in as the transpose operator:

```matlab
>> r = 1:3;
>> c = r'
c =
    1
    2
    3
```

## Creating Matrices

Creating a matrix variable is simply a generalization of creating row and column vector variables. The values within a row are separated by either spaces or commas, and the different rows are separated by semicolons:

```matlab
>> mat = [4 3 1; 2 5 6]
mat =
    4  3  1
    2  5  6
```

### Accessing Matrix Elements

To refer to matrix elements, the row and then the column subscripts are given in parentheses (always the row first and then the column):

```matlab
>> mat = [2:4; 3:5]
mat =
    2  3  4
    3  4  5

>> mat(2,3)
ans =
    5
```

This is called **subscripted indexing**; it uses the row and column subscripts.

### Accessing Matrix Subsets

It is also possible to refer to a subset of a matrix:

```matlab
>> mat(1:2,2:3)
ans =
    3  4
    4  5
```

## Modifying Vector and Matrix Elements

Once you've created vectors and matrices, you can modify their contents by assigning new values to specific elements or subsets.

### Modifying Individual Elements

To change a single element, use its index (for vectors) or row and column subscripts (for matrices):

```matlab
>> v = [10 20 30 40 50];
>> v(3) = 99
v =
    10  20  99  40  50

>> mat = [1 2 3; 4 5 6; 7 8 9];
>> mat(2,3) = 100
mat =
    1    2    3
    4    5  100
    7    8    9
```

### Modifying Entire Rows or Columns

You can use the colon operator to select and modify entire rows or columns. A single colon by itself means "all elements in that dimension":

```matlab
>> mat = [1 2 3; 4 5 6; 7 8 9];
>> mat(2,:) = [40 50 60]  % Modify entire second row
mat =
     1   2   3
    40  50  60
     7   8   9

>> mat(:,1) = [10; 20; 30]  % Modify entire first column
mat =
    10   2   3
    20  50  60
    30   8   9
```

### Modifying Submatrices

You can modify a rectangular subset of a matrix by specifying row and column ranges:

```matlab
>> mat = [1 2 3 4; 5 6 7 8; 9 10 11 12; 13 14 15 16];
>> mat(2:3, 2:4) = [0 0 0; 0 0 0]
mat =
     1   2   3   4
     5   0   0   0
     9   0   0   0
    13  14  15  16
```

You can also replace a submatrix with a scalar value, which will be replicated to fill all selected positions:

```matlab
>> mat = [1 2 3; 4 5 6; 7 8 9];
>> mat(1:2, 2:3) = 0
mat =
    1  0  0
    4  0  0
    7  8  9
```

### Modifying with Vector Subscripts

You can select specific (non-contiguous) elements using a vector of indices:

```matlab
>> v = [10 20 30 40 50];
>> v([1 3 5]) = [100 300 500]
v =
   100  20  300  40  500

>> mat = [1 2 3; 4 5 6; 7 8 9];
>> mat([1 3], [1 3]) = [10 30; 70 90]  % Modify corners
mat =
    10   2  30
     4   5   6
    70   8  90
```

### Modifying Using Logical Indexing

Logical indexing allows you to modify elements that meet certain conditions. This is very powerful for selective modifications:

```matlab
>> v = [5 12 8 3 15 9];
>> v(v > 10) = 10  % Cap all values greater than 10
v =
    5  10  8  3  10  9

>> mat = [1 -2 3; -4 5 -6; 7 -8 9];
>> mat(mat < 0) = 0  % Replace all negative values with 0
mat =
    1  0  3
    0  5  0
    7  0  9
```

You can also use logical conditions to create more complex modifications:

```matlab
>> scores = [85 92 78 95 88];
>> scores(scores < 80) = 80  % Set minimum score to 80
scores =
    85  92  80  95  88

>> mat = [10 25 30; 45 50 65; 70 85 90];
>> mat(mat >= 50 & mat <= 70) = 60  % Set values in range [50,70] to 60
mat =
    10  25  30
    45  60  60
    60  85  90
```

### Important Notes on Modification

1. When modifying submatrices, the dimensions of the replacement values must match the dimensions being replaced (unless you're using a scalar).
2. Logical indexing returns elements in a column vector, so modifications using logical indexing work element-by-element.
3. You can combine logical conditions using `&` (AND), `|` (OR), and `~` (NOT).

## Special Matrix-Creating Functions

MATLAB has many functions that help create matrices with certain values or a particular structure:

### zeros - Create Array of All Zeros

Creates a matrix filled with zeros:

- zeros(n) creates an n × n matrix of zeros
- zeros(m,n) creates an m × n matrix of zeros

```matlab
>> Z = zeros(3)
Z =
    0  0  0
    0  0  0
    0  0  0
```

### ones - Create Array of All Ones

Creates a matrix filled with ones:

- ones(n) creates an n × n matrix of ones
- ones(m,n) creates an m × n matrix of ones

```matlab
>> O = ones(2,4)
O =
    1  1  1  1
    1  1  1  1
```

### eye - Create Identity Matrix

Creates an identity matrix (diagonal elements are 1, all others are 0):

- eye(n) creates an n × n identity matrix

```matlab
>> I = eye(3)
I =
    1  0  0
    0  1  0
    0  0  1
```

### rand - Uniformly Distributed Random Numbers

Creates a matrix with random elements uniformly distributed on the interval (0, 1):

- rand(n) creates an n × n matrix
- rand(m,n) creates an m × n matrix

```matlab
>> R = rand(2,3)
R =
    0.8147  0.9134  0.2785
    0.9058  0.6324  0.5469
```

### randn - Normally Distributed Random Numbers

Creates a matrix with random elements having a normal distribution (mean = 0, variance = 1):

- randn(n) creates an n × n matrix
- randn(m,n) creates an m × n matrix

```matlab
>> N = randn(2,2)
N =
   -0.4326   0.2877
   -1.6656  -1.1465
```

### randi - Uniformly Distributed Random Integers

Creates a matrix with random integer values:

- randi(imax, n) creates an n × n matrix with integers from 1 to imax
- randi(imax, m, n) creates an m × n matrix
- randi([imin imax], m, n) creates a matrix with integers in the range [imin, imax]

```matlab
>> R = randi(10, 3, 4)
R =
     9   2   7   1
    10   7   1   5
     2  10   5   8
```

## Concatenating Matrices

**Concatenation** is the process of joining matrices together to create a larger matrix. Square brackets [] serve as the concatenation operator.

### Horizontal Concatenation

To concatenate matrices side-by-side, separate them with commas or spaces:

```matlab
>> A = [1 2; 3 4];
>> B = [5 6; 7 8];
>> C = [A, B]
C =
    1  2  5  6
    3  4  7  8
```

**Important:** When concatenating horizontally, matrices must have the same number of rows.

You can also use the horzcat function:

```matlab
>> C = horzcat(A, B)
```

### Vertical Concatenation

To concatenate matrices top-to-bottom, separate them with semicolons:

```matlab
>> A = [1 2; 3 4];
>> B = [5 6; 7 8];
>> C = [A; B]
C =
    1  2
    3  4
    5  6
    7  8
```

**Important:** When concatenating vertically, matrices must have the same number of columns.

You can also use the vertcat function:

```matlab
>> C = vertcat(A, B)
```

### Multi-Dimensional Concatenation

The cat function allows concatenation along any dimension:

- cat(1,A,B) concatenates vertically (along dimension 1)
- cat(2,A,B) concatenates horizontally (along dimension 2)
- cat(3,A,B) concatenates along the third dimension

## Expanding Matrices

You can expand a matrix by directly assigning values to elements outside the current size. MATLAB automatically expands the matrix and fills new positions with zeros:

```matlab
>> A = [1 2; 3 4];
>> A(3,4) = 10
A =
    1  2  0   0
    3  4  0   0
    0  0  0  10
```

### Preallocation Best Practice

For loops that repeatedly expand matrices, it's best to preallocate space for the largest anticipated matrix. Without preallocation, MATLAB has to allocate memory every time the size increases, slowing down operations:

```matlab
>> A = zeros(10000, 10000);  % Preallocate
```

## Reshaping Arrays

The reshape function rearranges existing elements in an array without changing the number of elements or their order. Elements are taken column-wise from the input array to fill the output array.

### Basic Syntax

```matlab
>> A = 1:12;
>> B = reshape(A, [3, 4])
B =
    1  4  7  10
    2  5  8  11
    3  6  9  12
```

### Automatic Dimension Calculation

You can use [] as a placeholder for one dimension, and MATLAB will automatically calculate it:

```matlab
>> A = 1:12;
>> B = reshape(A, [2, []])  % Automatically calculates 6 columns
B =
    1  3  5  7   9  11
    2  4  6  8  10  12
```

### Important Notes About Reshape

1. The total number of elements must remain the same
2. Elements are taken **column-wise** from the input
3. To reshape row-wise, transpose before and after: reshape(A', [m,n])'
4. reshape rearranges existing elements; use resize to add or remove elements

## Key Takeaways

1. MATLAB stores all data as matrices (vectors and scalars are special cases)
2. Indices in MATLAB start at 1
3. Use : for creating sequences and accessing ranges
4. Use ; to separate rows, , or spaces to separate columns
5. The ' operator transposes matrices
6. Square brackets [] are used for both creation and concatenation
7. MATLAB provides many functions (zeros, ones, eye, rand, randn, randi) for creating special matrices
8. When concatenating, dimensions must be compatible
9. You can modify individual elements, rows, columns, or submatrices using indexing
10. Logical indexing provides powerful conditional modification capabilities
11. reshape preserves elements but changes dimensions (column-wise ordering)
12. Preallocate large matrices for better performance

## Additional MATLAB Documentation Resources

For more detailed information about these functions and concepts, refer to the official MATLAB documentation:

- [Creating, Concatenating, and Expanding Matrices - MATLAB & Simulink](https://www.mathworks.com/help/matlab/math/creating-and-concatenating-matrices.html)
- [linspace - Generate linearly spaced vector - MATLAB](https://www.mathworks.com/help/matlab/ref/linspace.html)
- [ones - Create array of all ones - MATLAB](https://www.mathworks.com/help/matlab/ref/ones.html)
- [zeros - Create array of all zeros - MATLAB](https://www.mathworks.com/help/matlab/ref/zeros.html)
- [eye - Create an identity matrix - MATLAB](https://www.mathworks.com/help/matlab/ref/eye.html)
- [rand - Uniformly distributed random numbers - MATLAB](https://www.mathworks.com/help/matlab/ref/rand.html)
- [randn - Normally distributed random numbers - MATLAB](https://www.mathworks.com/help/matlab/ref/randn.html)
- [randi - Uniformly distributed random integers - MATLAB](https://www.mathworks.com/help/matlab/ref/randi.html)
- [reshape - Reshape array by rearranging existing elements - MATLAB](https://www.mathworks.com/help/matlab/ref/reshape.html)