# Week 6B Preread: Digging into arrays with NumPy

## Overview

The NumPy module is used extensively by Python programmers for mathematical and scientific computing. Most of the NumPy library is based on arrays, which are data structures that store values (elements) that are all the same type.  Typically the elements in arrays are numbers, either integers or floats. The numpy module has many built-in functions and the array objects have many associated methods for working with numbers to facilitate applications such as linear algebra and statistics.  Many data science applications work extensively with the numpy module.

Since it is a module, numpy must first be imported. It is very common to rename it as np:

```python
>>> import numpy as np
```

The rest of this chapter will assume that the numpy module has been imported in this manner.

## Array Basics

Arrays have dimensions. An array with one dimension is either a row or a column. Sometimes these are called vectors, either a row vector or a column vector.  Like other sequences in Python such as lists, arrays can be indexed, and the indices begin at 0.

A one dimensional array which is a row vector containing 4 elements might be depicted as:

| Index | 0  | 1  | 2  | 3  |
|-------|----|----|----|----|
| Value | 33 | 11 | -5 | 14 |

In NumPy, the dimensions are called axes, so this is an array with one axis. We say that this is a 1 x 4 (read “1 by 4”) array; it has 1 row and 4 columns.  The length of the array is the number of elements, which is 4.

A one dimensional column vector might be depicted as:

| Index | Value |
|-------|-------|
| 0     | 47    |
| 1     | 6     |
| 2     | 15    |

Again, this is an array with one axis. We say that it is a 3 x 1 array; it has 3 rows and 1 column. The length of this array is 3.

Elements in a one dimensional array are found using one index.

A two dimensional array has two axes. A two dimensional array looks like a table, with both rows and columns.  For example, a 2 x 4 array (2 rows, 4 columns) might be depicted as:

| Index | 0  | 1  | 2  | 3  |
|-------|----|----|----|----|
| 0 | -2 | 18 | 9 | 32 |
| 1 | 44 | -7 | 4 | 99 |

The first axis is the rows, so the length of the first axis is 2. The second axis is the columns, so the length of the second axis is 4.  Two dimensional arrays have indices for both the rows and the columns, and both start at 0. Elements are indexed using two indices, the first for the row, and the second for the column.

In general, an n-dimensional array has n dimensions, or axes. Elements are indexed using n indices. Arrays with two or more dimensions are frequently called matrices.

A scalar, which is a single value, is said to have a dimension of 0 in NumPy. Scalars are not indexed.

## Creating Arrays and Subarrays
### The array function and attributes of arrays

The basic function that is used to create arrays is the function ndarray, but this function has a simpler alias called array, which we will use.  One method for creating a one dimensional array is to convert a list (or a tuple) to an array using the array function. For the rest of this chapter, we will assume that the array function has been imported.

```python
>>> from numpy import array
>>> arr1d = array([31, 42, 11])
```

Note how the array is displayed by just typing the variable name and by using the print function, and note the type of the array.

```python
>>> arr1d
array([ 31, 42, 11])
```

```
>>> print(arr1d, type(arr1d))
[31 42 11] <class 'numpy.ndarray'>
```

The type of the data elements can be determined using the `dtype` method.

```python
>>> print(arr1d.dtype)
int64
```

The type `int64` is an integer type that uses 64 bits to store integers.

There are several methods that return the number of dimensions, and the lengths of the axes.  The ndim method returns the number of dimensions, or in other words the number of axes.

```python
>>> arr1d.ndim
1
```

The lengths of the axes are found using the shape method.

```python
>>> arr1d.shape
(3,)
```

In this case, there is only one axis since it is a one-dimensional array, and the length is 3. Notice that the result is returned as a tuple.

The size of an array is the number of elements. This is always going to be a single integer.

```python
>>> arr1d.size
3
```

Two dimensional arrays can be created by passing a nested list (a list of lists) to the array function. In the following example, two lists, each with 3 numbers, are used to create a 2 x 3 array.

```python
>>> arr2d = array([[1, 3, 8], [-2, 5, 33]])
>>> print(arr2d)
[[ 1  3  8]
[-2  5 33]]
```

The two nested lists must have the same number of elements, so there are the same number of elements in each row of the array.  The number of dimensions is 2. The shape function returns the tuple (2, 3) since it is a 2 x 3 array, and the size (the total number of elements) is 6.

```python
>>> arr2d.ndim
2
```
```python
>>> arr2d.shape
(2, 3)
```
```python
>>> arr2d.size
6
```

### Functions and methods that create arrays

There are quite a few ways in which arrays can be created.  Arrays can be created using a range:

```python
>>> rgarr = array(range(5))
>>> print(rgarr)
[0 1 2 3 4]
```

The numpy function arange specifically creates an array using a range.

```python
>>> rgarr = np.arange(5)
>>> print(rgarr)
[0 1 2 3 4]
```

Like the `range` function, arange can specify starting, ending, and step values.

```python
>>> myarr = np.arange(3,9,2)
>>> print(myarr)
[3 5 7]
```

There are several other functions that can be used to create arrays.

The `linspace` function can be used to create array elements that are linearly spaced within a specified range. The following specifies that the range begins with 1, ends with 9, with 3 linearly spaced points. The linspace function determines the step value (in this case, 4). Notice that the default type returned by `linspace` is float64, even if the numbers are integers.

```python
>>> myarr = np.linspace(1,9,3)
>>> print(myarr, myarr.dtype)
[1. 5. 9.] float64
```

If the number of elements is not specified, the default is 50. Also, the type can be specified using `dtype`.

```python
>>> myarr = np.linspace(1,9,3,dtype=np.int16)
>>> print(myarr, myarr.dtype)
[1 5 9] int16
```

The type `int16` is an integer type that uses 16 bits, so it stores smaller integers than the type int64.

An array of all zeros can be created using the `zeros` function, by specifying the shape. The default type of each element is float64, but like the linspace function, this can be modified using dtype.

```python
>>> print(np.zeros((2,4), dtype=int))
[[0 0 0 0]
[0 0 0 0]]
```

If only one integer `n` is specified for the shape, a 1 x n row vector is created.

```python
>>> zarr = np.zeros(3)
>>> print(zarr)
[0. 0. 0.]
```

Three dimensional arrays can be created by specifying a tuple of 3 integers for the shape. For example, a shape of `(2, 3, 5)` creates two 3 x 5 arrays. Notice that when displayed, the two arrays are printed separately with a blank line in between them.

```python
>>> print(np.zeros((2,3,5), dtype=np.int16))
[[[0 0 0 0 0]
[0 0 0 0 0]
[0 0 0 0 0]]

[[0 0 0 0 0]
[0 0 0 0 0]
[0 0 0 0 0]]]
```

Images are often stored as three dimensional arrays.  Arrays with more than 3 dimensions can also be created, but it becomes more difficult to visualize them and to find useful applications for them.

One reason for creating an array of all zeros might be if they are going to be running sums or counters.

Arrays of all ones can be created using the ones function, which uses the same format as the zeros function.  An array of all ones might be used as a starting point for running products.

```python
>>> print(np.ones(6, dtype=int))
[1 1 1 1 1 1]
```

To fill in an array with any number other than zeros or ones, the full function can be used.

```python
>>> print(np.full((2,3), 11))
[[11 11 11]
[11 11 11]]
```

Instead of just a single number, a set of numbers to be used for each row can be passed to the full function, for example using range.

```python
>>> newmat = np.full((3,4), range(4))
>>> print(newmat)
[[0 1 2 3]
[0 1 2 3]
[0 1 2 3]]
```

Another function that creates arrays is the `empty` function, which contrary to its name does not create an empty array, but fills in the elements with whatever is currently in the memory locations used for the array. This function is useful when the shape of an array is known, but the actual values in the elements will be filled in later.  Of course, your results may vary!

```python
>>> weird = np.empty(4)
>>> print(weird)
[-2.68156159e+154 -3.11108761e+231  2.68678769e+154  2.82470694e-309]
```

It is frequently useful to create arrays in which random numbers are stored in the elements. NumPy has a random module that has a method, `default_rng`, which is used to create a random number generator. Once that has been created, the method random can be used to create random floats, and the method integers can be used to create random integers in a specified range.

An integer can be passed to default_rng which is the seed for the random number generator; if no argument is passed the seed will be obtained from the operating system.

```python
>>> rng = np.random.default_rng()
>>> farr = rng.random(3)
>>> print(farr)
[0.74899722 0.9414599  0.70685837]
```

In the following example, a 1 x 4 array of random integers is created in the range from 3 to 9 inclusive.

```python
>>> rng.integers(3,10,4)
array([9, 6, 3, 9])
```

### Indexing and Slicing

Arrays can be indexed and sliced similarly to Python sequences such as lists. The indices begin at 0, and negative indices allow indexing from the end of the array.

```python
>>> arr1d = array([31, 42, 11])
>>> arr1d[1]
42
```

```python
>>> arr1d[-1]
11
```

With two dimensional arrays, the row index is given first, and then the column index.

```python
>>> arr2d = array([[1, 3, 8], [-2, 5, 33]])
>>> print(arr2d)
[[ 1  3  8]
[-2  5 33]]
```

```python
>>> arr2d[1,2]
33
```

Slicing creates another array.

```python
>>> arr1d[:]
array([31, 42, 11])
```

```python
>>> arr2d[0,1:]
array([3, 8])
```

In addition to using integer indices, NumPy allows logical indexing.  With logical indexing, Boolean expressions resulting in `True` or `False` can be used to index into an array. This returns the elements from the array for which the corresponding element in the logical array is True.

```python
>>> logarr = arr1d > 20
>>> logarr
array([ True,  True, False])
```
```python
>>> arr1d[logarr]
array([31, 42])
```

Notice that for a two dimensional array, with logical indexing the resulting  array is flattened into a one dimensional array.  The elements are taken from the original array one row at a time.

```python
>>> print(arr2d)
[[ 1  3  8]
[-2  5 33]]
```
```python
>>> arr2d[arr2d > 0]
array([ 1,  3,  8,  5, 33])
``` 

### Multidimensional Slicing

Just like lists, NumPy arrays can be sliced using the `:` operator. But because arrays can have multiple axes (dimensions), NumPy allows **slicing along each axis** simultaneously using a comma-separated list of slice specifications.  

For a 2 × 4 array:

```python
arr2d = np.array([
    [-2, 18,  9, 32],
    [44, -7,  4, 99]
])
print(arr2d)
```
```
# [[-2 18  9 32]
#  [44 -7  4 99]]
```

The **first index** corresponds to the **row**, and the **second index** to the **column**.

#### Slicing Rows and Columns

```python
# First row (row 0), all columns
print(arr2d[0, :])
# → [-2 18  9 32]
```

```python
# All rows, second column (column 1)
print(arr2d[:, 1])
# → [18 -7]
```

#### Slicing Subarrays

You can extract rectangular blocks by slicing along both axes:

```python
# Rows 0 through 1, columns 1 through 2
sub = arr2d[0:2, 1:3]
print(sub)
```
```
# Output:
# [[18  9]
#  [-7  4]]
```

This is a **2 × 2 subarray**, taken from the middle of `arr2d`.

#### Including a Step Size

NumPy slicing follows the pattern:

```python
# start : stop : step
```

You can include a **step size** along any axis. For example:

```python
# Every other column from all rows
print(arr2d[:, ::2])
```
```
# Output:
# [[-2  9]
#  [44  4]]
```
```python
# Take every row, but every other column starting at index 1
print(arr2d[:, 1::2])
```
```
# Output:
# [[ 18  32]
#  [ -7  99]]
```

#### 3D Arrays

The same slicing logic extends naturally to higher dimensions. For example, with a 3D array shaped `(2, 3, 4)`:
```python
arr3d = np.arange(24).reshape(2, 3, 4)
print(arr3d.shape)  # (2, 3, 4)
```

```python
# Take the first "layer" (axis 0)
print(arr3d[0, :, :])
```
```python
# Take all layers, first row, columns 1–3
print(arr3d[:, 0, 1:4])
```

```python
# Take every other column in every row of the first layer
print(arr3d[0, :, ::2])
```

Think of each comma as moving to the next axis. This makes multidimensional slicing a powerful way to extract structured portions of data—like rows, columns, planes, or sub-blocks—without looping.


### Changing Dimensions

There are NumPy methods that change the shape of an array.

The reshape method can take any array and reshape it into a new array, as long as the new array has the same number of elements as the original. For example, a 1 x 6 array could be reshaped into a 6 x 1 array, a 2 x 3 array, or a 3 x 2 array.

```python
>>> myarr = np.arange(6)
>>> print(myarr)
[0 1 2 3 4 5]
```
```python
>>> myarr.reshape(6,1)
array([[0],
[1],
[2],
[3],
[4],
[5]])
```

```python
>>> arr2by3 = myarr.reshape(2,3)
>>> print(arr2by3)
[[0 1 2]
[3 4 5]]
```

The reshape method returns a reshaped array, but does not modify the original array. The resize method works just like reshape except that it does modify the original array.

```python
>>> myarr = np.arange(6)
>>> myarr.resize(3,2)
>>> print(myarr)
[[0 1]
[2 3]
[4 5]]
```

The `ravel` method flattens an n-dimensional array into a one dimensional array. The default is that it flattens it one row at a time. The ravel method returns a flattened array, but does not modify the original.

```python
>>> print(arr2by3)
[[0 1 2]
[3 4 5]]
```

```python
>>> arr2by3.ravel()
array([0, 1, 2, 3, 4, 5])
```

```python
>>> print(arr2by3)
[[0 1 2]
[3 4 5]]
```

The method named simply `T` transposes an array, which for a two dimensional array means that it interchanges the rows and columns. The method returns a new array, but does not modify the original. In the following example, the first row [0 1 2] becomes the first column, and the second row becomes the second column. The other way to view it is that the first column becomes the first row, the second column becomes the second row, and the third column becomes the third row.

```python
>>> arr3by2 = arr2by3.T
>>> print(arr3by2)
[[0 3]
[1 4]
[2 5]]
```

### Combining Arrays

There are several functions that create new arrays by combining existing arrays.

The function vstack vertically stacks, or concatenates, arrays. A tuple consisting of the arrays to be stacked is passed to the vstack function.  In this case the one-dimensional arrays have to have the same number of elements so that the columns will match up in the stacked array.

```python
>>> arrone = np.array([4,11,9])
>>> arrtwo = np.array([3,15,22])
>>> np.vstack((arrone, arrtwo))
array([[ 4, 11,  9],
[ 3, 15, 22]])
```

The function hstack horizontally stacks arrays.

```python
>>> np.hstack((arrtwo,arrone))
array([ 3, 15, 22,  4, 11,  9])
```

## Functions and Operations on Arrays

NumPy has functions that take arrays as arguments, and operations that can be performed on entire arrays.

### Functions

NumPy has universal functions (`ufuncs`) to which an entire array can be passed. The function is evaluated on every element, returning an array with the same dimensions as the original.  These include trig functions such as `sin`, `cos`, `tan`, etc. For example:

```python
from math import pi
x = np.linspace(0, 2*pi, 10)
y = np.sin(x)
print(x)
print(y)
```
```
[0.      0.6981317  1.3962634  2.0943951  2.7925268  3.4906585
4.1887902  4.88692191 5.58505361 6.28318531]
[ 0.00000000e+00  6.42787610e-01  9.84807753e-01  8.66025404e-01
3.42020143e-01 -3.42020143e-01 -8.66025404e-01 -9.84807753e-01
-6.42787610e-01 -2.44929360e-16]
```

Other `ufuncs` include the square root function `sqrt`, the absolute value function `abs`, and exponential functions.  For example, we could get the absolute value of every element in a 2D array:

```python
rng = np.random.default_rng()
nums = rng.integers(-10,10, (3,5))
print(nums)
print()
print(np.abs(nums))
```
```python
[[  1  -1   7 -10   3]
[  6   2   6  -5  -3]
[ -1   1 -10  -6   9]]

[[ 1  1  7 10  3]
[ 6  2  6  5  3]
[ 1  1 10  6  9]]
```

There are aggregation functions that perform tasks such as summing elements, multiplying elements, and finding minimum and maximum values. By default, the aggregation is performed on the entire array, but to aggregate on just rows or columns, the axis can be specified.
For example, let’s first create a 2D array:

```python
>>> rng = np.random.default_rng()
>>> nums = rng.integers(60, 100, (4,2))
>>> print(nums)
```
```
[[96 91]
[68 64]
[60 97]
[77 77]]
```

To find the overall minimum, the min function is used:

```python
>>> np.min(nums)
60
```

To find the minimum for each column, specify axis 0:

```python
>>> np.min(nums, axis = 0)
array([60, 64])
```

To find the minimum for each row, specify axis 1:

```python
>>> smin = np.min(nums, axis = 1)
>>> smin
array([91, 64, 60, 77])
```

Other unfuncs that work the same way (overall, or specify the axis) include `max`, `mean`, `median`, `std` (standard deviation), and `var` (variance).

The aggregation functions also include `sum` and `prod`.  For example, to get an overall sum:

```python
>>> rng = np.random.default_rng()
>>> nums = rng.integers(0,10, (3,4))
>>> print(nums)
[[3 9 7 3]
[1 0 2 8]
[2 0 0 1]]
```

```python
>>> np.sum(nums)
36
```

To get a sum for every column:

```python
>>> np.sum(nums, axis = 0)
array([ 6,  9,  9, 12])
```

### Operations

Numerical operations can be performed on entire arrays. Scalar operations involve an array and one scalar.  For example, to add 5 to every element in an array:

```python
>>> rowvec = array([-3, 28, 6])
>>> rowvec + 5
array([ 2, 33, 11])
```

Operations such as addition, subtraction, multiplication, division, and exponentiation can be performed on arrays of any dimension.  For example, to divide every element in a 2D array by 2:

```python
rng = np.random.default_rng()
nums = rng.integers(10,20, (2,5))
print(nums)
print()
print(nums/2)
```

```
[[18 17 13 11 15]
[14 12 13 12 16]]

[[9.  8.5 6.5 5.5 7.5]
[7.  6.  6.5 6.  8. ]]
```

Array operations are operations that are performed element by element on two arrays that have the same dimensions.

For example, the following creates two 1 x 5 arrays, or vectors, and adds the corresponding elements to create another 1 x 5 array. This is array addition.

```python
>>> vec1 = array([6, 2, 7, 5, 9])
>>> vec2 = array([3, 1, 4, 2, 1])
>>> vec1 + vec2
array([ 9,  3, 11,  7, 10])
```

Similarly, array multiplication is an element-by-element operation:

```python
rng = np.random.default_rng()
mat1 = rng.integers(0,10, (2,3))
print(mat1)
print()
mat2 = rng.integers(0,10, (2,3))
print(mat2)
print()
print(mat1*mat2)
```
```
[[3 9 3]
[4 0 0]]

[[4 2 6]
[8 1 3]]

[[12 18 18]
[32  0  0]]
```

Note: this is array multiplication, not matrix multiplication.  Matrix multiplication is not performed element-by-element, and will be described later in the course.