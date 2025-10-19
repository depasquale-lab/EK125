# Week 8 Class 14 Pre-read: Function Arguments, Variable Scope, and Data Copying


## Learning Objectives
By the end of this reading, you should understand:
- How arguments get passed to functions
- Where variables "live" in your program (scope)
- When changes inside a function affect the original data
- How to make true copies of your data when needed

## 0. Why This Matters: Python's Hidden Bookkeeping

### The Invisible Rules That Can Break Your Code
Until now, you've written relatively simple programs where you could track all your variables. But as your programs grow and use functions more extensively, Python does invisible "bookkeeping" behind the scenes that can cause surprising behavior.

Consider this seemingly simple code:

```python
def addStudent(roster, name):
    roster.append(name)

def resetRoster(roster):
    roster = []
    
classA = ["Alice", "Bob"]
addStudent(classA, "Charlie")
print(classA)  # ["Alice", "Bob", "Charlie"] - Changed!

classB = ["David", "Eve"]
resetRoster(classB)
print(classB)  # ["David", "Eve"] - Unchanged! Why?
```

Why did one function change our list while the other didn't? The answer involves Python's hidden rules about:
- How data moves into functions (it's not always a copy!)
- Where variables exist (not all variables are available everywhere)
- When changes persist (some changes vanish when the function ends)

### Why Can't Python Just Make Copies of Everything?
You might wonder: wouldn't it be simpler if Python just made copies of all data passed to functions? The answer is efficiency:
- A list with a million items would need to be copied every time you pass it to a function
- This would make programs incredibly slow and use massive amounts of memory
- Instead, Python has rules about what gets copied and what doesn't

Let's understand Python's system step by step.

## 1. How Arguments Get Into Functions

### Function Parameters and Arguments
When you define a function, you specify **parameters** - the names that will receive values. When you call a function, you provide **arguments** - the actual values:

```python
def greet(name, greeting):  # 'name' and 'greeting' are parameters
    print(f"{greeting}, {name}!")

greet("Alice", "Hello")  # "Alice" and "Hello" are arguments
```

### Default Parameter Values
Parameters can have default values that are used when no argument is provided:

```python
def greet(name, greeting="Hello"):
    print(f"{greeting}, {name}!")

greet("Alice")           # Uses default: "Hello, Alice!"
greet("Bob", "Hi")       # Overrides default: "Hi, Bob!"
```

**Order matters:** Parameters with defaults must come after parameters without defaults:

```python
# CORRECT
def makeProfile(name, age, country="USA"):
    return f"{name}, {age}, {country}"

# WRONG - SyntaxError!
def makeProfile(name, country="USA", age):
    return f"{name}, {age}, {country}"
```

### Keyword Arguments
You can specify arguments by name, which gives you powerful flexibility. **This is one of Python's most useful features** because it lets you:
1. **Skip over defaults you want to keep** - you don't have to provide every argument in order
2. **Provide arguments in any order** - as long as you use names, Python knows what goes where
3. **Make your function calls more readable** - the names document what each value means

```python
def createAccount(username, email, role="user", active=True):
    return f"{username} ({email}): {role}, active={active}"

# Positional arguments - must be in exact order
createAccount("alice", "alice@example.com", "admin", False)

# Keyword arguments - can be in ANY order!
createAccount(username="bob", role="admin", email="bob@example.com")
# Notice: role came before email, which is NOT the definition order

# Mix positional and keyword (positional must come first)
# This is especially useful to skip middle parameters
createAccount("charlie", "charlie@example.com", active=False)
# We skipped 'role' and kept its default, while still setting 'active'

# Without keyword arguments, you'd have to do:
createAccount("charlie", "charlie@example.com", "user", False)
# Must provide "user" explicitly even though it's the default!
```

**Why this matters:** Imagine a function with many optional parameters:
```python
def analyzeData(data, normalize=False, removeOutliers=False, 
                fillMissing=False, scale=True, verbose=False):
    # Implementation
    pass

# Want to only set verbose=True, keeping all other defaults?
# With keyword arguments (EASY):
analyzeData(myData, verbose=True)

# Without keyword arguments (TEDIOUS):
analyzeData(myData, False, False, False, True, True)
# Have to remember the order and provide every single value!
```

### Python Gives Functions Access, Not Copies
When you call a function, Python doesn't copy your data. Instead, it gives the function access to your original data by creating a new name (the parameter name) that refers to it:

```python
def processData(data):
    # 'data' is a new name that gives access to whatever was passed in
    print(f"Parameter 'data' has id: {id(data)}")
    
myList = [1, 2, 3]
print(f"Original 'myList' has id: {id(myList)}")
processData(myList)

# Output shows they have the SAME id - it's the same object!
```

Think of it like giving someone a nickname:
- You have a friend named "Elizabeth"
- You start calling her "Liz"
- Both names refer to the same person
- If "Liz" gets a haircut, "Elizabeth" has a new haircut too (same person!)

### The Confusing Case: Same Names Inside and Outside
Sometimes the variable name outside the function matches the parameter name inside the function. This can be confusing at first:

```python
def processGrades(grades):
    # The parameter 'grades' is a NEW name
    # It refers to whatever was passed in
    grades.append(100)
    print(f"Inside: {grades}")

# Our variable is also called 'grades'
grades = [85, 90, 78]
processGrades(grades)  # Pass 'grades' to parameter 'grades'
print(f"Outside: {grades}")
# Outside: [85, 90, 78, 100] - Modified!
```

Even though both are named `grades`, they're different names that happen to refer to the same object. The parameter `grades` inside the function is created fresh when you call the function.

**This gets really confusing with keyword arguments:**
```python
def analyzeScores(scores, threshold):
    print(f"Analyzing {len(scores)} scores with threshold {threshold}")
    # ... implementation ...

scores = [85, 90, 78, 92]
threshold = 80

# This looks bizarre but is perfectly valid!
analyzeScores(scores=scores, threshold=threshold)
#             ^^^^^^ parameter name
#                    ^^^^^^ variable name (different!)
```

The pattern `scores=scores` means "assign the value of the variable `scores` to the parameter `scores`." The first `scores` is the parameter name, the second is the variable name. They're different names that happen to be spelled the same way.

**Even more confusing - they can be in different orders:**
```python
# These are ALL equivalent:
analyzeScores(scores, threshold)
analyzeScores(scores=scores, threshold=threshold)
analyzeScores(threshold=threshold, scores=scores)  # Different order!
analyzeScores(scores, threshold=threshold)  # Mix and match
```

**When you'll see this pattern:**
This happens frequently when:
- Your function parameter names match common variable names (`data`, `scores`, `grades`)
- You're refactoring code and extracting functions
- You want to be explicit about what you're passing

```python
def calculateAverage(numbers):
    return sum(numbers) / len(numbers)

numbers = [10, 20, 30]
# Both work identically:
avg1 = calculateAverage(numbers)
avg2 = calculateAverage(numbers=numbers)  # More explicit but redundant
```

### The Two Types of Data: Mutable and Immutable
Python data types fall into two categories:

**Immutable (Cannot be changed):**
- Numbers: `int`, `float`
- Strings: `str`
- Tuples: `tuple`

**Mutable (Can be changed):**
- Lists: `list`
- Dictionaries: `dict`
- Sets: `set`

This distinction determines what happens in functions.

### Immutable Objects: Always Safe
With immutable objects, any "change" creates a new object:

```python
def tryToModify(x, s, t):
    x = x + 1      # Creates NEW number
    s = s.upper()  # Creates NEW string
    t = t + (4,)   # Creates NEW tuple
    print(f"Inside: x={x}, s={s}, t={t}")

num = 10
string = "hello"
tup = (1, 2, 3)

tryToModify(num, string, tup)
print(f"Outside: num={num}, string={string}, tup={tup}")
# Outside: num=10, string=hello, tup=(1, 2, 3) - All unchanged!
```

### Mutable Objects: Changes Affect the Original
With mutable objects, the function can modify your original data:

```python
def modifyList(lst):
    lst.append(999)        # Modifies the original
    lst[0] = "changed"     # Modifies the original
    print(f"Inside: {lst}")

myData = [1, 2, 3]
modifyList(myData)
print(f"Outside: {myData}")
# Outside: ['changed', 2, 3, 999] - Original was modified!
```

### The Assignment Trap
Here's the confusing part - using `=` inside a function NEVER modifies the original, even with mutable objects:

```python
def reassignList(lst):
    print(f"Before reassignment, id: {id(lst)}")
    lst = [7, 8, 9]  # Creates a NEW local variable!
    print(f"After reassignment, id: {id(lst)}")  # Different id!
    lst.append(10)   # Only affects the new local list

myData = [1, 2, 3]
reassignList(myData)
print(myData)  # [1, 2, 3] - Unchanged!
```

Why? Because `=` doesn't modify an object - it creates a new binding (a new name-to-object connection).

## 2. Scope: Where Variables Live

### Variables Have Homes
Every variable lives in a specific "scope" - a region in the code where the variabel can be accessed.

Most variables only exist locally, but for _very specific, targeted_ reasons some can be created to exist everywhere. 

Generally, `global` variables, as those are called, are bad programming habits. Still, they have their uses and we would be amiss not to discuss them in EK125.

### Why Scope Exists: The Chaos of Global Everything
Imagine if every variable existed everywhere all at once. What would happen?

NOTE: All the below code is hypothetical in a world where `scope` does not exist. The below code is INVALID and will not produce the described results because, well, python does not _not_ have the concept of `scope`.

**Problem 1: Name Collisions**
```python
# Without scope (hypothetical disaster)
def calculateTax(amount):
    rate = 0.08
    total = amount * (1 + rate)
    return total

def calculateDiscount(amount):
    rate = 0.15  # COLLISION! This would overwrite the tax rate
    total = amount * (1 - rate)  # And this would overwrite the tax total
    return total

# Both functions try to use 'rate' and 'total'
# If everything was global, they'd interfere with each other!
```

**Problem 2: Accidental Modification**
```python
# Helper function that processes data
def helper():
    # Oops! We accidentally modify someone else's variable
    count = 0  # This could overwrite a 'count' somewhere else
    data = []  # This could destroy data someone was using
    # Imagine debugging this in a 10,000 line program!

# Main program
count = 100  # Important counter
data = loadExpensiveData()  # Took 5 minutes to load
helper()  # Accidentally destroys our count and data!
```

**Problem 3: Impossible to Reason About**
```python
def processStudents():
    # Where did 'threshold' come from? Another file? Another function?
    # Without scope, you'd have to search the ENTIRE program
    if grade > threshold:
        print("Pass")

# With scope, you know: if it's not defined in this function,
# it must be a parameter, a global, or it's an error
```

**Problem 4: Memory Waste**
```python
def analyzeData():
    hugeTemporaryList = [lots of data...]
    # Process it
    return result

# Without scope, hugeTemporaryList would exist FOREVER
# With scope, it disappears when the function ends
```

**Scope solves all these problems:**
- Variables in one function can't accidentally affect another function
- You can use common names like `count`, `total`, `i` without fear
- Temporary variables disappear when you're done with them
- Code is easier to understand because variables have limited reach

### How Scope Works in Python
```python
# Global scope - main level of program
className = "Intro to Programming"  

def printInfo():
    # Local scope - inside this function
    semester = "Fall 2025"  
    print(f"{className} - {semester}")  # Can see both

printInfo()
print(className)  # Works - it's global
print(semester)   # ERROR! semester only exists inside the function
```

Each function gets its own "workspace" where it can create variables without worrying about stepping on anyone else's toes.

### The LEGB Rule
When Python sees a variable name, it searches in this order:
1. **L**ocal - Inside the current function
2. **E**nclosing - In the outer function (for nested functions)
3. **G**lobal - At the module level
4. **B**uilt-in - Python's built-ins like `print`

### Assignment Creates Local Variables
Here's the rule: using `=` inside a function creates a new local variable:

```python
total = 100  # Global

def setTotal():
    total = 50  # Creates NEW local variable
    print(f"Inside: {total}")  # 50

setTotal()
print(f"Outside: {total}")  # 100 - Global unchanged
```

### The `global` Keyword
To modify a global variable, you must declare it:

```python
counter = 0  # Global

def increment():
    global counter  # "I want the global one, not a local one"
    counter = counter + 1

increment()
increment()
print(counter)  # 2
```

Without `global`, Python gets confused:

```python
counter = 0

def incrementWrong():
    counter = counter + 1  # UnboundLocalError!
    # Python sees 'counter =' and plans to create a local variable
    # But then needs counter's value before it exists!
```

### When to Use `global`
Use `global` sparingly and only when necessary:

**Good use cases:**
```python
# Configuration that multiple functions need to modify
debugMode = False

def enableDebug():
    global debugMode
    debugMode = True

# Maintaining state across function calls
requestCount = 0

def logRequest():
    global requestCount
    requestCount += 1
    print(f"Request #{requestCount}")
```

**Better alternatives when possible:**
```python
# Instead of global, use return values
def incrementCounter(count):
    return count + 1

counter = 0
counter = incrementCounter(counter)

# Or use a class to group related data
class RequestLogger:
    def __init__(self):
        self.count = 0
    
    def logRequest(self):
        self.count += 1
        print(f"Request #{self.count}")
```

### The `nonlocal` Keyword
For nested functions that need to modify variables in the enclosing function:

```python
def outerFunction():
    count = 0
    
    def increment():
        nonlocal count  # Refers to count in outerFunction
        count += 1
    
    increment()
    increment()
    print(count)  # 2

outerFunction()
```

## 3. Making Copies When You Need Them

### The Problem: Multiple Names, Same Object
Assignment doesn't copy - it creates another name:

```python
original = [1, 2, 3]
alias = original  # NOT a copy!

print(original is alias)  # True - same object
alias.append(4)
print(original)  # [1, 2, 3, 4] - Changed!
```

### Solution 1: Shallow Copy
Use `.copy()` for a new list with the same contents:

```python
original = [1, 2, 3]
shallowCopy = original.copy()

shallowCopy.append(4)
print(original)  # [1, 2, 3] - Unchanged
print(shallowCopy)  # [1, 2, 3, 4]
```

**Warning:** Shallow copy only copies the outer container:

```python
original = [1, 2, [3, 4]]
shallowCopy = original.copy()

shallowCopy[2].append(5)  # Modifying the inner list
print(original)  # [1, 2, [3, 4, 5]] - Inner list changed!
```

### Solution 2: Deep Copy
For complete independence with nested structures:

```python
import copy

original = [1, 2, [3, 4]]
deepCopy = copy.deepcopy(original)

deepCopy[2].append(5)
print(original)  # [1, 2, [3, 4]] - Completely unchanged
print(deepCopy)  # [1, 2, [3, 4, 5]]
```

### When to Use Each Type of Copy

**Use shallow copy when:**
- Your list contains only immutable items (numbers, strings)
- You're copying simple structures
- Performance matters (shallow copy is faster)

```python
def processScores(scores):
    # Make a copy so we don't modify the original
    localScores = scores.copy()
    localScores.sort()
    return localScores[len(localScores)//2]  # Median
```

**Use deep copy when:**
- Your list contains other lists, dicts, or mutable objects
- You need complete independence
- You're okay with the performance cost

```python
def backupStudentData(students):
    # students is a list of dicts with nested lists
    # Need deep copy to protect all levels
    return copy.deepcopy(students)
```

**Don't copy when:**
- The function only reads data (no modifications)
- You want changes to persist (that's the point!)
- Working with huge datasets (copying is expensive)

```python
def calculateAverage(numbers):
    # No copy needed - just reading
    return sum(numbers) / len(numbers)

def sortInPlace(data):
    # No copy needed - we WANT to modify original
    data.sort()
```

## 4. Putting It All Together

### Example: Understanding a Complex Case
```python
def processGrades(grades, bonusPoints=5):
    # 'grades' refers to the original list (mutable)
    # 'bonusPoints' has value 5 (immutable)
    
    # This modifies the original grades
    for i in range(len(grades)):
        grades[i] += bonusPoints
    
    # This creates a new local variable
    bonusPoints = 10  # Doesn't affect anything outside
    
    return grades

# First call
class1 = [85, 90, 78]
result1 = processGrades(class1)
print(class1)  # [90, 95, 83] - Modified!
print(result1 is class1)  # True - same object

# Second call with protection
class2 = [88, 92, 86]
result2 = processGrades(class2.copy())  # Pass a copy
print(class2)  # [88, 92, 86] - Original unchanged!
```

### Common Pitfalls and Solutions

| Problem | Why It Happens | Solution |
|---------|----------------|----------|
| Function modifies your list | Lists are mutable, function gets original | Pass `myList.copy()` |
| Can't modify global in function | `=` creates local variable | Use `global` keyword |
| Nested lists change unexpectedly | Shallow copy shares inner objects | Use `copy.deepcopy()` |
| Can't modify enclosing variable | Assignment creates new local | Use `nonlocal` keyword |

## Appendix: The Mutable Default Parameter Trap

This is an advanced topic that won't affect most of your code, but you should be aware of it.

### The Problem
Default parameter values are created once when Python first reads the function definition:

```python
def addItem(item, itemList=[]):
    print(f"List id: {id(itemList)}")
    itemList.append(item)
    return itemList

# Call it multiple times without providing a list
result1 = addItem("apple")
print(result1)  # ['apple']

result2 = addItem("banana")  
print(result2)  # ['apple', 'banana'] - WHAT?!

result3 = addItem("cherry")
print(result3)  # ['apple', 'banana', 'cherry'] - It keeps growing!

# They're all the SAME list object!
print(result1 is result2 is result3)  # True
```

### Why This Happens
1. Python read the function definition and created ONE empty list
2. Every call without providing `itemList` uses that SAME list
3. Each call modifies the SAME list
4. This list persists between function calls

### The Solution
Use `None` as the default and create a new list inside the function:

```python
def addItem(item, itemList=None):
    if itemList is None:
        itemList = []  # Create a NEW list each time
    itemList.append(item)
    return itemList

# Now each call gets its own list
print(addItem("apple"))   # ['apple']
print(addItem("banana"))  # ['banana'] - Correct!
```

This pattern works for any mutable default (lists, dicts, sets).

## Key Concepts Summary

- **Parameter passing**: Functions get access to your data, not copies
- **Parameters**: Names defined in function definition
- **Arguments**: Values passed when calling the function
- **Default values**: Parameter values used when no argument provided
- **Keyword arguments**: Arguments specified by parameter name
- **Mutable**: Can be modified (lists, dicts, sets)
- **Immutable**: Cannot be modified (numbers, strings, tuples)
- **Scope**: Where a variable exists and can be accessed (LEGB rule)
- **Shallow copy**: Copies container but shares nested objects
- **Deep copy**: Complete independent copy of all levels
- **`global`**: Declares you want to modify a module-level variable
- **`nonlocal`**: Declares you want to modify an enclosing function's variable

## Check Your Understanding

1. Why does `lst.append(x)` modify the original list but `lst = []` doesn't?
2. What's the difference between `list2 = list1` and `list2 = list1.copy()`?
3. When should you use shallow copy vs. deep copy?
4. Why does Python search Local, Enclosing, Global, Built-in in that order?
5. When is it better to return a value rather than use `global`?
6. What happens if you define a function with `def func(a, b=5, c)`?
7. How can you call `createAccount(username, email, role, active)` with only username and email, keeping the defaults for role and active?
