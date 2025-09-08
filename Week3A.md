# Chapter 4 (Week 3A) – Selection Statements in Python

In programming, we often need to **make choices**:
- Should a piece of code run, or be skipped?
- Should we choose between two or more alternatives?

Statements that let us make these decisions are called **selection statements**.  
In Python, the most common are `if`, `if-else`, and `if-elif-else`.

---

## 3.1 Boolean Expressions

Selection statements are built on conditions that evaluate to either **True** or **False**.

```python
print(3 < 5)        # True
print(10 == 7)      # False
print(8 % 2 == 0)   # True (8 is even)
```

- **Relational operators**: `<`, `>`, `<=`, `>=`, `==`, `!=`
- **Logical operators**: `and`, `or`, `not`

---

## 3.2 The `if` Statement

The `if` statement executes a block of code only when its condition is **True**.

**General form**
```python
if condition:
    action
# rest of code
```

- A colon (`:`) follows the condition.
- The action(s) must be **indented consistently** (4 spaces is standard).
- If the condition is False, the indented block is skipped.

**Example**
```python
num = 33
if num < 50:
    print("It is smaller")
print("And that is it")
```

**Possible output**
```
It is smaller
And that is it
```

---

## 3.3 The `if-else` Statement

Use `if-else` to choose **between two possibilities**. Exactly one branch runs.

```python
num = float(input("Enter a number: "))

if num < 0:
    print(f"{num} is a negative number")
else:
    print(f"{num} is a nonnegative number")
```

**Sample runs**
```
Enter a number: -33
-33.0 is a negative number
```

```
Enter a number: 14
14.0 is a nonnegative number
```

---

## 3.4 The `if-elif-else` Statement

For more than two possibilities, Python provides `elif` (short for *else if*). This avoids deep nesting and improves readability.

```python
num = float(input("Enter a number: "))

if num < 0:
    print("Negative")
elif num == 0:
    print("Zero")
else:
    print("Positive")
```

- You can include **multiple** `elif` clauses.
- The final `else` is optional but useful for “none of the above.”

---

## 3.5 Chained Comparisons

Python allows math-like chained comparisons:

```python
if 5 <= num <= 10:
    print("In range")
else:
    print("Not in range")
```

This is equivalent to `if num >= 5 and num <= 10`.

---

## 3.6 Selection with Strings and Lists

The `in` operator can test membership in strings and lists.

```python
name = input("Enter your name: ")
if "z" in name:
    print("Your name contains a 'z'!")

numbers = [3, 8, 12]
if 8 in numbers:
    print("The list contains 8.")
```

---

## 3.7 Randomized Example

```python
from random import randint

ri = randint(0, 10)
print(ri, "is", end=" ")

if ri < 5:
    print("in range 0 to 5 inclusive")
else:
    print("in range 5 through 10")
```

**Possible output**
```
3 is in range 0 to 5 inclusive
```

---

## 3.8 Common Beginner Mistakes

- **Forgetting the colon** (`:`) after the condition.
- **Incorrect indentation** (Python requires consistent 4 spaces).
- **Mixing up `=` and `==`** (`=` assigns, `==` tests for equality).

---

## Key Takeaways

- `if`: run code if condition is True.  
- `if-else`: choose between two alternatives.  
- `if-elif-else`: handle multiple possibilities.  
- Indentation matters!  
- Boolean expressions drive all decisions.  

Selection statements let programs **make choices** — the foundation for building interactive and intelligent behavior.

---

👉 Next up: **Loops** — repeating actions until a condition changes.
