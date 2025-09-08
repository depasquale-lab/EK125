# Week 2 Pre-read: Sequences, Strings, and Lists in Python

## Why Do We Need Sequences?

Imagine you're writing a program to manage a class roster. You could create individual variables for each student:

```python
student1 = "Alice"
student2 = "Bob" 
student3 = "Charlie"
```

But what happens when you have 100 students? 1000? Or when you need to add or remove students? Reorder them from last name to first name? This approach quickly becomes unmanageable.

**Sequences solve this problem** by letting you store multiple related items in a single, organized collection. Instead of dozens of individual variables, you can have:

```python
students = ["Alice", "Bob", "Charlie"]  # A list - can grow and shrink
```

Different sequence types solve different problems:

* **Strings** solve the problem of working with text (words, sentences, user input)
* **Lists** solve the problem of managing collections that change (shopping carts, student rosters, game scores)
* **Tuples** solve the problem of storing related data that shouldn't change (coordinates, RGB colors, database records)

All sequences share common operations (indexing, length, iteration) which means once you learn one, you understand the pattern for all of them.


## Learning Objectives
By the end of this reading, you should understand:
- What sequences are in Python and their common characteristics
- How to work with strings as sequences of characters
- How to work with tuples as immutable sequences
- How to create, modify, and access elements in lists
- Basic operations you can perform on sequences

## 1. Understanding Sequences

In Python, a **sequence** is an ordered collection of items. The three most important built-in sequence types are:

- **Strings** (`str`) - sequences of characters
- **Tuples** (`tuple`) - ordered collections of items that cannot be changed
- **Lists** (`list`) - ordered collections of items that can be changed

All sequences share common properties:
- They have a defined order
- You can access individual elements by position (indexing)
- You can find their length
- You can iterate through them

## 2. Working with Strings

Strings are sequences of characters enclosed in quotes.

### Creating Strings
```python
name = "Alice"
message = 'Hello, world!'
multiline = """This is a
multiline string"""
```

### String Indexing and Length
```python
word = "Python"
print(word[0])      # 'P' (first character)
print(word[-1])     # 'n' (last character)
print(len(word))    # 6 (length of string)
```

### Common String Operations
```python
text = "Hello World"
print(text.upper())           # "HELLO WORLD"
print(text.lower())           # "hello world"
print(text.title())           # "Hello World"
print(text.replace("o", "0")) # "Hell0 W0rld"
print(text.count("l"))        # 3 (counts occurrences of "l")
print(text.split())           # ["Hello", "World"] (splits on whitespace)
print("World" in text)        # True
```

## 3. Working with Tuples

Tuples are ordered collections that cannot be modified after creation.

### Creating Tuples
```python
coordinates = (10, 20, 30)
mixed = (1, "hello", 3.14, True)
empty = ()
singleItem = (42,)  # Note the comma for single-item tuples
```

### Tuple Indexing and Length
```python
point = ("x", 25, "y", 50)
print(point[0])     # "x"
print(point[-1])    # 50
print(len(point))   # 4
```

### Common Tuple Operations
```python
colors = ("red", "green", "blue")
print(colors[1])              # "green"
print("red" in colors)        # True
print(colors + ("yellow",))   # ("red", "green", "blue", "yellow")
print(colors * 2)             # ("red", "green", "blue", "red", "green", "blue")

# Tuples are often used for data that shouldn't change
studentRecord = ("Alice", 20, "Computer Science")
rgbColor = (255, 128, 0)  # Orange color
```

## 4. Working with Lists

Lists are ordered collections that can hold different types of data and can be modified.

### Creating Lists
```python
numbers = [1, 2, 3, 4, 5]
mixed = [1, "hello", 3.14, True]
empty = []
```

### Accessing List Elements
```python
fruits = ["apple", "banana", "orange"]
print(fruits[0])    # "apple"
print(fruits[-1])   # "orange"
print(len(fruits))  # 3
print(fruits.count("apple"))  # 1 (count of "apple" in list)
```

### Modifying Lists
```python
colors = ["red", "green", "blue"]
colors[1] = "yellow"           # Change element
colors.append("purple")        # Add to end
colors.insert(0, "pink")       # Insert at position
colors.remove("blue")          # Remove first occurrence of "blue"
removed = colors.pop()         # Remove and return last item
```

## 5. Common Sequence Operations

These operations work on all sequence types:

### Slicing (Preview - covered in more detail later)
```python
text = "Programming"
print(text[0:4])    # "Prog"

numbers = [1, 2, 3, 4, 5]
print(numbers[1:4]) # [2, 3, 4]
```

### Membership Testing
```python
print("gram" in "Programming")  # True
print("xyz" in "Programming")   # False
print(3 in [1, 2, 3, 4])       # True
print(5 in [1, 2, 3, 4])       # False

# Checking for "at least" - using count()
email = "alice@university.edu"
print(email.count("@") >= 1)   # True (has at least 1 @)
print(email.count("@") >= 2)   # False (doesn't have at least 2 @)
```

### Concatenation

(i.e., putting things next to each other)

```python
print("Hello" + " " + "World")  # "Hello World"
print([1, 2] + [3, 4])         # [1, 2, 3, 4]
```

### Processing All Items in a List
Sometimes we need to apply the same operation to every item in a list. Here is one common pattern, we'll introduce without explanation. Many more will come later.

```python
# Using a loop to create a new list
names = ["alice", "bob", "charlie"]
upperNames = []
for name in names:
    upperNames.append(name.upper())
print(upperNames)  # ["ALICE", "BOB", "CHARLIE"]
```

## 6. Important Differences: Strings vs Lists vs Tuples

`Strings` are **immutable** - you cannot change individual characters:
```python
word = "Hello"
# word[0] = "h"  # This would cause an error!
```

`Tuples` are **immutable** - like strings, you cannot change individual elements:
```python
coordinates = (10, 20, 30)
# coordinates[0] = 15  # This would cause an error!
```

`Lists` are **mutable** - you can change individual elements:
```python
items = ["a", "b", "c"]
items[0] = "A"  # This works fine
print(items)    # ["A", "b", "c"]
```

## 7. Other sequences
Strings, lists, and tuples are not the only sequences in Python. There are also others, such as range objects, byte arrays, etc.... However, those are out of scope for now.

### When to use each:
- **Strings**: For text data
- **Tuples**: For collections that should stay the same (coordinates, database records)
- **Lists**: For collections that need to change (add, remove, modify items)
3. What are the advantages of having a consistent interface across sequence types?

## Key Terms to Remember
- **Sequence**: An ordered collection of items
- **Index**: The position of an element in a sequence (starting from 0)
- **Mutable**: Can be changed after creation (like lists)
- **Immutable**: Cannot be changed after creation (like strings and tuples)
- **Length**: The number of elements in a sequence
