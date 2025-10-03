# Week 6A Pre-read: Mastering Slicing in Python

## Learning Objectives
By the end of this reading, you should understand:
- How to use slice notation to extract portions of sequences
- The rules governing start, end, and step parameters in slices
- How to use negative indices within slices
- Common slicing patterns and idioms
- How slice assignment works for mutable sequences
- When slicing creates copies versus references

## 0. Review: Sequence Operators

Before we dive into slicing, let's review two operators that work on sequences. These should have been covered in Week 2, but we'll briefly (re?)introduce them here since they're useful when working with slices.

The `+` operator concatenates (joins) sequences together:

```python
listA = [1, 2, 3]
listB = [4, 5, 6]
combined = listA + listB
print(combined)  # [1, 2, 3, 4, 5, 6]

greeting = "Hello" + " " + "World"
print(greeting)  # "Hello World"
```

The `*` operator repeats a sequence:

```python
zeros = [0] * 5
print(zeros)  # [0, 0, 0, 0, 0]

dashes = "-" * 10
print(dashes)  # "----------"

pattern = [1, 2] * 3
print(pattern)  # [1, 2, 1, 2, 1, 2]
```

These operators will be useful later when we combine slices to build new sequences.

## 1. Review: Indexing Sequences

Recall from Weeks 2 and 5 that Python sequences (strings, lists, tuples) can be accessed by index:

```python
word = "Python"
print(word[0])      # 'P'
print(word[-1])     # 'n'

numbers = [10, 20, 30, 40, 50]
print(numbers[2])   # 30
print(numbers[-2])  # 40
```

Indexing lets us access a single element. But what if we want to extract multiple consecutive elements? That's where slicing comes in.

## 2. Simplest Slice Notation

A **slice** extracts a portion of a sequence. The simplest syntax is:

```python
sequence[start:end]
```

This extracts elements from index `start` up to (but not including) index `end`.

```python
word = "Programming"
print(word[0:4])    # "Prog"
print(word[3:7])    # "gram"
print(word[5:11])   # "amming"

numbers = [10, 20, 30, 40, 50]
print(numbers[1:4]) # [20, 30, 40]
print(numbers[0:2]) # [10, 20]
print(numbers[2:5]) # [30, 40, 50]

colors = ["red", "green", "blue", "yellow", "orange"]
print(colors[1:3])  # ["green", "blue"]
print(colors[0:4])  # ["red", "green", "blue", "yellow"]
```

**Key rule: The end index is NOT included.** Think of it as "start here, stop before end."

### Why "up to but not including"?

This convention is counterintuitive at first. When you hear "elements 0 through 4," you naturally expect to get element 4. Python doesn't work this way, and it takes time to adjust.

The payoff is that consecutive slices connect seamlessly:

```python
word = "Programming"
first = word[0:4]   # "Prog"
second = word[4:8]  # "ramm"
third = word[8:]    # "ing"
print(first + second + third)  # "Programming"
```

Notice that the second slice starts at 4, exactly where the first slice ended. And the third slice starts at 8, exactly where the second slice ended.

Another benefit: the length of any slice is simply `end - start`.

## 3. Omitting Start and End

At the cost of some readability, you can omit `start` or `end` to use default values:
- Omitting `start` defaults to 0 (beginning of sequence)
- Omitting `end` defaults to the length (end of sequence)

Python doesn't provide an explicit `end` keyword to refer to the end of a sequence, so when you want to slice to the end, we're effectively forced to omit the `end` value. This makes our slices more compact but less explicit. (Note that `-1` is not a suitable proxy for a specific `end`, as depending on the context it can mean the last element or the second-to-last element.)

```python
word = "Python"
print(word[:3])     # "Pyt" (same as word[0:3])
print(word[3:])     # "hon" (from 3 to end)
print(word[:])      # "Python" (entire sequence)

numbers = [10, 20, 30, 40, 50]
print(numbers[:2])  # [10, 20]
print(numbers[2:])  # [30, 40, 50]
print(numbers[3:])  # [40, 50]
print(numbers[:4])  # [10, 20, 30, 40]

# Identical to the above, but showing the non-omitted versions
numbers = [10, 20, 30, 40, 50]
print(numbers[0:2])  # [10, 20]
print(numbers[2:len(numbers)])  # [30, 40, 50]
print(numbers[3:len(numbers)])  # [40, 50]
print(numbers[0:4])  # [10, 20, 30, 40]

message = "Hello World"
print(message[:5])  # "Hello"
print(message[6:])  # "World"
```

FYI: The slice `[:]` creates a copy of the entire sequence.

## 4. Negative Indices in Slices

Just like with single-element indexing, you can use negative numbers in slices to count from the end:

```python
word = "Programming"
# (Below are the letters of `Programming` matched up with its positive/negative indices)
#  P   r   o   g   r   a   m   m   i   n   g
#  0   1   2   3   4   5   6   7   8   9  10
# -11 -10 -9  -8  -7  -6  -5  -4  -3  -2  -1

print(word[-4:])        # "ming" (last 4 characters)
print(word[-7:-2])      # "ammin"
print(word[:-3])        # "Programmi" (all but last 3)
print(word[-5:])        # "mming" (last 5 characters)
```

Common patterns:
- `sequence[-n:]` - last n elements
- `sequence[:-n]` - all but last n elements
- `sequence[-n:-m]` - slice from nth-to-last to mth-to-last

```python
numbers = [10, 20, 30, 40, 50, 60, 70]
print(numbers[-3:])     # [50, 60, 70]
print(numbers[:-2])     # [10, 20, 30, 40, 50]
print(numbers[-5:-2])   # [30, 40, 50]
print(numbers[-1:])     # [70] (last element as a list)
print(numbers[:-1])     # [10, 20, 30, 40, 50, 60] (all but last)

text = "Hello World"
print(text[-5:])        # "World"
print(text[:-6])        # "Hello"
print(text[-11:-6])     # "Hello"
```

## 5. The Step Parameter

The full slice syntax includes a third parameter:

```python
sequence[start:end:step]
```

The `step` parameter determines how many indices to move forward each time. The default step is 1.

```python
numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
print(numbers[0:8:2])   # [0, 2, 4, 6] (every 2nd element)
print(numbers[1:9:3])   # [1, 4, 7] (every 3rd element)
print(numbers[::2])     # [0, 2, 4, 6, 8] (every 2nd, entire sequence)
print(numbers[0:10:3])  # [0, 3, 6, 9] (every 3rd element)
print(numbers[1::2])    # [1, 3, 5, 7, 9] (odd indices)

alphabet = "abcdefghijklmnop"
print(alphabet[0:10:2]) # "acegi" (every 2nd char in first 10)
print(alphabet[::3])    # "adgjm" (every 3rd character)
print(alphabet[1:15:4]) # "bfj" (every 4th starting from index 1)
```

You can omit `start` and `end` while specifying `step`:

```python
letters = "abcdefghij"
print(letters[0::3])    # "adgj" (every 3rd character, starting from 0)
print(letters[::3])     # "adgj" (every 3rd character)
print(letters[1::3])    # "beh" (every 3rd character, starting from 1)
print(letters[2::3])    # "cfi" (every 3rd character, starting from 2)
```

## 6. Negative Steps (Reversing)

A negative `step` moves backwards through the sequence:

```python
word = "Python"
print(word[::-1])       # "nohtyP" (reversed)

numbers = [1, 2, 3, 4, 5]
print(numbers[::-1])    # [5, 4, 3, 2, 1]

alphabet = "abcdefgh"
print(alphabet[::-1])   # "hgfedcba"
print(alphabet[::-2])   # "hfdb" (every 2nd element, backwards)
```

The slice `[::-1]` is a Python idiom for reversing any sequence.

You can combine negative steps with start and end:

```python
word = "Programming"
print(word[8:2:-1])     # "immarg" (from index 8 down to 3)
print(word[-1:-8:-2])   # "gimr" (backwards, every 2nd)
print(word[10:5:-1])    # "gnimm" (from index 10 down to 6)

numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
print(numbers[9:4:-1])  # [9, 8, 7, 6, 5] (from 9 down to 5)
print(numbers[7:2:-2])  # [7, 5, 3] (from 7 down to 3, every 2nd)
print(numbers[:3:-1])   # [9, 8, 7, 6, 5, 4] (from end down to 4)
```

When using negative steps, remember:
- `start` should be greater than `end` (you're going backwards)
- If omitted, `start` defaults to end of sequence, `end` defaults to beginning

## 7. Common Slicing Patterns

Here are idioms you'll see frequently in Python code:

### Getting first/last n elements
```python
data = [10, 20, 30, 40, 50, 60]
firstThree = data[:3]      # [10, 20, 30]
lastTwo = data[-2:]        # [50, 60]
firstFive = data[:5]       # [10, 20, 30, 40, 50]
lastOne = data[-1:]        # [60]

sentence = "The quick brown fox"
firstWord = sentence[:3]   # "The"
lastWord = sentence[-3:]   # "fox"
```

### Removing first/last n elements
```python
data = [10, 20, 30, 40, 50]
withoutFirst = data[1:]    # [20, 30, 40, 50]
withoutLast = data[:-1]    # [10, 20, 30, 40]
withoutFirstTwo = data[2:] # [30, 40, 50]
withoutLastTwo = data[:-2] # [10, 20, 30]

url = "https://example.com"
withoutProtocol = url[8:]  # "example.com"
```

### Every nth element
```python
numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
evens = numbers[::2]       # [0, 2, 4, 6, 8]
odds = numbers[1::2]       # [1, 3, 5, 7, 9]
everyThird = numbers[::3]  # [0, 3, 6, 9]
everyFourth = numbers[::4] # [0, 4, 8]

days = ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]
weekdays = days[::1]       # All days
weekends = days[5:]        # ["Sat", "Sun"]
everyOtherDay = days[::2]  # ["Mon", "Wed", "Fri", "Sun"]
```

## 8. Slice Assignment (Lists Only)

Slicing works the same way on strings, lists, and tuples. However, only lists allow you to assign to a slice to replace a section:

```python
numbers = [1, 2, 3, 4, 5]
numbers[1:4] = [20, 30, 40]
print(numbers)  # [1, 20, 30, 40, 5]

values = [10, 20, 30, 40, 50, 60]
values[2:5] = [100, 200, 300]
print(values)  # [10, 20, 100, 200, 300, 60]
```

With strings, you cannot assign to slices because strings are immutable:

```python
text = "Hello"
# text[1:4] = "ELL"    # ERROR: strings are immutable
```

The replacement doesn't need to be the same length:

```python
values = [10, 20, 30, 40, 50]
values[1:3] = [100, 200, 300, 400]
print(values)  # [10, 100, 200, 300, 400, 40, 50]

letters = ['a', 'b', 'c', 'd', 'e']
letters[1:4] = ['X']
print(letters)  # ['a', 'X', 'e']
```

You can even delete elements by assigning an empty list:

```python
numbers = [1, 2, 3, 4, 5]
numbers[1:3] = []
print(numbers)  # [1, 4, 5]

data = [10, 20, 30, 40, 50, 60]
data[2:5] = []
print(data)  # [10, 20, 60]
```

Slice assignment with steps requires the replacement to match the slice length:

```python
numbers = [0, 1, 2, 3, 4, 5]
numbers[0::2] = [10, 20, 30]
print(numbers)  # [10, 1, 20, 3, 30, 5]

values = [1, 2, 3, 4, 5, 6, 7, 8]
values[1:7:2] = [100, 200, 300]
print(values)  # [1, 100, 3, 200, 5, 300, 7, 8]

# numbers[::2] = [10, 20]  # ERROR: length mismatch
```

## 9. Building New Sequences from Slices

Slicing is powerful for constructing new sequences from parts of existing ones:

```python
# Extracting parts of a string
email = "student@university.edu"
username = email[:email.index('@')]
domain = email[email.index('@')+1:]  # +1 to skip the '@' character
print(username)  # "student"
print(domain)    # "university.edu"

# Rearranging list elements
data = [1, 2, 3, 4, 5, 6]
rearranged = data[3:] + data[:3]
print(rearranged)  # [4, 5, 6, 1, 2, 3]

# Interleaving sequences
evens = [0, 2, 4, 6]
odds = [1, 3, 5, 7]
combined = [0] * 8
combined[0::2] = evens  # Could write [::2], but explicit 0 shows contrast with 1 below
combined[1::2] = odds
print(combined)  # [0, 1, 2, 3, 4, 5, 6, 7]
```

## 10. Slicing with Variables

Slice indices can be variables, making slicing very flexible:

```python
data = [10, 20, 30, 40, 50, 60, 70, 80]
start = 2
end = 6
step = 2
print(data[start:end:step])  # [30, 50]

begin = 1
finish = 5
print(data[begin:finish])    # [20, 30, 40, 50]

chunkSize = 3
print(data[:chunkSize])      # [10, 20, 30]
print(data[-chunkSize:])     # [60, 70, 80]
```

Finding and extracting:

```python
text = "The quick brown fox"
spaceIndex = text.index(' ')
firstWord = text[:spaceIndex]
rest = text[spaceIndex+1:]  # +1 to skip the space character
print(firstWord)  # "The"
print(rest)       # "quick brown fox"

email = "john.doe@company.com"
atIndex = email.index('@')
username = email[:atIndex]
domain = email[atIndex+1:]
print(username)  # "john.doe"
print(domain)    # "company.com"

path = "/home/user/documents/file.txt"
lastSlash = path.rfind('/')
filename = path[lastSlash+1:]
directory = path[:lastSlash]
print(filename)   # "file.txt"
print(directory)  # "/home/user/documents"
```

## 11. Practical Applications

### Splitting strings into parts
```python
filename = "report_2024_final.pdf"
parts = []
lastIndex = 0
for i in range(len(filename)):
    if filename[i] == '_' or filename[i] == '.':
        parts.append(filename[lastIndex:i])
        lastIndex = i + 1
parts.append(filename[lastIndex:])
print(parts)  # ['report', '2024', 'final', 'pdf']

date = "2024-10-15"
parts = []
lastIndex = 0
for i in range(len(date)):
    if date[i] == '-':
        parts.append(date[lastIndex:i])
        lastIndex = i + 1
parts.append(date[lastIndex:])
year = parts[0]
month = parts[1]
day = parts[2]
print(f"Year: {year}, Month: {month}, Day: {day}")
```

### Processing data in chunks
```python
data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
chunkSize = 3
chunks = []
for i in range(0, len(data), chunkSize):
    chunks.append(data[i:i+chunkSize])  # Extract chunkSize elements starting at i
print(chunks)  # [[1, 2, 3], [4, 5, 6], [7, 8, 9], [10, 11, 12]]

numbers = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
pairSize = 2
pairs = []
for i in range(0, len(numbers), pairSize):
    pairs.append(numbers[i:i+pairSize])
print(pairs)  # [[10, 20], [30, 40], [50, 60], [70, 80], [90, 100]]
```

### Windowing through sequences
```python
temperatures = [72, 75, 78, 76, 74, 71, 69, 73]
windowSize = 3
for i in range(len(temperatures) - windowSize + 1):
    window = temperatures[i:i+windowSize]  # Extract windowSize consecutive elements
    average = sum(window) / len(window)
    print(f"Days {i} to {i+windowSize-1}: {window}, Average: {average:.1f}")

prices = [100, 105, 103, 108, 112, 110, 115]
windowSize = 4
for i in range(len(prices) - windowSize + 1):
    window = prices[i:i+windowSize]
    minPrice = min(window)
    maxPrice = max(window)
    print(f"Window {i}: Range from ${minPrice} to ${maxPrice}")
```

## 12. Edge Cases and Gotchas

### The confusing behavior of -1

When used as a single index, `-1` refers to the last element. But when used as the `end` in a slice, it refers to the position before the last element (the penultimate position):

python

```python
numbers = [10, 20, 30, 40, 50]

# As a single index: -1 is the last element
print(numbers[-1])    # 50

# As the end of a slice: -1 means "up to but not including the last element"
print(numbers[0:-1])  # [10, 20, 30, 40] (excludes 50!)
print(numbers[:-1])   # [10, 20, 30, 40] (same thing)

word = "Python"
print(word[-1])       # "n" (last character)
print(word[:-1])      # "Pytho" (all but last character)
print(word[0:-1])     # "Pytho" (same thing)
```

This is confusing but follows from the "up to but not including" rule. The slice `[0:-1]` means "from 0 up to (but not including) the last element."

### Out-of-bounds indices
Unlike single-element indexing, slicing never raises an error for out-of-bounds indices:

```python
word = "Python"
print(word[10:20])   # "" (empty string, not an error)
print(word[-100:2])  # "Py" (treated as word[0:2])
print(word[2:100])   # "thon" (treated as word[2:6])

numbers = [1, 2, 3]
print(numbers[5:10]) # [] (empty list, not an error)
print(numbers[1:50]) # [2, 3] (goes to end)
print(numbers[-99:2]) # [1, 2] (starts at beginning)
```

### Empty slices
Various slice specifications can result in empty sequences:

```python
data = [1, 2, 3, 4, 5]
print(data[3:3])     # [] (start equals end)
print(data[5:10])    # [] (start beyond length)
print(data[3:1])     # [] (start > end with positive step)
print(data[10:20])   # [] (both indices beyond length)

text = "Hello"
print(text[2:2])     # "" (empty string)
print(text[10:15])   # "" (beyond string length)
```

### Step of zero
A step of zero is not allowed:

```python
numbers = [1, 2, 3, 4, 5]
# print(numbers[::0])  # ERROR: slice step cannot be zero
# print(numbers[0:5:0])  # ERROR: slice step cannot be zero
```has

### Out-of-bounds indices
Unlike single-element indexing, slicing never raises an error for out-of-bounds indices:

```python
word = "Python"
print(word[10:20])   # "" (empty string, not an error)
print(word[-100:2])  # "Py" (treated as word[0:2])

numbers = [1, 2, 3]
print(numbers[5:10]) # [] (empty list, not an error)
```

### Empty slices
Various slice specifications can result in empty sequences:

```python
data = [1, 2, 3, 4, 5]
print(data[3:3])     # [] (start equals end)
print(data[5:10])    # [] (start beyond length)
print(data[3:1])     # [] (start > end with positive step)
```

### Step of zero
A step of zero is not allowed:

```python
numbers = [1, 2, 3, 4, 5]
# print(numbers[::0])  # ERROR: slice step cannot be zero
```

## Key Terms to Remember
- **Slice**: A portion of a sequence extracted using `start:end:step` notation
- **Step**: The increment between indices in a slice
- **Slice assignment**: Replacing a portion of a mutable sequence using slice notation
- **Stride**: Another term for the step value in a slice