# Week 5A Morning Assignment: Nested lists and Multilevel Indexing
**Group Exercise (Teams of 3)**
Work together to complete the following tasks!

## Warmup: Nested list comprehensions

Create a nested list of the integers from 1 to n and their squares. Generate n as a random integer from 5 to 8. Do this using a list comprehension.


```python
import random
n = random.randint(5, 8)

newlist = [[i, i**2] for i in range(1, n+1)]
print(newlist)
```

    [[1, 1], [2, 4], [3, 9], [4, 16], [5, 25]]
    [[1, 1], [2, 4], [3, 9], [4, 16], [5, 25]]


## Problem 1: Ice cream combinations with nested list comprehensions

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


```python
flavors = ["vanilla", "chocolate", "strawberry"]
toppings = ["sprinkles", "fudge", "nuts"]

# Nested list comprehension: each flavor has a sublist of valid toppings
combos_small = [
    [(f, t) for t in toppings if not (f == "strawberry" and t == "fudge")]
    for f in flavors
]

print(combos_small)
```

    [[('vanilla', 'sprinkles'), ('vanilla', 'fudge'), ('vanilla', 'nuts')], [('chocolate', 'sprinkles'), ('chocolate', 'fudge'), ('chocolate', 'nuts')], [('strawberry', 'sprinkles'), ('strawberry', 'nuts')]]



```python
flavors = list(range(200))
toppings = list(range(200))

combos_large = [
    [(f, t) for t in toppings if (f + t) % 17 != 0]
    for f in flavors
]

print(len(combos_large))          # 200 sublists (one for each flavor)
```

    200


## Problem 2: Classroom Attendance Tracker with nested lists

A teacher wants to record the number of students present in each row of her classroom.  
The classroom has `n` rows and `m` desks per row.  

Each desk can either be **occupied (1)** or **empty (0)**.  

**Task**

1. Build the nested list using this description
  - Set `n = 3` (3 rows) and `m = 4` (4 desks per row).  
  - Create a nested list called `attendance`, where **each inner list represents one row of desks**.  
  - Fill each inner list with 1s and 0s to indicate which desks are occupied and which are empty:  
    - **Row 0:** first desk occupied, second desk empty, third desk occupied, fourth desk occupied  
    - **Row 1:** first desk occupied, second desk occupied, third desk occupied, fourth desk empty  
    - **Row 2:** first desk empty, second desk occupied, third desk occupied, fourth desk occupied  
2. **Nested For Loops (Build the Nested List)**  
   - Use **nested for loops** to create a nested list `attendance2` that represents the classroom using the nested listed you build above `attendance`.
   - Each inner list represents one row, and each element is the attendance at that desk.  
3. **Nested List Comprehension**  
   - Create another nested list `attendance3` using a **nested list comprehension** instead of loops.  

**Example Expected Output for each approach**
```
[1, 0, 1, 1]
[1, 1, 1, 0]
[0, 1, 1, 1]
```


```python
# --- Step 0: Start with the given nested list ---
attendance = [
    [1, 0, 1, 1],
    [1, 1, 1, 0],
    [0, 1, 1, 1]
]

print("Initial attendance list:")
for row in attendance:
    print(row)

# --- Nested Loops: Copy attendance to a new nested list ---
attendance_loops = []
for row in attendance:
    new_row = []
    for desk in row:
        new_row.append(desk)
    attendance_loops.append(new_row)

print("\nAttendance (nested loops):")
for row in attendance_loops:
    print(row)

# --- Nested List Comprehension: Copy attendance to another nested list ---
attendance_comp = [[desk for desk in row] for row in attendance]

print("\nAttendance (nested comprehension):")
for row in attendance_comp:
    print(row)
```

    Initial attendance list:
    [1, 0, 1, 1]
    [1, 1, 1, 0]
    [0, 1, 1, 1]
    
    Attendance (nested loops):
    [1, 0, 1, 1]
    [1, 1, 1, 0]
    [0, 1, 1, 1]
    
    Attendance (nested comprehension):
    [1, 0, 1, 1]
    [1, 1, 1, 0]
    [0, 1, 1, 1]


## Problem 3: Nested tuples

Do the same thing as Problem 2, but this time using nested tuples.

### Instructions

1. Start by constructing a nested tuple for `attendance`.
2. **Nested Loops Copy**
   - Use nested `for` loops to create a **new nested tuple** that copies the original.
   - **Important:** Tuples are immutable, so you cannot use `append()`.
     - Instead, build each inner tuple by **concatenating a single-element tuple**:
       ```python
       new_row = new_row + (desk,)
       ```
     - Then concatenate the inner tuple to the outer tuple:
       ```python
       new_attendance = new_attendance + (new_row,)
       ```
3. **Nested Comprehension Copy**
   - Use a **nested comprehension** to create another copy of the nested tuple.

Your code output should look like this:

```
Initial attendance tuple:
(1, 0, 1, 1)
(1, 1, 1, 0)
(0, 1, 1, 1)

Attendance (nested loops copy with tuples):
(1, 0, 1, 1)
(1, 1, 1, 0)
(0, 1, 1, 1)

Attendance (nested comprehension copy with tuples):
(1, 0, 1, 1)
(1, 1, 1, 0)
(0, 1, 1, 1)
```

**Key Point:** Tuples are immutable. You **cannot append**; you must **concatenate** tuples to add entries inside loops.


```python
# --- Part 0: Construct the nested tuple manually ---
attendance = (
    (1, 0, 1, 1),  # Row 0: first desk occupied, second empty, third occupied, fourth occupied
    (1, 1, 1, 0),  # Row 1: first three desks occupied, fourth empty
    (0, 1, 1, 1)   # Row 2: first desk empty, others occupied
)

print("Initial attendance tuple:")
for row in attendance:
    print(row)

# --- Part 1: Nested Loops Copy ---
attendance_loops = ()  # outer tuple
for row in attendance:
    new_row = ()  # inner tuple
    for desk in row:
        # Tuples are immutable: concatenate single-element tuple
        new_row += (desk,)
    # Add inner tuple to outer tuple
    attendance_loops += (new_row,)

print("\nAttendance (nested loops copy with tuples):")
for row in attendance_loops:
    print(row)

# --- Part 2: Nested Comprehension Copy ---
attendance_comp = tuple(tuple(desk for desk in row) for row in attendance)

print("\nAttendance (nested comprehension copy with tuples):")
for row in attendance_comp:
    print(row)
```

    Initial attendance tuple:
    (1, 0, 1, 1)
    (1, 1, 1, 0)
    (0, 1, 1, 1)
    
    Attendance (nested loops copy with tuples):
    (1, 0, 1, 1)
    (1, 1, 1, 0)
    (0, 1, 1, 1)
    
    Attendance (nested comprehension copy with tuples):
    (1, 0, 1, 1)
    (1, 1, 1, 0)
    (0, 1, 1, 1)


## Problem 4: Multi-indexing

From problem 1, you should have a nested list called `combos_small`. With this nested list, do the following indexing:

- Print the tuple of the first topping for "vanilla" (hint: access the first sublist, first element).
- Print the tuple of the last topping for "chocolate".
- Print all the tuples for all the topping pairs for "strawberry".
- Print the tuple of the second topping of "vanilla".
- Print the tuple containing "strawberry" and "nuts".


```python
# 1. First topping for "vanilla"
first_vanilla = combos_small[0][0]
print("First topping for vanilla:", first_vanilla)

# 2. Last topping for "chocolate"
last_chocolate = combos_small[1][-1]
print("Last topping for chocolate:", last_chocolate)

# 3. All topping pairs for "strawberry"
strawberry_toppings = combos_small[2]
print("All topping pairs for strawberry:", strawberry_toppings)

# 4. Second topping of "vanilla"
second_vanilla = combos_small[0][1]
print("Second topping for vanilla:", second_vanilla)

# 5. Tuple containing ("strawberry", "nuts")
strawberry_nuts = combos_small[2][1]
print("Tuple ('strawberry', 'nuts'):", strawberry_nuts)

```

    First topping for vanilla: ('vanilla', 'sprinkles')
    Last topping for chocolate: ('chocolate', 'nuts')
    All topping pairs for strawberry: [('strawberry', 'sprinkles'), ('strawberry', 'nuts')]
    Second topping for vanilla: ('vanilla', 'fudge')
    Tuple ('strawberry', 'nuts'): ('strawberry', 'nuts')


## Problem 5: Basic slicing

From problem 1, you should have a nested list called `combos_small`. With this nested list, do the following using slicing:

- Get the first two tuples of the first two toppings for "vanilla".
- Get all tuples for all toppings except the last one for "chocolate".
- Get the last tuple of the last topping for each flavor using slicing.
- Get the tuples for the first two flavors with all their toppings.
- Get a sublist of tuples containing the second and third toppings of "vanilla".


```python
# 1. First two toppings for "vanilla"
vanilla_first_two = combos_small[0][:2]
print("First two toppings for vanilla:", vanilla_first_two)

# 2. All toppings except the last one for "chocolate"
chocolate_except_last = combos_small[1][:-1]
print("Chocolate toppings except last:", chocolate_except_last)

# 3. Last topping for each flavor using slicing
last_toppings = [row[-1:] for row in combos_small]  # slice keeps it as a list
print("Last topping for each flavor:", last_toppings)

# 4. First two flavors with all their toppings
first_two_flavors = combos_small[:2]
print("First two flavors with all toppings:", first_two_flavors)

# 5. Second and third toppings of "vanilla"
vanilla_second_third = combos_small[0][1:3]
print("Second and third toppings for vanilla:", vanilla_second_third)
```

    First two toppings for vanilla: [('vanilla', 'sprinkles'), ('vanilla', 'fudge')]
    Chocolate toppings except last: [('chocolate', 'sprinkles'), ('chocolate', 'fudge')]
    Last topping for each flavor: [[('vanilla', 'nuts')], [('chocolate', 'nuts')], [('strawberry', 'nuts')]]
    First two flavors with all toppings: [[('vanilla', 'sprinkles'), ('vanilla', 'fudge'), ('vanilla', 'nuts')], [('chocolate', 'sprinkles'), ('chocolate', 'fudge'), ('chocolate', 'nuts')]]
    Second and third toppings for vanilla: [('vanilla', 'fudge'), ('vanilla', 'nuts')]

