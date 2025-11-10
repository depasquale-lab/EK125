# Weeks 5-10 GPP Review

Here are a selected (and slighlty edited if necessary) set of GPPs from the past 5 weeks. 

**NOTE**: These were selected with an eye toward covering all of the material we have worked on over the last 5 weeks. But inclusion of material here does not suggest that material NOT covered here will NOT be on the test! This document if designed to get you practicing on GPP problems away from the computer. We recommend doing this for ALL GPP problems!

# Week5A: Nested lists and Multilevel Indexing

## Ice cream combinations with nested list comprehensions

An ice cream shop wants to test every possible combination of ice cream flavors and toppings for a new menu.  
Some combos are disallowed because they would be gross (for example: `"oreo"` with `"peach"`).  

Your task is to generate the combinations using a **nested list comprehension**.  
That means the result should be a **list of lists**, where each sublist contains all the valid `(flavor, topping)` pairs for one particular flavor.  

**Tasks**  

1. **Small test case**  
   - Manually create two short lists:  
     ```python
     flavors = ["vanilla", "chocolate", "strawberry"]
     toppings = ["sprinkles", "fudge", "nuts"]
     ```  
   - Use a nested list comprehension to generate all `(flavor, topping)` pairs, but **structure the result by flavor** (i.e., each flavor has its own sublist of topping pairs). Call this nested list `combos_small`.
   - Exclude one specific bad combo (e.g., `"strawberry"` with `"fudge"`).  

2. **Large test case**  
   - Pretend the shop has 200 flavors and 200 toppings.  
     ```python
     flavors = list(range(200))   # 0, 1, 2, ..., 199
     toppings = list(range(200))
     ```  
   - Use a nested list comprehension to generate a list of 200 sublists.  
   - Each sublist should contain all the valid `(flavor, topping)` pairs for that flavor.  
   - Exclude pairs where `(flavor + topping) % 17 == 0`.

# Week 6A: Mastering Slicing in Python
## Quick Slicing Drills

Complete these quick exercises to practice basic slicing. Write each answer on one line.


```python
# Given these variables:
word = "Python"
numbers = [10, 20, 30, 40, 50, 60, 70, 80, 90]
```

**Write slices to extract:**

1. `"Pyt"` from word
2. `"hon"` from word
3. `"th"` from word
4. `[10, 20, 30]` from numbers
5. `[40, 50, 60]` from numbers
6. `[70, 80, 90]` from numbers
7. `[10, 30, 50, 70, 90]` from numbers (every other, starting at 0)
8. `[20, 40, 60, 80]` from numbers (every other, starting at 1)
9. The last element of numbers as a single value
10. The last 3 elements of numbers as a list

## More Quick Drills: Negative Indices

```python
data = [5, 10, 15, 20, 25, 30, 35, 40]
text = "HelloWorld"
```

**Write slices to extract:**

1. `[30, 35, 40]` using negative indices
2. `[5, 10, 15, 20, 25]` (all but last 3) using negative indices
3. `"World"` from text using negative indices
4. `"Hello"` from text using negative indices
5. `[40]` as a list using negative indices
6. `40` as a single value using negative index

## Problem 1: Basic Slice Practice

Write a program that demonstrates basic slicing on a word.

**Task:** Ask the user for a word, then print:
1. The first 3 characters
2. The last 3 characters
3. Characters from index 2 to 5
4. Every other character

**Example:**
```
Enter a word: Programming
First 3 characters: Pro
Last 3 characters: ing
Characters 2 to 5: ogr
Every other character: Pormig
```

# Week 6B: Working with arrays in NumPy

## Problem 2: Arrays, types and dtypes

- Create a one-dimensional array variable using the array function, in which 5 integers are stored.
- Also create a 1D array variable that stores 6 floats.
- Print both arrays and their types.
- Use the dtype method to determine the type of both array variables. Note that no arguments are passed to dtype, and the parentheses are not needed.

## Problem 4: 2D arrays

- Create a nested list variable that stores integers: two inner lists, each of which stores 4 integers.
- Then, create a 2 by 4 array variable by passing the list to the array function.  
- Print both variables so you can see the difference.
- Use the `ndim`, `shape`, and `size` methods on your 2D array variable.
- Use the `reshape` method to reshape it into as many other arrays as you can.
- Use the `transpose` method to transpose your array.
- Indexing and slice your 2D array:
  - Index element at row 0, column 0  
  - Index element at row 0, column 1  
  - Slice row 0, last column
  - Slice all columns from row 1  
  - Slice all rows, column 0  
  - Slice all rows, first two columns
- Replace the first row of your 2D array with other numbers.

## Problem 6: Special Arrays

- Create and print a 3 by 5 array of all zeros (integers).
- Create and print a 4 by 2 array of all ones (integers).
- Create and print a 3 by 5 array of all 33's using `np.full`
- Create a 3D array of all zeros. Print it.

## Problem 8: Logicals in arrays

Using an array in a Boolean expression results in a logical array.

- Create a 1D array variable `my1d` and try the expression `my1d < 10`. (Store it in a variable and print it, as shown below

```python
my1d = array([33, 2, 11, 5, 8, 4])
mylog = my1d < 10
print(mylog)
```
- Now, use your logical array to index into your original 1D array variable.

```python
my1d[mylog]**bold text**
```

# Week 7A: Python dictionaries

## Problem 2: Book information

You are tasked with storing information about a book in a dictionary. Perform the following steps:

1. Create a dictionary named `book` with the following key-value pairs:
- `'title': 'To Kill a Mockingbird'`
- `'author': 'Harper Lee'`
- `'year_published': 1960`
- `'genre': 'Fiction'`
2. Print the value associated with the key `'author'`.
3. Check if the key `'pages'` exists in the dictionary and print the result. (Hint: use `in`!)
4. Use the `len()` function to determine the number of key-value pairs in the dictionary and print it.

## Problem 3: `get()` and `pop()` on a smartphone dictionary

You are given a dictionary representing a smartphone:

```python
smartphone = {
    'brand': 'Apple',
    'model': 'iPhone 13',
    'year_released': 2021,
    'price': 799
}
```
1. Use the `get` method to retrieve the value for the key `'price'`.
2. Add a new key-value pair: `'storage': '128GB'`.
3. Update the `'price'` to `749`.
4. Remove the `'year_released'` key using the `pop()` method.
5. Print the updated dictionary.

## Problem 5: `.keys()`, `.values()` and `.items()`

You are given a dictionary representing the stock of items in a grocery store:

```python
store_stock = {
    'apples': 50,
    'bananas': 30,
    'milk': 20,
    'bread': 15
}
```

1. Use the `.keys()` method to print a list of all items in the store.
2. Use the `.values()` method to calculate and print the total stock of all items.
3. Use the `.items()` method to print each item and its stock as tuples.
1. Use a loop to print each item and its stock in the format: `"Item: <item>, Stock: <stock>"`.

# Week 7B: User-defined functions

## Problem 1.3

Write a function that will prompt the user for a float in a range. The minimum and maximum values in the range will be passed to the function, in any order. The function should loop to error-check until the user enters a valid number. Make sure that you test your function thoroughly.

## Problem 2.1

Write a function that will print a box of *'s. Two arguments will be passed to the function: the number of rows to print, and the number of columns (the number of *'s in each row).

## Problem 3.2

Write a cumulative product function. The function will receive a list of numbers, and will create a new list which is a list of the running products of the numbers in the parameter list. For example, if the list that is passed to the function is `[4, 2, 5]`, the returned list would be `[4, 8, 40]`.

# Week 8A: Argument passing, location, scope

## Exercise 1.1: Predict the Output
Before running any code, predict what each snippet will print. Then run it to check.

```python
# Snippet A
def updateScore(score):
    score = score + 10
    print(f"Inside function: {score}")

playerScore = 50
updateScore(playerScore)
print(f"Outside function: {playerScore}")
```
**Your prediction:**
- Inside function: ___________
- Outside function: ___________

**Actual result:** ___________

**Explanation (why did this happen?):**

```python
# Snippet B
def updateScores(scores):
    scores.append(100)
    print(f"Inside function: {scores}")

playerScores = [85, 90, 78]
updateScores(playerScores)
print(f"Outside function: {playerScores}")
```
**Your prediction:**
- Inside function: ___________
- Outside function: ___________

**Actual result:** ___________

**Explanation (why is this different from Snippet A?):**

## Exercise 3.1: Fix the Broken Code
This code has scope errors. Fix it so it works correctly:
```python
# Goal: Track how many students have been processed
studentsProcessed = 0

def processStudent(name):
    print(f"Processing {name}")
    studentsProcessed = studentsProcessed + 1
    return f"Processed: {name}"

processStudent("Alice")
processStudent("Bob")
processStudent("Charlie")
print(f"Total processed: {studentsProcessed}")  # Should print 3
```

## Exercise 4.1: Protecting Your Data
You're analyzing student grades and need to sort them, but you want to keep the original order too.
```python
grades = [85, 92, 78, 90, 88]

# Method 1: Try this (wrong way)
sortedGrades1 = grades
sortedGrades1.sort()
print(f"Original: {grades}")
print(f"Sorted: {sortedGrades1}")
```
**What went wrong?**

___________________________________________________________________________

**Now fix it using .copy():**

# Week 8B Scripts, Comments, and Documentation 

## Question 1 
This function calculates the area of a triangle. Add ONE comment explaining the formula: 
```python
def calculateArea(base, height): 
    return 0.5 * base * height 
```

## Quiz 7 
Write a script that:
- Creates a function called `applyBonus` that takes a list of scores and a bonus amount. The function should:
    - Add the bonus to each score in the list
    - NOT modify the original list
    - Return the new list with bonuses applied
- Creates a short (3-5 elements) list of scores, pre-bonus
- Uses the `applyBonus` to apply the bonuses to the list of scores
- Calculates 
    - the sum of the scores pre-bonus
    - the sum of the scores with bonuses applied

In addition, include a docstring and appropriate comments.

# Week 9B: Plotting

Try the fill in the blank problem from below. Revisit our [plotting examples notebook](https://colab.research.google.com/drive/1p0AU1aephf-jbK0DxcBeugnYnOvjqAHY) if you need a refresher!

## Practice Problem: Scatter Plot — Train Service vs Wait Time

You have data showing how the number of trains per hour affects passengers’ average wait time:

| Trains per Hour | 8 | 10 | 12 | 15 | 18 | 20 | 22 |
|-----------------|---|----|----|----|----|----|----|
| Average Wait (minutes) | 7.5 | 6.0 | 5.0 | 4.0 | 3.3 | 3.0 | 2.7 |

Write a Python script that produces a scatter plot showing this relationship using **matplotlib**.  
Fill in the missing parts so that the code:
- creates a figure of size 10 × 6  
- plots green points with moderate size and partial transparency  
- labels both axes and adds a title  
- includes a grid and a tight layout  

```python
import matplotlib.pyplot as plt

trains_per_hour = [8, 10, 12, 15, 18, 20, 22]
avg_wait = [7.5, 6.0, 5.0, 4.0, 3.3, 3.0, 2.7]

plt.figure(figsize=(____, ____))
plt.________(trains_per_hour, avg_wait, s=____, alpha=____, color='____')

plt.________('Trains per Hour')
plt.________('Average Wait Time (minutes)')
plt.________('Relationship: Service Frequency vs Wait Time')

plt.________(True, alpha=0.3)
plt.________()
plt.show()
```

# Week 10A: File I/O Practice

## Problem 1: Temperature Logger

**Part A: Write the logger**

Create a cell that:
1. Asks the user for their name
2. Asks them to enter temperatures (in Celsius) one at a time
3. After each temperature, asks if they want to enter another (y/n)
4. When they are done, writes all data to a file called `tempLog.txt`

Format:
```
Observer: Alice
22.5
23.1
21.8
24.2
```

**Part B: Read and analyze**

Create a cell that:
1. Reads the data from `tempLog.txt`
2. Prints the observer's name
3. Calculates and prints:
   - Number of readings
   - Average temperature
   - Highest temperature
   - Lowest temperature