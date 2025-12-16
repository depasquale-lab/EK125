# Week4B Preread: Advanced Loops using List Comprehensions

## Range and enumerate

We have seen that the range function generates a sequence of integers and that when called as `range(n)`, it generates the sequence of integers from 0 through n-1.  The type returned from range is `range`, although it can be passed to the list function if a list consisting of the individual items is desired.

```python
>>> mr = range(6)
>>> print(mr, type(mr))
range(0, 6) <class 'range'>
```

```python
>>> ml = list(mr)
>>> print(ml, type(ml))
[0, 1, 2, 3, 4, 5] <class 'list'>
```

So, again, `range` does not create a list; it creates an `iterator`.  An interesting aspect of iterators is that the values are created one at a time; the entire iterator is not created first.

The built-in function `iter` will determine whether an argument can be iterated through or not, and if so what type of iterator the argument is. For example, the range variable `mr` is a range iterator, a list `[3, 66]` is a list iterator, and an integer is not an iterator.

```python
>>> iter(mr)
<range_iterator at 0x7f888bb480f0>
```

```python
>>> iter([3, 66])
<list_iterator at 0x7f888bb48070>
```

```python
>>> iter(33)
TypeError: 'int' object is not iterable
```

By storing the iterator object in a variable, the `next` function returns the next value in the iterator, e.g.

```python
>>> imr = iter(mr)
```

```python
>>> next(imr)
0
>>> next(imr)
1
>>> next(imr)
2
```

When there are no more values, an error message is thrown if `next` is called again.

If two arguments are passed to the `range` function, they are the first value (instead of the default of 0) and the last (minus 1).  For example, `range(3,7)` generates the sequence of integers 3, 4, 5, 6.

```python
>>> mr = range(3,7)
>>> ml = list(mr)
>>> print(ml)
[3, 4, 5, 6]
```

An integer "step" value can also be specified as a third argument (instead of the default of 1).

```python
>>> mr = range(3,10,2)
>>> ml = list(mr)
>>> print(ml)
[3, 5, 7, 9]
```

Step values can also be negative.

```python
>>> ml = list(range(10,2,-2))
>>> print(ml)
[10, 8, 6, 4]
```

The `range` function is frequently used with `for` loops to specify how many times to execute the action of a loop.  More generally, the form of a `for` loop is

```python
for itervar in iterator:
    action
```

The `enumerate` function can be used to return both an index and an item from a sequence such as a list.

```python
numlist = [4, 52, 33, 11, -3]
for i, item in enumerate(numlist):
print('Item', i, 'is', item)
Item 0 is 4
Item 1 is 52
Item 2 is 33
Item 3 is 11
Item 4 is -3
```

Notice that this gives us two iterator variables: `i`, which is the index, and then `item`, which is the value of that item in the list.

## Resurrecting Tuples

We talked about tuples in Week 1 and 2, but we haven't used them much. Let's have a little tuple refresher, and we'll introduce a few points about tuples that we have not yet discussed. 

Tuples are similar to lists, but are **immutable**. Tuples are generally created by putting values in parentheses, separated by commas.

```python
>>> mytup = (2, 11, 33)
```

Functions such as `len` and indexing/slicing work the same on tuples as on lists.

```python
>>> len(mytup)
3
```

```python
>>> mytup[1]
11
```

Parentheses are not always necessary:

```python
>>> newtuple = 5, 19
>>> newtuple
(5, 19)
```

The concatenation operator can be used to join two tuples together.

```python
>>> mytup + newtuple
(2, 11, 33, 5, 19)
```

An empty tuple is created using parentheses with nothing inside, e.g.:

```python
>>> emptup = ()
>>> len(emptup)
0
```

To create a tuple with one entry, the value must be followed by a comma, and both must be in parentheses.

```python
>>> onetup = (7,)
>>> onetup
(7,)
```

The parentheses and comma here are necessary. If the comma is omitted, the value is just an integer:

```python
>>> print(type((7,)), type((7)))
<class 'tuple'> <class 'int'>
```

The type of a tuple is `tuple`, as shown. Because of this, indexing into a tuple to get one value is not quite the same as slicing to get one value.  A slice returns another tuple, even if it only has a length of 1:

```python
>>> mytup = (2, 11, 33)
>>> mytup[0:1]
(2,)
```

Indexing using `0` returns just the integer 2:

```python
>>> mytup[0]
2
```

The `list` function can be used to create a list from a tuple:

```python
>>> tl = list(mytup)
>>> tl
[2, 11, 33]
```

The `tuple` function can be used to create a tuple from another sequence type such as a string or a list:

```python
>>> wordlist = ['howdy', 'hi']
>>> wordtuple = tuple(wordlist)
>>> chartuple = tuple(wordlist[0])
>>> print(wordlist, wordtuple, chartuple)
['howdy', 'hi'] ('howdy', 'hi') ('h', 'o', 'w', 'd', 'y')
```

Putting values into a tuple is called "packing", as in:

```python
>>> mytup = 2, 11, 33
```
or

```python
>>> mytup = (2, 11, 33)
```

The reverse is called "unpacking". In order to unpack a tuple, it is necessary to know how many values are in the tuple and to have that many variables on the left-hand side of the assignment operator.

```python
>>> a, b, c = mytup
>>> print(mytup, a, b, c)
(2, 11, 33) 2 11 33
```

Unpacking is also possible for other sequence types such as `lists` and `strings`.

## For Loops and Vectorized Code

In general, in coding, `for` loops are used to iterate through the indices of data structures such as Python sequences in order to perform the same operation on every element. As we've seen, in Python, the general form of this is for a sequence `seq` is:

```python
for i in range(len(seq)):
    # do something with seq[i]
```

In Python, rather than iterating through the indices of the sequence, it is possible to iterate through the **sequence itself**, as in:

```python
for item in seq:
    # do something with item
```

**Vectorizing code** means getting rid of loops, and instead using built-in functions and operators.

This can involve using operators such as the `*` concatenation operator, functions such as `min`, `max`, `sum`, and `sort`, and methods such as `count` and `reverse`. We've used lots of these already!

Another powerful way to vectorize Python code when creating lists is to use **list comprehensions**.

Comprehensions do not add any power to the language, but provide a convenient, succinct way to create sequences. A comprehension essentially compresses using a `for` loop to create and add expressions to a sequence into one line.

List comprehensions are the most common, but it is also possible to create other comprehensions.

The simplest general form of a list comprehension is
```python
[expression for i in iterable]
```

which creates a list of the expressions for all values of `i`.  For example,

```python
>>> [i ** 3 for i in range(5)]
[0, 1, 8, 27, 64]
```

This creates a list of the cubes of all values of `i` in the range from 0 to 4 inclusive. Assigning this to a list variable

```python
>>> cubelist = [i**3 for i in range(5)]
```

is equivalent to

```python
cubelist = []
for i in range(5):
    cubelist.append(i**3)
```

Conditionals (such as `if-else`) can be added to the list comprehension to determine which expressions based on the iterator variable to include in the list.  For example,

```python
>>> cubelisteven = [i**3 for i in range(7) if i%2==0]
```

creates a list of cubes of the even integers in the range from 0 to 6 inclusive, and is equivalent to:

```python
cubelisteven = []
for i in range(7):
    if i%2 == 0:
        cubelisteven.append(i**3)
```