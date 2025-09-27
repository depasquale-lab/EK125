# Week 5A Preread: Nested lists and Multilevel Indexing

## Overview

In the past few weeks, you have built a strong foundation with **loops**, **lists**, **indexing**, and even some **list comprehensions**. You’ve learned how to repeat actions with for loops, how to store values in lists, and how to print or process data row by row.

Now, we are ready for the next step: working with **nested structures**. A nested structure is simply a list (or tuple) that contains other lists (or tuples). This creates data that looks like a grid or table. Instead of just having a single line of values, you now have rows and columns—just like seats in a theater, squares in a chessboard, or entries in a multiplication table. It's important to note that these structures are **strictly** organized into a grid (that's an **array** which we will study later!) but rather, the nesting structing can be **interpreted** as coordinates along a grid.

This preread introduces you to three major skills that go hand-in-hand:

- **Constructing** nested structures using nested loops or list comprehensions.
- **Accessing** values inside those structures using multi-level indexing.
- **Extracting** segments of lists using **slicing**, a new kind of indexing that lets you grab a “window” of values at once.

You will see that everything we do this week builds directly on what you already know. Nested structures come naturally out of nested loops (Week 4). Indexing rules from Week 1–2 still apply—but now you’ll sometimes apply them twice. And slicing, which you only glimpsed briefly when working with strings, now takes center stage for lists.

⚠️ **Important:** We’ll keep slicing simple in this preread—only working with **flat lists**, not nested ones. Advanced slicing (like selecting whole rows or columns of a grid) will come later.

---

## Nested List Comprehensions

### Why This Matters

List comprehensions are a compact way of writing loops that build lists. In Week 4, you learned that instead of writing a `for` loop with `append`, you could create a list in one line. This was your first encounter with list comprehensions, and you saw how they can replace loops when the goal is simply to build a list.

Now, we extend this idea: if one comprehension builds a single list, then a **nested comprehension** can build a **list of lists**. This is how you can create 2D structures like grids or tables in just one expression. Conditionals can be added as well, just as in Week 4!

### A Simple Comprehension

```python
[row for row in range(3)]
```

**Output:**

```python
[0, 1, 2]
```

This reads as: “for each `row` in the sequence `0, 1, 2`, collect `row` into a list.” You saw in Week 4 that this is equivalent to:

```python
result = []
for row in range(3):
    result.append(row)
```

📎 **Recall Week 4:** This was your first glimpse of **vectorization**. Instead of looping step by step, you could build the whole list in one expression. 

---

### Adding Nesting

So far, your comprehensions have created flat lists—just a single row of values. But what if you want a table or grid? That’s where **nesting** comes in.

The key idea is this:

- The **inner comprehension** builds one **row**.
- The **outer comprehension** repeats that row for each iteration.

This mirrors what you did with **nested for loops** in Week 4—one loop for rows, one loop for columns. With nesting, you’re compacting that logic into a single expression.

```python
[[col for col in range(3)] for row in range(2)]
```

**Output:**

```python
[[0, 1, 2], [0, 1, 2]]
```

**Step by step:**

1. The inner comprehension `[col for col in range(3)]` creates `[0, 1, 2]`.
2. The outer comprehension repeats that inner list twice, once for each row.

The final result is a list with two entries in the outer list, and three entries *within* each location of that outer list.

---

### Nested Loop Comparison

As we've noted, you can do the same thing with a *nested loop*:

```python
nested = []
for row in range(2):
    inner = []
    for col in range(3):
        inner.append(col)
    nested.append(inner)
print(nested)
```

**Output:**

```python
[[0, 1, 2], [0, 1, 2]]
```

Both approaches are identical in logic. The comprehension is just a shorthand.

---

### Another Example: Product Table

```python
products = [[r * c for c in range(1, 5)] for r in range(1, 4)]
for row in products:
    print(row)
```

**Output:**

```python
[1, 2, 3, 4]
[2, 4, 6, 8]
[3, 6, 9, 12]
```

**Comparison to Nested Loop**

```python
products = []
for r in range(1, 4):
    row = []
    for c in range(1, 5):
        row.append(r * c)
    products.append(row)

for row in products:
    print(row)
```

**Output:**

```python
[1, 2, 3, 4]
[2, 4, 6, 8]
[3, 6, 9, 12]
```

**Why this matters:** nested comprehensions let you **construct** the very structures that nested loops would otherwise only **print**, or that would be constructed in a nested loop via `.append()`. That’s the big shift this week.

#### Pause and Think
- How many items will this comprehension build in the *inner* comprehension: `[[c for c in range(4)] for r in range(3)]`?
- How many elements in total will the result contain?
- What happens if you swap the inner and outer loops?

---

## Nested Lists and Tuples

### Why This Matters

Nested lists (and tuples) are the data structures that correspond to the nested loops you wrote last week. Instead of just printing values row by row, you can now **store** them in memory in a way that preserves their row/column relationships. Nested comprehensions, like above, allow us to avoid using `.append()` in a nested loop.

### A Nested List Example

```python
seats = [['A1', 'A2'], ['B1', 'B2']]
```

**Looping through:**

```python
for row in seats:
    for seat in row:
        print(seat)
```

**Output:**

```
A1
A2
B1
B2
```

### Using `enumerate()`

Sometimes, just looping through values isn’t enough—you also want to know **where** you are in the list. That’s where `enumerate()` comes in: it gives you both the **index (position)** and the **value** at the same time.

This is especially useful for nested lists, where you often care about both the **inner list index number** and the **outer list index number**. Without `enumerate()`, you’d have to manually manage counters. With it, Python handles the counting for you.

```python
for i, row in enumerate(seats):
    for j, seat in enumerate(row):
        print(f"Row {i}, Col {j}: {seat}")
```

**Output:**

```
Row 0, Col 0: A1
Row 0, Col 1: A2
Row 1, Col 0: B1
Row 1, Col 1: B2
```

### `enumerate()` in a List Comprehension

So far, you’ve seen `enumerate()` in a regular nested loop, where it provided inner and outer loop index numbers alongside each value. But remember: comprehensions are just another way of expressing loops. That means you can combine `enumerate()` with **comprehensions** to directly build a new structured list in one step.

This is especially useful when you want to **tag** values with their positions or transform data into a new form without writing extra loop code.

```python
indexed = [(i, j, seat) 
           for i, row in enumerate(seats) 
           for j, seat in enumerate(row)]

print(indexed)
```

**Output:**

```python
[(0, 0, 'A1'), (0, 1, 'A2'), (0, 2, 'A3'),
 (1, 0, 'B1'), (1, 1, 'B2'), (1, 2, 'B3'),
 (2, 0, 'C1'), (2, 1, 'C2'), (2, 2, 'C3')]
```

Now each seat is tagged with both row and column indices—this is structured data in action.

### Nested Tuples

Tuples are immutable but can still nest:

```python
blocks = ((8, 10), (11, 1), (2, 4))
print(blocks[1][0])
```

**Output:**

```python
11
```

And can be looped over. Notice that here we are **unpacking** each tuple into two separate variables, `start` and `end`, so they can be used directly:

```python
for start, end in blocks:
    print(f"Starts at {start}, ends at {end}")
```

**Output:**

```
Starts at 8, ends at 10
Starts at 11, ends at 1
Starts at 2, ends at 4
```

#### Pause and Think
- What’s the difference between `[(8, 10), (11, 1)]` and `[[8, 10], [11, 1]]`?
- Which one is better if times can change?

---

## Multi-Level Indexing

### Why This Matters

Nested loops build nested structures. But once those structures exist, you don’t need to loop over everything—you can jump straight to the element you want with **multi-level indexing**.

This is like replacing a journey through the whole theater with a direct seat ticket: “Row 2, Seat 3.”

### Indexing Into a Grid

```python
grid = [[10, 20], [30, 40], [50, 60]]

print(grid[0])      # First row
print(grid[2])      # Third row
print(grid[2][0])   # Third row, first element
```

**Output:**

```python
[10, 20]
[50, 60]
50
```

Think of each bracket as peeling away a layer:

```
grid        → entire 2D structure
grid[2]     → one row: [50, 60]
grid[2][0]  → one number: 50
```

⚠️ **Important:** Recall again that this grid is not **literally** a square, but rather an interpretation of a grid where the outer list is rows of a grid and the inner list is a column. For example, in this structure, each row does not have to have the same number of entires (which would be true for an actual grid). Later we will discuss **arrays** that are **literally grids**. 

### Visualizing Multi-Level Indexing

```
Grid = [
  [10, 20],   # Row 0
  [30, 40],   # Row 1
  [50, 60]    # Row 2
]

Indexes:   Row   Col
           [2]   [0]

Result:    50
```

Here, `grid[2][0]` means:

1. Go to **row 2** → `[50, 60]`.
2. Then go to **column 0** inside that row → `50`.

### Comparing to Loops

```python
for row in grid:
    for val in row:
        print(val)
```

**Output:**

```
10
20
30
40
50
60
```

Pinpointing with indexing:

```python
print(grid[1][1])  # Second row, second column
```

**Output:**

```
40
```

### Modifying Values

Recall that lists are *mutable* (tuples are not) so you can modify individual entries within a nested list:

```python
grid[0][1] = 99
print(grid)
```

**Output:**

```python
[[10, 99], [30, 40], [50, 60]]
```

#### Pause and Think
- If `grid` has 3 rows, what happens if you ask for `grid[3][0]`?
- If `grid[2]` is `[50, 60]`, what type is `grid[2]`? What type is `grid[2][0]`?
- Why does indexing always go **row first**, then **column**?

---

## Slicing (First Look) and Negative Indexing

### Why This Matters

Indexing gives you one element. **Slicing** gives you a **window** of elements in one step. This is both convenient and powerful: it’s your **next** taste of **vectorization**.

📎 **Recall Week 4:** comprehensions were your first vectorization step. You could replace a loop with one line that built the whole list. Slicing is another, simpler vectorization: instead of looping to collect values into a new list, slicing produces that new list immediately.

### The Basics

- In slicing, the colon `:` separates the **start index** from the **end index**.
- The slice **includes** the element at the **start** index,
- but it **stops right before** the **end** index (**exclusive**).

Think of it as: *start at the first number, go up to but not including the second.*

```python
nums = [5, 10, 15, 20, 25]
print(nums[1:4])   # 1 through 3
print(nums[:3])    # start from the beginning through 2
print(nums[2:])    # start from 2 to the end of the list
print(nums[-1])    # last element
print(nums[:-1])   # everything but last
```

**Output:**

```python
[10, 15, 20]
[5, 10, 15]
[15, 20, 25]
25
[5, 10, 15, 20]
```

**Visual diagram for `nums[1:4]`:**

```
Index:   0    1    2    3    4
Value:   5   10   15   20   25
Slice:       [----|----|----)   
             ^              ^
             start=1        end=4 (exclusive)
```

Here, the slice starts at index `1` (value `10`) and goes up to but not including index `4`. That’s why you get `[10, 15, 20]`.

👉 **Note:** A very common beginner mistake is to expect the slice `[1:4]` to include the element at index `4`. Remember: **end is exclusive**.

### Loops vs Slicing

Why compare loops to slicing? Because they solve the same problem: extracting a subset of values from a list.

- A loop does it **step by step**: you move through indices one by one, appending values to a new list.
- Slicing does it in **one vectorized expression**: you describe the block of data you want, and Python hands it back.

This shift from “step-by-step instructions” to “whole-block operations” is at the heart of vectorized thinking.

**Loop approach:**

```python
subset = []
for i in range(1, 4):
    subset.append(nums[i])
print(subset)
```

**Output:**

```python
[10, 15, 20]
```

**Vectorized with slicing:**

```python
print(nums[1:4])
```

**Output:**

```python
[10, 15, 20]
```

### Negative Indices

Negative indexing (or working backwards from the end of a list) is not limited to -1:

```python
print(nums[-1])   # last element
print(nums[-2])   # second to last
print(nums[:-1])  # all but last
```

**Output:**

```python
25
20
[5, 10, 15, 20]
```

#### Pause and Think
- What is the length of `nums[2:5]`?
- What happens with `nums[1:10]`?
- Why might negative indices be safer than hardcoding `len(nums)-1`?

---

## Summary Table

| Concept                 | Example                                           | Result                |
|-------------------------|---------------------------------------------------|-----------------------|
| Basic list comp         | `[x for x in range(3)]`                           | `[0, 1, 2]`           |
| Nested list comp        | `[[x for x in range(2)] for _ in range(3)]`       | `[[0, 1], [0, 1], [0, 1]]` |
| Nested list indexing    | `seats[1][0]`                                     | `'B1'`                |
| Tuple in list           | `((8, 10), (11, 1))[1][0]`                        | `11`                  |
| Slice of list           | `nums[1:4]`                                       | `[10, 15, 20]`        |
| Negative index          | `nums[-1]`                                        | `25`                  |
| Using `enumerate`       | `for i, x in enumerate(vals)`                     | `(0, x), (1, y), ...` |

---

## Why This Matters

This week is about moving from **procedural thinking** (loops that do everything step by step) to **structural thinking** (building and working with data structures).

- **Nested comprehensions and loops** let you build **2D data**.
- **Multi-level indexing** lets you access **exactly** what you need from that data.
- **Slicing** lets you work with **spans of data** at once—your first step into **vectorization**.

These skills are foundational for later work in **tabular data**, **2D arrays**, and libraries that rely heavily on vectorized operations (like **NumPy** and **pandas**). They also reappear in **string processing**, since strings can be sliced just like lists.

---

## Quick Check: Did You Understand This?

- Can I explain how a nested list comprehension builds **rows vs columns**?
- Can I trace exactly what `grid[1][2]` refers to and **why**?
- Can I predict the **length** of `nums[1:4]` without running it?
- Can I write both a **nested loop** and a **nested comprehension** to create the same **2D structure**?
- Can I use `enumerate()` to **track row/column positions** in a nested loop?
- Can I explain **why slicing is considered a simple form of vectorization**?
