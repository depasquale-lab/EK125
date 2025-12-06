# Week3A Preread: Coding Basics Wrap-up and Selection Statements
# Coding Basics Wrap-up: Strings, Objects & Methods, Input, Casting, and Output Formatting

Before we start working with **selection statements**, we need to wrap up a few loose ends on our basic foundational skills. We might retread some ground we've already covered, but we want to make sure we didn't miss anything critical! These include:  

- 'escape characters' when working with **strings**,
- understanding **objects and methods**,
- using **input** to interact with the user,
- performing **type casting** to safely convert between types, and
- controlling **output formatting** for readable results.

---

## 1. Strings: Escape Characters, Indexing, and Slicing

As we've discussed, strings are sequences of characters. Because they are sequences, we can index, slice, and iterate through them. Strings are central in Python: every input you receive from the keyboard is a string, every filename is a string, and every printed message is a string.

### Creating Strings

Strings can be created using single quotes, double quotes, or triple quotes:

```python
s1 = 'Hello'
s2 = "World"
s3 = '''A longer string
that can span multiple lines'''
s4 = """A longer string
that can span multiple lines"""
```

All four are strings. The choice of quote often depends on whether the string itself contains quotes.

---

### Escape Characters

Certain characters cannot be typed directly into a string, so Python uses escape sequences — a backslash ``\`` followed by another character.

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
A backslash looks like this: \
```

---

## Indexing

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

**Remember**: the slice excludes the end index. `"Python"[0:2]` is `'Py'`, not `'Pyt'`.

---

### REMEMBER: Strings Are Immutable!

We've discussed this, but remember as we point out slicing and indexing a string, remember that strings **cannot be changed in place**. Once created, they remain fixed. To “change” a string, build a new one:

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
  print("This is a backslash: \")   # ERROR
  ```
  Must be written as:  
  ```python
  print("This is a backslash: \\")
  ```

---

## 2. Objects and Methods

In Python, everything is an **object**: numbers, strings, lists, dictionaries. Each object has associated data and **methods** (we've also called them **functions** tied to that object). We've seen lots of objects and methods already, so some of the following may look familiar. But we wanted to formally point out the notion of **objects** and **methods**, which are important concepts as we move forward!

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

**Important:** `input()` always returns a string.

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

There are other conversions, that allow us to 'cast' (or convert) between the different object types we've discussed, such as `floats`, `int`, and `bools`:

```python
print(int("42"))       # 42
print(float("3.14"))   # 3.14
print(str(123))        # '123'
print(bool(0))         # False
print(bool(1))         # True
print(bool(-1))        # True
```

That last one seems crazy! What's going on there? In Python, the `bool()` method converts a value to a Boolean. The rule is based on the concept of truthiness: `bool(x)` is `False` if x is one of the “falsy” values. Common falsy values include:

- 0 (integer zero)
- 0.0 (floating-point zero)
- "" (empty string)
- [] (empty list), {} (empty dict), () (empty tuple), set() (empty set)
- None
- False

Everything else is considered “truthy”, so `bool(x)` returns `True` for anything not in this list!

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

Output

```
Hello
World
Isn't this great?
The character "a" is a short string
```
---

### f-Strings

We've seen this a few times, and now we are goign to learn it properly! A 'f-string' embeds variables directly into strings:

```python
name = "Alice"
age = 21
print(f"My name is {name} and I am {age} years old.")
```

Output
```
My name is Alice and I am 21 years old.
```

---

### Field Width and Alignment

When you use an f-string like `{num:8d}`, the part after the colon (`:`) is called the **format specifier**, and the colon itself acts as a **delimiter** separating the variable from its formatting instructions. For integers:

- `8` → sets the **field width** to 8 characters. If the number has fewer digits, spaces are added.
- `<` → **left-aligns** the number in the field.
- `>` → **right-aligns** (default for numbers).
- `^` → **centers** the number.

So in `xx{num:<8d}xx`, the `1234` takes up 4 characters, and 4 spaces are added on the right to fill the width of 8.

**Example:**

```python
num = 1234
print(f"xx{num:8d}xx")   # right aligned
print(f"xx{num:<8d}xx")  # left aligned
print(f"xx{num:^8d}xx")  # centered
```

Output:

```
xx    1234xx
xx1234    xx
xx  1234  xx
```

For floating-point numbers:

- `.2f` → rounds to 2 decimal places.
- `8.3f` → total field width of 8 characters, rounded to 3 decimals. Spaces are added if needed to meet the width.
- `e` → scientific notation (e.g., `3.141590e+00`).

Mechanically, Python first **rounds the number** according to the precision, then **pads it with spaces** if the field width is larger than the resulting string, and finally **aligns** it according to `<`, `>`, or `^`.

**Example:**

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

For `num = 1234` with width 8:

  - Right-aligned → `"    1234"`
  - Left-aligned → `"1234    "`
  - Centered → `"  1234  "`

For `pi = 3.14159` with `8.3f`:

  - Formatted → `"   3.142"` (rounded to 3 decimals, padded with spaces to reach width 8)

Example:

```python
# Integer alignment
num = 1234

right_aligned = f"{num:8d}"   # right-aligned (default)
left_aligned  = f"{num:<8d}"  # left-aligned
centered      = f"{num:^8d}"  # centered

print("Integer alignment examples:")
print(f"Right-aligned: '{right_aligned}'")
print(f"Left-aligned:  '{left_aligned}'")
print(f"Centered:      '{centered}'")

# Floating-point formatting
pi = 3.14159
formatted_pi = f"{pi:8.3f}"   # width 8, 3 decimal places

print("\nFloating-point example:")
print(f"Formatted pi: '{formatted_pi}'")
```

Output
```
Integer alignment examples:
Right-aligned: '    1234'
Left-aligned:  '1234    '
Centered:      '  1234  '

Floating-point example:
Formatted pi: '   3.142'
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

## Key Takeaways

- Strings are sequences: use indexing, slicing, and escape sequences.
- Objects (like strings, lists) come with useful **methods**.
- `input()` always produces a string. Convert with casting functions (`int`, `float`, `str`, `bool`).
- f-strings allow rich formatting for readable and precise output.
- Most errors come from forgetting types: strings vs. ints, method parentheses, format specifiers.

<br>
<br>
<br>

# Selection Statements

**AT LAST! WE ARE MOVING ON TO SELECCTION STATEMENTS!**

It is possible in a program to choose **whether statements are executed or not**, or to **choose between statements or sets of statements**. Statements that accomplish this are called **selection statements** and include the `if` statement and the `if-else` statement. Python has additional selection statements including the `match` statement and `try-except`.

## If Statements

The `if` statement chooses whether statement(s) are executed or not. The general form is

```python
if condition:
    action
# rest of code
```

The `if` statement starts with the reserved word `if`, then an expression that evaluates to either `True` or `False`, then a colon. The expression is frequently called a **condition**. After that, the action of the `if` is indented. The action consists of one or more statements that are executed only if the expression evaluates to `True`. If the expression evaluates to `False`, the action is skipped.

The indentation is very important in Python. If the action consists of more than one statement, all must be indented to the same level. It is customary to indent by 4 spaces.

In the following example, it is `True` that the value of the variable num is less than 50, so the  action is executed, which prints ‘It is smaller’.

```python
print('OK') 
num = 33  
if num < 50: 
    print('It is smaller') 
print('And that is it')
```

Output

```
OK
It is smaller
And that is it
```

If the value of num is changed to be something greater than or equal to 50, the action would be skipped entirely.

```python
print('OK') 
num = 62 
if num < 50: 
    print('It is smaller') 
print('And that is it')
```

```
OK
And that is it
```

Of course, building in the value of the variable `num` by assigning a value and then testing it does not really make sense! It would make much more sense to generate a random number (which we will get too soon!) or to prompt the user for the value of the variable num, and then print (or not) based on what the user entered.

```python
# Prompt the user to enter a number
num = int(input("Please enter a number: "))

# Check the value and print a message if it is smaller than 50
if num < 50:
    print("It is smaller")
print("And that is it")
```

```
It is smaller
And that is it
```

We can use the in operator to test to see whether a particular character is in a string, or whether a particular value is in a list.

```python
if 'x' in 'abcde': 
    print("Yay, there is an 'x'!") 
print('Done.')
```

```
Done.
```

Again, it would make more sense if the string was not built in. It could instead be something entered by the user.

```python
urname = input('Please enter your name: ') 
if 'z' in urname: 
    print("Yay, there is a 'z'!") 
print('OK.')
```

```
Please enter your name: Frazier
Yay, there is a 'z'!
OK.
```

## If-else Statements

To choose between two statements, or sets of statements, the `if-else` statement is used. The general form is

```python
if condition:
    ifaction
else:
    elseaction
# rest of code
```

The `if-else` statement chooses between two actions, called ‘ifaction’ and ‘elseaction’ here. The way it works is that the condition is evaluated. If the value of the condition is `True`, then all of the statements in the ‘ifaction’ will be executed, and the `if-else` statement ends. If, on the other  and, the value of the condition is `False`, the statement(s) in the second action, ‘elseaction’, will  be executed. The `if-else` statement chooses between executing the ‘ifaction’ or the ‘elseaction’. One of these actions, and only one, will be executed. The terminology is that 'control' goes to the chosen action, and those statements are executed. Once the statements in the chosen action have been executed, control goes to the rest of the code after the `if-else` statement.

```python
num = input('Enter a number: ') 
num = float(num) # convert the string to a float 
if num < 0: 
    print(f'{num} is a negative number') 
else: 
    print(f'{num} is a nonnegative number')
```

In this example, the user is asked for a number. The code then prints whether it is a negative  number or a nonnegative number. Here are two examples of what the output would look like:

```python
Enter a number: -33
-33.0 is a negative number
Enter a number: 14
14.0 is a nonnegative number
```

In order to choose from more than one option, it is possible to nest `if-else` statements, which  means putting one inside of another.

```python
num = input('Enter a number: ') 
num = float(num) # convert the string to a float 
if num < 0: 
    print(f'{num} is a negative number') 
else: 
    if num == 0: 
        print('It is a zero') 
    else: 
        print(f'{num} is a positive number')
```

When this is executed, the expression `num < 0` is evaluated. If that is `True`, the code prints that it is a negative number and the entire `if-else` statement ends. If, however, it is `False`, the action consisting of the second `if-else` statement is executed. The second, nested, `if-else` statement evaluates the expression `num == 0`. If that expression is `True`, it prints that it is a zero, and if not it prints that it is a positive number.

Notice the indentation. If we get sloppy with our indentation, we might see the following:

```python
num = input('Enter a number: ') 
num = float(num) # convert the string to a float 
if num < 0: 
    print(f'{num} is a negative number') 
else: 
if num == 0: 
    print('It is a zero') 
else: 
    print(f'{num} is a positive number')
```

Output

```
File "/tmp/ipython-input-4246442818.py", line 6
  if num == 0:
  ^
IndentationError: expected an indented block after 'else' statement on line 5
```

Is there an easier way to implement the logic of dividing our search into three cases, positive, negative, and 0? This can be simplified using the `elif` keyword, as follows.

```python
num = input('Enter a number: ') 
num = float(num) # convert the string to a float 
if num < 0: 
    print(f'{num} is a negative number') 
elif num == 0: 
    print('It is a zero') 
else: 
    print(f'{num} is a positive number')
```

With `elif`, it is not necessary to have else followed by another `if-else` statement. The indentation, which is required in Python, is also simpler using `elif`. There can be as many `elif` clauses as necessary to handle all of the possible actions. It is not a requirement to have an `else` at the very end, although it is common to do so to handle ‘none of the above’.

Note that in Python, unlike many languages, relational operators can be chained together. The  expression `a < x < b`, for example, is equivalent to the expression `a < x` `and` `x < b`. The following will test whether or not a variable `num` is in the range from 5 to 10 inclusive.

```python
if 5 <= num <= 10: 
    print('In range') 
else: 
    print('Not in range')
```

Like `if` statements, `if-else` statements can also be used with strings. For example, the following uses the `in` operator to determine whether or not the character `‘x’` is in a string variable `mystr`.

```python
if 'x' in mystr: 
    print('There is an x!') 
else: 
    print('Alas, no x.')
```

`if-else` statement can also be used to print the range of a number, for example from user input:

```python
# Prompt the user to enter a number
ri = int(input("Please enter a number between 0 and 10: "))

print(ri, "is", end=" ")

if 0 <= ri <= 5:
    print("in range 0 to 5 inclusive")
elif 6 <= ri <= 10:
    print("in range 5 through 10")
else:
    print("out of the expected range (0 to 10)")
```

```python
3 is in range 0 to 5 inclusive
```