# Class 23 Pre-Read: Vector and Matrix Operations in MATLAB

## Scalar Operations

Scalar operations can be performed on arrays (vectors or matrices). For example, one can perform scalar operations on the following vector vec:

```matlab
>> vec = 1: 0.5: 3
vec =
    1.0000    1.5000    2.0000    2.5000    3.0000
```

Multiplying the vector by 2 performs the multiplication on every element in the vector, resulting in a vector with the same length as the original. Since this is not stored in a variable, the result is stored in the default variable `ans`.

```matlab
>> vec * 2
ans =
    2    3    4    5    6
```

All numerical operators can be used in this way. For example, we can add 5 to every element:

```matlab
>> newv = vec + 5
newv =
    6.0000    6.5000    7.0000    7.5000    8.0000
```

Scalar operations can also be performed on matrices:

```matlab
>> mat = randi([5, 20], 2, 3)
mat =
    17    15    18
    20     5    19

>> mat / 2
ans =
    8.5000    7.5000    9.0000
   10.0000    2.5000    9.5000
```

---

## Array Operations

MATLAB's array operations are highlighted for their simplicity and efficiency, particularly through **scalar and vector expansion**, features in MATLAB that automatically expand arrays to match each other's dimensions for certain operations.

You can also perform array operations on operands of different compatible sizes. Two arrays have compatible sizes if the size of each dimension is either the same or 1.

```matlab
>> vec1 = [2 11 33 5];
>> vec2 = linspace(3, 9, 4)
vec2 =
    3    5    7    9

>> vec1 + vec2
ans =
    5   16   40   14
```

The vector implicitly "expanded" to the same size as the matrix for element-wise arithmetic. If both elements are vectors, they will both be "expanded".

### Array Multiplication Operators

For operations that are based on multiplication, the operator must have a **dot** in front of it. So, the multiplication array operators are:

- `.^` - Element-wise power
- `.*` - Element-wise multiplication
- `./` - Element-wise right division
- `.\` - Element-wise left division

```matlab
>> vec2
vec2 =
    3    5    7    9

>> vec2 .^ 2
ans =
    9   25   49   81
```

```matlab
>> mata = [1 3; 2 1]
mata =
    1    3
    2    1

>> matb = [4 2; 1 5]
matb =
    4    2
    1    5

>> mata .* matb
ans =
    4    6
    2    5
```

### Understanding Array vs. Matrix Operations

MATLAB has two different types of arithmetic operations: array operations and matrix operations. Matrix operations follow the rules of linear algebra, while array operations execute element-by-element operations and support multidimensional arrays. The period character (.) distinguishes the array operations from the matrix operations. However, since the matrix and array operations are the same for addition and subtraction, the character pairs .+ and .- are unnecessary.

If operands have compatible sizes, then each input is implicitly expanded as needed to match the size of the other. For instance, if you add a 1-by-3 row vector to a 2-by-1 column vector, both vectors implicitly expand into a 2-by-3 matrix before MATLAB executes the element-wise addition.

---

## Introduction to Matrix Multiplication

You have done element-wise arithmetic with matrices, including multiplication (`.*`). Another way to multiply matrices is called **matrix multiplication** (`*`). You can think of it as a way to add elements in a row where you weight, or scale, the elements differently.

### Weighted Sums

A weighted sum is where you scale numbers and then add them together.

One example of a weighted sum is how some teachers calculate final grades. If tests are worth 50% of the final grade, homework is 40%, and quizzes are 10%, then a student's final grade is calculated by multiplying these proportions by the test, homework, and quiz grades and adding them together.

### Matrix-Vector Multiplication

Matrix multiplication of a matrix times a vector is the weighted sum of each row of the matrix. So, imagine a teacher records the test, homework, and quiz grades of different students in the rows of a matrix. Multiplying the matrix of grades by the vector of weights gives you the grades for each student.

The resulting matrix has the same number of rows as the matrix on the left — one final grade per student.

### Matrix-Matrix Multiplication

When you multiply two matrices, the elements of the resulting matrix are the weighted sums of the rows and columns of the first and second matrix, respectively. The matrix order is important because matrix multiplication is **not commutative**. That is, AB ≠ BA.

Another way of looking at matrix multiplication is that a matrix times a matrix is the left matrix multiplied by each column of the right matrix.

### Matrix Dimensions

Matrix multiplication requires that the dimensions agree. You need the same number of columns in the first matrix as there are rows in the second, or else there won't be enough elements to multiply together.

Matrix multiplication requires that the **inner dimensions** of the two matrices agree. The resulting matrix has the **outer dimensions** of the two matrices.

The number of columns in the first matrix, n, must equal the number of rows in the second matrix, n.

When these conditions are satisfied, you can do matrix multiplication. If `.*` is array multiplication, the matrix multiplication operator is just `*`.

```matlab
>> matc = mata * matb
matc =
    7   17
    9    9
```

---

## Logical Operators

Logical operators can be used with arrays, also.

```matlab
>> vec = randi([1,20], 1,5)
vec =
    16    15     8    14     4

>> isless10 = vec < 10
isless10 =
  1×5 logical array
   0     0     1     0     1
```

### Logical Indexing

Logical indexing also works in MATLAB:

```matlab
>> vec(isless10)
ans =
    8    4
```

```matlab
>> vec == vec
ans =
  1×5 logical array
   1     1     1     1     1
```

To find out just true/false whether two vectors are equal to each other, use `isequal`:

```matlab
>> isequal(vec, vec)
ans =
  logical
   1
```

---

## Arrays as Function Arguments

Entire arrays can be passed to functions. The function will evaluate each of the elements in the array and return an array with the same dimensions as the original.

```matlab
>> myvec = [-3: 2]
myvec =
    -3    -2    -1     0     1     2

>> abs(myvec)
ans =
     3     2     1     0     1     2
```

```matlab
>> mymat = randi([0, 5], 2, 4)
mymat =
     0     5     3     4
     2     2     1     1

>> sqrt(mymat)
ans =
         0    2.2361    1.7321    2.0000
    1.4142    1.4142    1.0000    1.0000
```

```matlab
>> x = linspace(-2*pi, 2*pi, 6);
>> y = sin(x)
y =
    0.0000    0.5878   -0.9511    0.9511   -0.5878   -0.0000
```

---

## Functions that Change Array Dimensions

### `reshape` Function

The `reshape` function can change the dimensions of any array to any other array that has the same number of elements.

```matlab
>> ranmat = randi([1,20], 2,4)
ranmat =
     9    16    14    17
    19    20     1    19

>> reshape(ranmat,4,2)
ans =
     9    14
    19     1
    16    17
    20    19
```

### `rot90` Function

The `rot90` function rotates a matrix 90 degrees counterclockwise.

```matlab
>> ranmat
ranmat =
     9    16    14    17
    19    20     1    19

>> rot90(ranmat)
ans =
    17    19
    14     1
    16    20
     9    19
```

### Flip Functions

There are several functions that flip matrices.

**`fliplr`** flips a row vector or the columns of a matrix from left to right:

```matlab
>> vec = 2:3:14
vec =
     2     5     8    11    14

>> fliplr(vec)
ans =
    14    11     8     5     2
```

**`flipud`** flips a column vector or the rows of a matrix up to down:

```matlab
>> vec = 2:3:14
vec =
     2     5     8    11    14

>> flipud(vec)
ans =
    14    11     8     5     2
```

**`flip`** flips a row vector left to right or a column vector or matrix up to down.

### Replication Functions

There are functions that replicate a matrix or element from a matrix.

**`repmat`** replicates an entire matrix. The following replicates a matrix variable `mymat` 6 times, as a 2 × 3 matrix of `mymat`s:

```matlab
>> mymat = [1 2; 3 4]
mymat =
     1     2
     3     4

>> repmat(mymat,2,3)
ans =
     1     2     1     2     1     2
     3     4     3     4     3     4
     1     2     1     2     1     2
     3     4     3     4     3     4
```

**`repelem`** replicates each element, in this case as a 2 × 3 matrix of each element:

```matlab
>> mymat = [1 2; 3 4]
mymat =
     1     2
     3     4

>> repelem(mymat, 2,3)
ans =
     1     1     1     2     2     2
     1     1     1     2     2     2
     3     3     3     4     4     4
     3     3     3     4     4     4
```

---

## Array Functions and Statistical Functions

There are functions that perform statistical analyses on vectors, including:

- **`min`**: minimum value
- **`max`**: maximum value
- **`mean`**: average
- **`mode`**: number that appears most frequently
- **`median`**: number in the middle of a sorted vector
- **`std`**: standard deviation
- **`var`**: variance
- **`sum`**: sum

For example:

```matlab
>> vec = [5 33 11 2 7 9 4]
vec =
     5    33    11     2     7     9     4

>> min(vec)
ans =
     2

>> mean(vec)
ans =
   10.1429
```

### Statistical Functions on Matrices

For matrices, all of these functions work **column-wise**:

```matlab
>> mat = randi([1, 10], 3, 5)
mat =
     4     4     7     7     1
     2     6     3     8     3
     8     2     7     5    10

>> max(mat)
ans =
     8     6     7     8    10

>> sum(mat)
ans =
    14    12    17    20    14
```

Notice that the result is a row vector that contains the result of the function for each column.

### Specifying Dimensions

Many statistical functions accept an optional dimensional argument that specifies whether the operation should be applied to the columns independently (the default) or to the rows.

To get the mean of each row, specify `2` as the second input:

```matlab
>> max(mat,2)
ans =
     7
     8
    10
```

To get an overall value, for example overall maximum, get the max of this row vector:

```matlab
>> max(max(mat))
ans =
    10

>> sum(sum(mat))
ans =
    77
```

You could also specify the operating dimension of the function as `"all"`:

```matlab
>> max(mat,"all")
ans =
    10
```

### Other Similar Functions

Other similar functions include:

- **`prod`**: product
- **`cumsum`**: cumulative sum
- **`cumprod`**: cumulative product
- **`cummin`**: cumulative minimum
- **`cummax`**: cumulative maximum

These can similarly work on a vector or the columns of a matrix.

```matlab
>> rvec = 2:5
rvec =
     2     3     4     5

>> prod(rvec)
ans =
   120

>> rvec = [rvec 11]
rvec =
     2     3     4     5    11

>> cumsum(rvec)
ans =
     2     5     9    14    25
```

```matlab
>> mat = randi([0,5], 3, 4)
mat =
     0     4     3     1
     1     3     1     4
     3     2     4     1

>> prod(mat)
ans =
     0    24    12     4

>> cummin(mat)
ans =
     0     4     3     1
     0     3     1     1
     0     2     1     1
```

### `diff` Function

The `diff` function returns differences between consecutive elements in a vector:

```matlab
>> vec = [5 33 11 2 7]
vec =
     5    33    11     2     7

>> diff(vec)
ans =
    28   -22    -9     5
```

Note that the result has one fewer element than the original vector.

### `sort` Function

The `sort` function sorts a vector, or columns of a matrix:

```matlab
>> mat
mat =
     0     4     3     1
     1     3     1     4
     3     2     4     1

>> sort(mat)
ans =
     0     2     1     1
     1     3     3     1
     3     4     4     4
```

---

## Square Matrices

There are functions that operate on square matrices.

### `diag` Function

The `diag` function returns the diagonal of a matrix, or creates a diagonal matrix by putting a vector on the diagonal:

```matlab
>> mydm = diag([2:4])
mydm =
     2     0     0
     0     3     0
     0     0     4

>> diag(mydm)
ans =
     2
     3
     4
```

### `trace` Function

The trace of a square matrix is the sum of the diagonal; the `trace` function will return this:

```matlab
>> trace(mydm)
ans =
     9
```

### `eye` Function

The `eye` function creates an Identity matrix:

```matlab
>> eye(4)
ans =
     1     0     0     0
     0     1     0     0
     0     0     1     0
     0     0     0     1
```

### "Is" Functions for Square Matrices

There are "is" functions that ask True/False questions about square matrices.

**`isdiag`** returns 1 for true if a matrix is a diagonal matrix:

```matlab
>> isdiag(mydm)
ans =
  logical
   1
```

**`issymmetric`** returns logical 1 if the matrix is a symmetric matrix:

```matlab
>> smat = [1 2 3; 2 7 4; 3 4 5]
smat =
     1     2     3
     2     7     4
     3     4     5

>> issymmetric(smat)
ans =
  logical
   1
```

---

## Key Takeaways

1. Scalar operations apply to every element of an array
2. Array operations use element-wise arithmetic with the dot operator (`.*, ./, .^`)
3. Matrix operations follow linear algebra rules and use operators without dots (`*, /, ^`)
4. MATLAB automatically expands arrays with compatible dimensions
5. Matrix multiplication requires inner dimensions to match (columns of first = rows of second)
6. Logical operators create logical arrays that can be used for indexing
7. Most statistical functions operate column-wise by default on matrices
8. Use dimension arguments (e.g., `2` for rows, `"all"` for entire array) to control function behavior
9. Functions like `reshape`, `flip`, and `repmat` modify array structure
10. Square matrix functions like `diag`, `trace`, and `eye` are useful for linear algebra

---

## Additional MATLAB Documentation Resources

For more detailed information about these functions and concepts, refer to the official MATLAB documentation:

- [Array vs. Matrix Operations - MATLAB & Simulink](https://www.mathworks.com/help/matlab/matlab_prog/array-vs-matrix-operations.html)
- [Reshape array by rearranging existing elements - MATLAB](https://www.mathworks.com/help/matlab/ref/reshape.html)
- [rot90 - Rotate CW 90 deg - MATLAB](https://www.mathworks.com/help/matlab/ref/rot90.html)
- [fliplr - Flip array left to right - MATLAB](https://www.mathworks.com/help/matlab/ref/fliplr.html)
- [flipud - Flip array up to down - MATLAB](https://www.mathworks.com/help/matlab/ref/flipud.html)
- [flip - Flip order of elements - MATLAB](https://www.mathworks.com/help/matlab/ref/flip.html)
- [repmat - Repeat copies of array - MATLAB](https://www.mathworks.com/help/matlab/ref/repmat.html)
- [repelem - Repeat copies of array elements - MATLAB](https://www.mathworks.com/help/matlab/ref/repelem.html)
- [min - Minimum elements of array - MATLAB](https://www.mathworks.com/help/matlab/ref/min.html)
- [max - Maximum elements of array - MATLAB](https://www.mathworks.com/help/matlab/ref/max.html)
- [mean - Average or mean value of array - MATLAB](https://www.mathworks.com/help/matlab/ref/mean.html)
- [mode - Most frequent values in array - MATLAB](https://www.mathworks.com/help/matlab/ref/mode.html)
- [median - Median value of array - MATLAB](https://www.mathworks.com/help/matlab/ref/median.html)
- [std - Standard deviation - MATLAB](https://www.mathworks.com/help/matlab/ref/std.html)
- [var - Variance - MATLAB](https://www.mathworks.com/help/matlab/ref/var.html)
- [sum - Sum of array elements - MATLAB](https://www.mathworks.com/help/matlab/ref/sum.html)
- [prod - Product of array elements - MATLAB](https://www.mathworks.com/help/matlab/ref/prod.html)
- [cumsum - Cumulative sum - MATLAB](https://www.mathworks.com/help/matlab/ref/cumsum.html)
- [cumprod - Cumulative product - MATLAB](https://www.mathworks.com/help/matlab/ref/cumprod.html)
- [cummin - Cumulative minimum - MATLAB](https://www.mathworks.com/help/matlab/ref/cummin.html)
- [cummax - Cumulative maximum - MATLAB](https://www.mathworks.com/help/matlab/ref/cummax.html)
- [sort - Sort array elements - MATLAB](https://www.mathworks.com/help/matlab/ref/sort.html)
- [diag - Create diagonal matrix or get diagonal elements of matrix - MATLAB](https://www.mathworks.com/help/matlab/ref/diag.html)
- [isdiag - Determine if matrix is diagonal - MATLAB](https://www.mathworks.com/help/matlab/ref/isdiag.html)
- [trace - Sum of diagonal elements - MATLAB](https://www.mathworks.com/help/matlab/ref/trace.html)
- [eye - Identity matrix - MATLAB](https://www.mathworks.com/help/matlab/ref/eye.html)
- [issymmetric - Determine if matrix is symmetric or skew-symmetric - MATLAB](https://www.mathworks.com/help/matlab/ref/issymmetric.html)
- [movmean - Moving mean - MATLAB](https://www.mathworks.com/help/matlab/ref/movmean.html)