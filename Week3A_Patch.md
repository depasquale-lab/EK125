# Week3A Patch: Strings, Objects & Methods, Input, Casting, and Output Formatting

Before we start working with **selection statements**, we need to strengthen a few foundational skills. These include:  
- working with **strings** (beyond the basics),  
- understanding **objects and methods**,  
- using **input** to interact with the user,  
- performing **type casting** to safely convert between types, and  
- controlling **output formatting** for readable results.  

These topics will be woven together: strings are objects with methods; input always produces strings, so casting and formatting go hand-in-hand.  

---

## 1. Strings: Basics, Escape Characters, Indexing, and Slicing

Strings are sequences of characters. Because they are sequences, we can index, slice, and iterate through them. Strings are central in Python: every input you receive from the keyboard is a string, every filename is a string, and every printed message is a string.

### Creating Strings

Strings can be created using single quotes, double quotes, or triple quotes:

```python
s1 = 'Hello'
s2 = "World"
s3 = '''A longer string
that can span multiple lines'''
```

All three are strings. The choice of quote often depends on whether the string itself contains quotes.

---

### Escape Characters

Certain characters cannot be typed directly into a string, so Python uses escape sequences — a backslash `\` followed by another character.

- `\n` – newline  
- `\t` – tab  
- `\'` – single quote  
- `\"` – double quote  
- `\\` – backslash  

Examples:

```python
print("Hello\nWorld")
print("Column1\tColumn2")
print('Isn\'t this neat?')
print("The character \"a\" is a short string")
print("A backslash looks like this: \\")
```

Output:

```
Hello
World
Column1    Column2
Isn't this neat?
The character "a" is a short string
A backslash looks like this: ```

---

### Indexing

Each character in a string has an index, starting at 0:

```python
word = "Python"
print(word[0])   # P
print(word[1])   # y
print(word[5])   # n
```

Negative indices count from the end:

```python
print(word[-1])   # n
print(word[-2])   # o
```

---

### Slicing

Slicing extracts a part of a string: `string[start:end]` returns characters from `start` up to but **not including** `end`.

```python
print(word[0:2])   # 'Py'
print(word[2:5])   # 'tho'
print(word[:3])    # 'Pyt'
print(word[3:])    # 'hon'
```

⚠️ Remember: the slice excludes the end index. `"Python"[0:2]` is `'Py'`, not `'Pyt'`.

---

### Strings Are Immutable

Strings cannot be changed in place. Once created, they remain fixed. To “change” a string, build a new one:

```python
s = "cat"
# s[0] = "b"   # ERROR: strings don’t support assignment

s = "b" + s[1:]   # slice and concatenate
print(s)          # "bat"
```

---

### Common Mistakes with Strings

- Treating them like mutable lists:  
  ```python
  s = "dog"
  s[0] = "c"   # ERROR
  ```
- Forgetting that slices exclude the end index.  
- Misunderstanding escape sequences:  
  ```python
  print("This is a backslash: ")   # ERROR
  ```
  Must be written as:  
  ```python
  print("This is a backslash: \\")
  ```

---

## 2. Objects and Methods

In Python, everything is an **object**: numbers, strings, lists, dictionaries. Each object has associated data and **methods** (functions tied to that object).

### Methods for Strings

Methods are invoked with dot notation:

```python
word = "python"
print(word.upper())       # 'PYTHON'
print(word.lower())       # 'python'
print(word.capitalize())  # 'Python'
print(word.title())       # 'Python'
```

Replacement and searching:

```python
print(word.replace("py", "java"))  # 'javathon'
print(word.find("th"))             # 2
print(word.find("z"))              # -1 (not found)
```

Whitespace removal:

```python
s = "   Hello World   "
print(s.strip())   # 'Hello World'
print(s.lstrip())  # 'Hello World   '
print(s.rstrip())  # '   Hello World'
```

---

### Methods for Lists

Lists are also objects, with their own methods:

```python
numbers = [3, 1, 4]
numbers.append(2)     # add to the end
numbers.sort()        # sort in place
print(numbers)        # [1, 2, 3, 4]

numbers.reverse()     # reverse in place
print(numbers)        # [4, 3, 2, 1]
```

---

### Common Errors with Methods

- Forgetting parentheses:  
  ```python
  word = "python"
  print(word.upper)   # prints function object, not 'PYTHON'
  ```
- Calling methods on the wrong type:  
  ```python
  num = 42
  num.upper()   # ERROR
  ```

---

## 3. User Input

Programs can read input from the keyboard using the **`input()`** function:

```python
name = input("What is your name? ")
print("Hello,", name)
```

Sample run:

```
What is your name? Alice
Hello, Alice
```

⚠️ **Important:** `input()` always returns a string.

---

### Common Errors with Input

- Forgetting that input returns a string:  
  ```python
  age = input("How old are you? ")
  print(age + 1)   # ERROR: string + int
  ```

---

## 4. Type Casting (Converting Between Types)

Because `input()` produces strings, we often need to convert to numbers.

```python
age = input("How old are you? ")
numage = int(age)
print("Next year you will be", numage + 1)
```

Other conversions:

```python
print(int("42"))       # 42
print(float("3.14"))   # 3.14
print(str(123))        # '123'
print(bool(0))         # False
print(bool(1))         # True
```

---

### Common Errors with Casting

- Trying to cast invalid strings:  
  ```python
  int("hello")   # ERROR
  float("ten")   # ERROR
  ```
- Forgetting that `bool("0")` is `True` because any non-empty string is True.

---

## 5. Output Formatting

Controlling the appearance of printed output makes programs more readable.

### Escape Characters in Output

```python
print("Hello\nWorld")
print("Isn\'t this great?")
print("The character \"a\" is a short string")
```

---

### f-Strings

Embed variables directly into strings:

```python
name = "Alice"
age = 21
print(f"My name is {name} and I am {age} years old.")
```

---

### Field Width and Alignment

```python
num = 1234
print(f"The number is xx{num:8d}xx")   # right aligned
print(f"The number is xx{num:<8d}xx")  # left aligned
print(f"The number is xx{num:^8d}xx")  # centered
```

Output:

```
The number is xx    1234xx
The number is xx1234    xx
The number is xx  1234  xx
```

---

### Floats

```python
pi = 3.14159
print(f"Pi is {pi:.2f}")      # 2 decimal places
print(f"Pi is {pi:8.3f}")     # width 8, 3 decimals
print(f"Pi is {pi:e}")        # scientific notation
```

Output:

```
Pi is 3.14
Pi is    3.142
Pi is 3.141590e+00
```

---

### Common Errors with Output

- Forgetting the `f`:  
  ```python
  name = "Alice"
  print("Hello {name}")   # literally prints {name}
  ```
- Wrong specifier:  
  ```python
  pi = 3.14
  print(f"{pi:d}")   # ERROR, 'd' is for integers
  ```

---

## ✅ Key Takeaways

- Strings are sequences: use indexing, slicing, and escape sequences.  
- Strings are immutable — new strings must be created instead of changing them directly.  
- Objects (like strings, lists) come with useful **methods**.  
- `input()` always produces a string. Convert with casting functions (`int`, `float`, `str`, `bool`).  
- f-strings allow rich formatting for readable and precise output.  
- Most errors come from forgetting types: strings vs. ints, method parentheses, format specifiers.
