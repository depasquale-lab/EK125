# Week 10A: File I/O and File Structure

## Learning Objectives
By the end of this reading, you should understand:
- Why programs need to read and write files
- How to open and close files properly in Python
- The difference between text and binary files
- How to read from files using different methods
- How to write data to files
- Best practices for working with files
- How to handle common file-related errors

## 1. What Is a File?

Before we discuss reading and writing files in Python, we need to understand what a file actually is.

### Files on Your Computer

When you use your computer, you interact with files constantly:
- Documents you write (essays, reports)
- Photos you take
- Music you download
- Programs you run
- Spreadsheets with data

Each file is a collection of data stored on your computer's hard drive or solid-state drive. Unlike data in your program's memory (variables, lists, etc.), files persist even after your program ends or your computer turns off.

### The File System

Your computer organizes files in a tree structure (technically a Directed Acyclic Graph, or DAG):

```
Your Computer
├── Documents/
│   ├── essay.txt
│   └── research/
│       └── notes.txt
├── Pictures/
│   └── vacation.jpg
└── Downloads/
    └── data.csv
```

This structure is like a set of nested folders (also called directories). Each file has a specific location in this structure. The "acyclic" part means you cannot have loops - a folder cannot contain itself, and you cannot navigate in circles. This makes sense: imagine if Documents contained a folder that somehow contained Documents again. You would get lost quickly.

### How You Find Files vs How They're Stored

Most modern computer users find files by searching: you type "essay" in the search bar and the computer finds "essay.txt" wherever it lives. This is convenient, but it hides the actual organization of your computer.

**Under the hood, your computer uses a tree structure.** Every file exists at a specific location in this tree, defined by its path from the root (the top of the tree) down through branches (directories) to the file itself.

**What is a path?** A path is simply the sequence of directories you must walk through to reach a file, just like a path through a forest tells you which way to walk to reach your destination. If you want to reach a cabin deep in the woods, you might walk: "Start at the trailhead, take the left fork, continue to the meadow, then follow the stream." A file path works the same way: "Start at Documents, go into the research folder, then find notes.txt."

In code, we write paths using slashes to separate each step. This is operating system dependent:
- **Windows**: Uses backslashes `\` (e.g., `"Documents\research\notes.txt"`)
- **macOS and Linux**: Use forward slashes `/` (e.g., `"Documents/research/notes.txt"`)

However, **Python accepts forward slashes `/` on all operating systems**, including Windows. This means you should always use forward slashes in your Python code for portability:
```python
# Good: Works everywhere
"Documents/research/notes.txt"

# Avoid: Only works on Windows, and requires escaping
"Documents\\research\\notes.txt"
```

The reason Windows paths need double backslashes `\\` is that a single backslash is an escape character in Python strings (like `\n` for newline). Using forward slashes avoids this complication entirely.

This reads as: "Starting from where I am now, go into Documents, then into research, then access notes.txt."

Think of it like a building:
- **Search approach**: "Find the office where Alice works" - you search the entire building
- **Tree approach**: "Alice's office is in Building A, Floor 3, Room 301" - you follow a specific path

When you write programs that work with files, you cannot just search for "essay.txt" and hope Python finds it. You must understand where the file lives in the tree structure and provide the path to reach it - the sequence of folders to walk through.

```python
# This is what search-based thinking looks like (doesn't work):
# "Just find essay.txt somewhere"

# This is what tree-based thinking looks like (works):
# "essay.txt is in the Documents folder"
filePath = "Documents/essay.txt"
```

The tree structure matters because:
1. Multiple files can have the same name in different locations
2. Your program starts from a specific location in the tree (the working directory)
3. You need to tell Python how to navigate from that starting point to your file

**Example of why this matters:**

```
Your Computer
├── Documents/
│   └── data.txt          # Contains today's experiment data
├── Downloads/
│   └── data.txt          # Contains old reference data
└── Projects/
    └── analysis.py       # Your Python program (working directory)
```

If your program just asks for "data.txt", which one should Python open? The computer cannot guess. You must specify the path: `"Documents/data.txt"` or `"Downloads/data.txt"`.

### Navigating the Tree

When you specify a file path, you're giving directions through the tree:

- `"data.txt"` - "Look in the current directory"
- `"data/readings.txt"` - "Go into the data subdirectory, then find readings.txt"
- `"../output.txt"` - "Go up one level to the parent directory, then find output.txt"
- `"../../Desktop/file.txt"` - "Go up two levels, then into Desktop"

The `..` notation means "parent directory" (move up one level in the tree). This is how you navigate relative to where your program is running.

### What's Inside a File?

At the most basic level, files contain sequences of bytes (numbers from 0 to 255). How these bytes are interpreted depends on the file type:

- **Text files** (.txt, .py, .csv): Bytes represent readable characters using an encoding like ASCII or UTF-8. When you open a .txt file, your computer interprets the bytes as letters, numbers, and punctuation.
- **Binary files** (.jpg, .exe, .pdf): Bytes represent specialized data formats. These files are not meant to be read as text.

For example, the text "Hi" in a file is stored as bytes:
- 'H' → byte value 72
- 'i' → byte value 105

When Python reads a text file, it automatically converts these bytes back into the characters you expect.

### Memory vs Storage

Understanding the difference between memory and storage is crucial:

```
MEMORY (RAM)                    STORAGE (Hard Drive/SSD)
- Fast                          - Slower
- Temporary                     - Permanent
- Lost when program ends        - Persists after program ends
- Where variables live          - Where files live
- Limited capacity              - Much larger capacity
```

When your Python program runs:
1. Your code and variables exist in memory
2. Files exist in storage
3. To use file data, you must read it from storage into memory
4. To save data permanently, you must write it from memory to storage

## 2. Why File I/O Matters

Until now, your programs have lost all their data when they finish running. File Input/Output (I/O) allows programs to:
- Store data permanently between program runs
- Process large datasets that cannot fit in memory all at once
- Share data between different programs
- Create reports and logs
- Read configuration settings

Think of file I/O as giving your program a filing cabinet where it can store and retrieve information.

### Real-World Scenarios

**Scenario 1: Saving game progress**
When you play a video game and save your progress, the game writes your current state (level, score, inventory) to a file. When you restart the game, it reads that file to restore your progress.

**Scenario 2: Processing experimental data**
You run an experiment that produces thousands of measurements. Rather than typing them all into your program, you store them in a file. Your Python program reads the file, analyzes the data, and writes results to a new file.

**Scenario 3: Configuration**
An application needs to remember user preferences (theme color, language, default directory). These settings are stored in a file that the program reads each time it starts.

## 3. File Paths and Location

Before reading or writing files, you need to know where they are located.

### Absolute vs Relative Paths

This is one of the most important concepts to understand when working with files. The difference between absolute and relative paths determines how your program finds files.

#### Absolute Paths

An **absolute path** is the complete address of a file from the root of the file system. It specifies every directory from the very top level down to your file. Think of it like a complete mailing address: "123 Main Street, Boston, MA 02115, USA" - no matter where you are in the world, this address points to exactly one location.

```python
# Absolute paths start from the root of the file system
# On macOS/Linux (start with /):
absolutePath = "/Users/alice/Documents/data.txt"

# On Windows (start with drive letter):
absolutePath = "C:/Users/alice/Documents/data.txt"
```

**Advantages of absolute paths:**
- Unambiguous - always points to the same file
- Works regardless of where your program runs from

**Disadvantages of absolute paths:**
- Not portable - won't work on someone else's computer (they don't have `/Users/alice/`)
- Fragile - breaks if you move your project to a different location
- Verbose - requires typing the entire path

#### Relative Paths

A **relative path** specifies the location of a file relative to your current working directory (where your program is running from). Think of it like giving directions from where you are standing: "Go two blocks north, then turn left." These directions only make sense relative to your starting position.

```python
# Relative paths start from the current working directory
relativePath = "data.txt"                  # Same directory as your program
relativePath = "./data.txt"                # Explicitly "current directory" (same as above)
relativePath = "data/readings.txt"         # In a subdirectory called data
relativePath = "./data/readings.txt"       # Same as above, explicit notation
relativePath = "../output.txt"             # In the parent directory
relativePath = "../../results.csv"         # Two directories up
```

**Note on the `./` notation:** The dot `.` means "current directory", so `"./data.txt"` and `"data.txt"` are identical. Some programmers prefer the explicit `./` notation to make it clear they're using a relative path, but it's optional. Both work the same way.

**Understanding relative paths with an example:**

Imagine your project structure looks like this:
```
myProject/
├── code/
│   └── analysis.py       # Your program runs here
├── data/
│   └── readings.txt
├── output/
│   └── results.txt
└── config.txt
```

If `analysis.py` is running (making `code/` the working directory), the relative paths would be:

```python
# To access config.txt (one level up, then into root):
path = "../config.txt"

# To access readings.txt (one level up, then into data/):
path = "../data/readings.txt"

# To access results.txt (one level up, then into output/):
path = "../output/results.txt"
```

**How to read relative paths:**
- `"data.txt"` means "in my current directory"
- `"./data.txt"` means "in my current directory" (explicit notation, same as above)
- `"data/readings.txt"` means "go into data subdirectory, then find readings.txt"
- `"../output.txt"` means "go up one directory level, then find output.txt"
- `"../../file.txt"` means "go up two directory levels, then find file.txt"

**Advantages of relative paths:**
- Portable - your project can move anywhere and still work
- Flexible - someone else can run your code without changes
- Concise - shorter to write

**Disadvantages of relative paths:**
- Context-dependent - only work relative to where your program runs
- Can be confusing if you don't understand the working directory

#### Which Should You Use?

**For this class, use relative paths.** Keep your data files in the same directory as your Python programs, or in a subdirectory like `data/`. This makes your code portable and easy for others (including your TAs) to run.

```python
# Good: Relative path, works on any computer
with open("data.txt", "r") as fileHandle:
    content = fileHandle.read()

# Good: Relative path with subdirectory
with open("data/readings.txt", "r") as fileHandle:
    content = fileHandle.read()

# Avoid: Absolute path, only works on your computer
with open("/Users/alice/Documents/class/data.txt", "r") as fileHandle:
    content = fileHandle.read()
```

### Working Directory

Your program runs from a specific directory called the working directory. Relative paths start from here.

```python
import os

# Find out where your program is running from
currentDirectory = os.getcwd()
print(currentDirectory)
```

## 4. Opening and Closing Files

The basic pattern for working with files involves three steps: open, process, close.

**CRITICAL CONCEPT: File operations are expensive.** Opening and closing files is much slower than working with data in memory. Every time you open a file, the operating system must:
1. Locate the file on the physical disk
2. Check permissions
3. Allocate system resources (file descriptors, buffers)
4. Load metadata about the file

Every time you close a file, the system must:
1. Flush any buffered data to disk
2. Update file metadata (modification time, etc.)
3. Release system resources

This takes time - much more time than you might think.

**Even worse: Opening files in write mode ("w") is destructive.** Every time you open a file in write mode, you erase its entire contents. If you accidentally open-close-open-close in a loop, you can destroy data.

### Common Mistakes That Destroy Performance or Data

**Mistake 1: Opening file inside a loop**
```python
# TERRIBLE: Opening file inside loop (hundreds of slow operations)
for i in range(100):
    with open("output.txt", "a") as fileHandle:
        fileHandle.write(f"Line {i}\n")
# This opens and closes the file 100 times! Extremely slow. If done quickly and repeatedly, ***WILL*** wreck an SD card in just a few hours.
```

**Mistake 2: Opening in write mode repeatedly**
```python
# TERRIBLE: Opening in write mode repeatedly (destroys data!)
with open("data.txt", "w") as fileHandle:
    fileHandle.write("First line\n")

with open("data.txt", "w") as fileHandle:  # ERASES EVERYTHING!
    fileHandle.write("Second line\n")
# Result: File only contains "Second line\n"
```

**The correct approach:**
```python
# GOOD: Open once, process, close once
with open("output.txt", "w") as fileHandle:
    for i in range(100):
        fileHandle.write(f"Line {i}\n")
# Opens file once, writes 100 times, closes once. Fast!
```

**The principle: Open the file once, do all your work, then close it.** Treat file operations as precious and rare. Minimize how often you open and close files.

### The open() Function

```python
# Open a file for reading
fileHandle = open("data.txt", "r")

# Open a file for writing
fileHandle = open("output.txt", "w")

# Open a file for appending
fileHandle = open("log.txt", "a")
```

### File Modes

| Mode | Description | Creates file? | Erases existing? |
|------|-------------|---------------|------------------|
| "r"  | Read only   | No (error if missing) | No |
| "w"  | Write only  | Yes | Yes |
| "a"  | Append      | Yes | No |
| "r+" | Read and write | No | No |

### Closing Files

Always close files when done:

```python
fileHandle = open("data.txt", "r")
# ... do something with the file ...
fileHandle.close()
```

**Critical concept**: If you do not close a file after writing, your data might not be saved properly. The operating system buffers (temporarily stores) file writes, and closing the file ensures everything is written to disk.

## 5. The with Statement (Best Practice)

Python provides a safer way to work with files that automatically closes them:

```python
with open("data.txt", "r") as fileHandle:
    content = fileHandle.read()
    print(content)
# File is automatically closed here, even if an error occurs
```

The `with` statement guarantees the file closes properly, even if your code encounters an error. This is the preferred approach and what you should use in your programs.

## 6. Reading from Files

There are several ways to read file content, each suited for different situations.

### Reading the Entire File

```python
with open("story.txt", "r") as fileHandle:
    content = fileHandle.read()
    print(content)  # Prints entire file as one string
```

Use this when:
- The file is small enough to fit in memory
- You need to process the entire content at once

### Reading Line by Line

```python
with open("data.txt", "r") as fileHandle:
    for line in fileHandle:
        print(line.strip())  # strip() removes the newline character
```

Use this when:
- Processing large files
- You need to handle each line separately
- Memory efficiency matters

### Reading All Lines into a List

```python
with open("names.txt", "r") as fileHandle:
    allLines = fileHandle.readlines()
    
# allLines is now a list where each element is one line
for line in allLines:
    print(line.strip())
```

Use this when:
- You need random access to lines
- The file is small enough for memory
- You want to process lines multiple times

### Reading a Specific Number of Lines

```python
with open("data.txt", "r") as fileHandle:
    firstLine = fileHandle.readline()
    secondLine = fileHandle.readline()
    print(firstLine.strip())
    print(secondLine.strip())
```

## 7. Writing to Files

### Writing Strings

```python
with open("output.txt", "w") as fileHandle:
    fileHandle.write("Hello, World!\n")
    fileHandle.write("This is a second line.\n")
```

**Important**: `write()` does not add newline characters automatically. You must include `\n` explicitly.

### Writing Multiple Lines

```python
lines = ["First line\n", "Second line\n", "Third line\n"]

with open("output.txt", "w") as fileHandle:
    fileHandle.writelines(lines)
```

### Appending to Files

```python
with open("log.txt", "a") as fileHandle:
    fileHandle.write("New log entry\n")
```

Mode "a" adds content to the end without erasing existing data.

## 8. Processing Structured Text Files

Many files contain structured data where each line follows a pattern.

### Example: Reading CSV-like Data

```python
# File content:
# Alice,85,92,88
# Bob,78,84,91

with open("grades.txt", "r") as fileHandle:
    for line in fileHandle:
        line = line.strip()
        parts = line.split(",")
        name = parts[0]
        scores = [int(parts[1]), int(parts[2]), int(parts[3])]
        average = sum(scores) / len(scores)
        print(f"{name}: {average:.1f}")
```

### Writing Structured Data

```python
students = [
    ("Alice", 85, 92, 88),
    ("Bob", 78, 84, 91),
    ("Charlie", 90, 87, 93)
]

with open("grades.txt", "w") as fileHandle:
    for student in students:
        name = student[0]
        score1 = student[1]
        score2 = student[2]
        score3 = student[3]
        line = f"{name},{score1},{score2},{score3}\n"
        fileHandle.write(line)
```

## 9. Common Patterns and Idioms

### Counting Lines in a File

```python
# Open the file for reading
with open("data.txt", "r") as fileHandle:
    lineCount = 0
    # Process each line one at a time
    for line in fileHandle:
        lineCount = lineCount + 1
    # After loop completes, lineCount holds the total
    print(f"File has {lineCount} lines")
```

### Finding Specific Content

```python
# Search through a file for lines containing specific text
with open("data.txt", "r") as fileHandle:
    for line in fileHandle:
        # Convert to lowercase for case-insensitive search
        if "temperature" in line.lower():
            # Strip whitespace before printing
            print(line.strip())
```

### Creating a Summary

```python
total = 0
count = 0

# Read numbers from a file and calculate average
with open("numbers.txt", "r") as fileHandle:
    for line in fileHandle:
        # Convert string to number after stripping whitespace
        number = float(line.strip())
        total = total + number
        count = count + 1

# Calculate average after processing all numbers
average = total / count
print(f"Average: {average:.2f}")
```

### Reading and Processing in One Pass

```python
validEmails = []

# Filter data while reading (don't store everything)
with open("contacts.txt", "r") as fileHandle:
    for line in fileHandle:
        email = line.strip()
        # Basic validation: must contain @ and at least one dot
        if "@" in email and "." in email:
            validEmails.append(email)

# Now validEmails contains only the valid ones
print(f"Found {len(validEmails)} valid emails")
```

## 10. File Structure and Organization

As your programs grow, organizing files becomes important.

### Common Directory Structures

```
project/
    main.py           # Your main program
    data/             # Input data files
        input1.txt
        input2.txt
    output/           # Generated output files
        results.txt
    logs/             # Log files
        activity.log
```

### Creating Directories

```python
import os

if not os.path.exists("output"):
    os.makedirs("output")

with open("output/results.txt", "w") as fileHandle:
    fileHandle.write("Results go here\n")
```

## 11. Best Practices

1. **Always use the `with` statement** when working with files
2. **Close files explicitly** if you cannot use `with`
3. **Use relative paths** when possible for portability
4. **Check if files exist** before trying to read them (see Appendix)
5. **Use meaningful variable names** for file handles: `inputFile`, `outputFile`, not just `f`
6. **Add newlines explicitly** when writing text
7. **Strip whitespace** when reading lines that you will process
8. **Comment your file operations** to explain the file format

## 12. Common Mistakes to Avoid

```python
# WRONG: File not closed if error occurs
fileHandle = open("data.txt", "r")
content = fileHandle.read()
# If an error happens here, the file never closes
fileHandle.close()

# RIGHT: Use with statement
with open("data.txt", "r") as fileHandle:
    content = fileHandle.read()
```

```python
# WRONG: Forgetting newlines
with open("output.txt", "w") as fileHandle:
    fileHandle.write("Line 1")
    fileHandle.write("Line 2")
# Results in: "Line 1Line 2"

# RIGHT: Add newlines explicitly
with open("output.txt", "w") as fileHandle:
    fileHandle.write("Line 1\n")
    fileHandle.write("Line 2\n")
```

```python
# WRONG: Not stripping whitespace when processing
with open("numbers.txt", "r") as fileHandle:
    for line in fileHandle:
        number = int(line)  # Will fail due to newline character

# RIGHT: Strip whitespace first
with open("numbers.txt", "r") as fileHandle:
    for line in fileHandle:
        number = int(line.strip())
```

## Questions to Consider

1. Why might you want to process a file line-by-line instead of reading it all at once?
2. What happens if you open a file in write mode ("w") and the file already exists?
3. Why is the `with` statement safer than manually opening and closing files?
4. How would you modify a program that reads from one file and writes to another?

## Key Terms to Remember

- **File I/O**: Input/Output operations for reading and writing files
- **File Handle**: An object that represents an open file
- **Path**: The location of a file in the file system
- **Working Directory**: The directory from which your program runs
- **Mode**: Specifies how to open a file (read, write, append)
- **with Statement**: Python construct that ensures proper file closure
- **Buffer**: Temporary storage used by the OS for file operations
- **Text File**: File containing readable character data
- **Binary File**: File containing raw byte data

---

## Appendix A: Text vs Binary Files (Background Information)

This section provides background on file types. For this class, we will focus exclusively on text files, but understanding the distinction helps you recognize what kind of files you're working with.

### What Python Sees

Python distinguishes between text and binary files based on how it interprets the bytes in the file.

### Text Files

**Text files** contain human-readable characters encoded using a character encoding system (usually UTF-8 or ASCII).

**Characteristics:**
- Contain readable characters (letters, numbers, punctuation)
- Use encoding to convert bytes to characters
- Can be opened in any text editor
- Lines typically end with newline characters (`\n`)
- Examples: `.txt`, `.csv`, `.py`, `.html`, `.json`, `.md` files

**How to open:**
```python
# Default mode is text
with open("data.txt", "r") as fileHandle:
    content = fileHandle.read()  # Returns a string
```

**What Python does:**
- Reads bytes from disk
- Converts bytes to characters using UTF-8 encoding
- Handles newline characters appropriately for your OS
- Returns strings that you can work with directly

### Binary Files

**Binary files** contain raw bytes that represent specialized data formats. These bytes are not meant to be interpreted as text characters.

**Characteristics:**
- Contain raw byte data
- Not human-readable when opened in a text editor
- No character encoding applied
- Examples: `.jpg`, `.png`, `.mp3`, `.exe`, `.pdf`, `.zip` files

**How to open:**
```python
# Note the 'b' in the mode string
with open("image.jpg", "rb") as fileHandle:
    content = fileHandle.read()  # Returns bytes, not a string
```

**What Python does:**
- Reads raw bytes from disk
- No character encoding or conversion
- Returns a `bytes` object instead of a string

### Why the Distinction Matters

If you try to open a binary file in text mode, you'll get errors or garbage:

```python
# WRONG: Opening a binary file in text mode
with open("photo.jpg", "r") as fileHandle:
    content = fileHandle.read()  # Error or garbage characters
```

If you open a text file in binary mode, you get bytes instead of strings:

```python
# Opens successfully but gives you bytes
with open("data.txt", "rb") as fileHandle:
    content = fileHandle.read()  # Returns b'Hello\n' instead of 'Hello\n'
```

### For This Class

**We will only work with text files.** Always use:
- `"r"` for reading text files
- `"w"` for writing text files
- `"a"` for appending to text files

You do not need to use binary mode (`"rb"`, `"wb"`) in this course.

### How to Tell File Types Apart

**Text files** usually have extensions like:
- `.txt` - plain text
- `.csv` - comma-separated values
- `.py` - Python source code
- `.html` - web pages
- `.json` - structured data
- `.md` - markdown documents

**Binary files** usually have extensions like:
- `.jpg`, `.png`, `.gif` - images
- `.mp3`, `.wav` - audio
- `.mp4`, `.avi` - video
- `.pdf` - documents
- `.exe` - programs
- `.zip` - compressed archives

When in doubt, try opening the file in a text editor. If you see readable text, it's a text file. If you see strange symbols or boxes, it's binary.

## Appendix B: Error Handling (Preview)

This section introduces concepts you will learn later in the course. You do not need to master these now, but they are included for reference.

### Why File Operations Can Fail

File operations can fail for many reasons:
1. **File doesn't exist**: You try to read "data.txt" but it's not in the expected location
2. **Permission denied**: The operating system won't let you read or write the file (e.g., system files, files owned by other users)
3. **Disk full**: No space left to write data
4. **File in use**: Another program has locked the file
5. **Path doesn't exist**: You try to write to "output/results.txt" but the "output" directory doesn't exist

### What Happens When Operations Fail

When Python encounters a file error, it raises an **exception** - a special signal that something went wrong. If you don't handle the exception, your program crashes with an error message.

```python
# This will crash if the file doesn't exist
with open("missing.txt", "r") as fileHandle:
    content = fileHandle.read()
# Error: FileNotFoundError: [Errno 2] No such file or directory: 'missing.txt'
```

### The try-except Block (Preview)

Python provides a way to catch and handle errors gracefully using `try-except` blocks. This is like saying "try to do this, but if it fails, do something else instead of crashing."

**Structure:**
```python
try:
    # Code that might fail
    risky_operation()
except SomeError:
    # What to do if it fails
    handle_the_error()
```

### Example 1: File Not Found

```python
# Try to open a file, but handle the case where it doesn't exist
try:
    with open("missing.txt", "r") as fileHandle:
        content = fileHandle.read()
        print(content)
except FileNotFoundError:
    # This code runs ONLY if the file doesn't exist
    print("Error: File does not exist")
    # Program continues instead of crashing
```

**What's happening:**
1. Python tries to execute the code in the `try` block
2. If `open()` cannot find the file, it raises a `FileNotFoundError`
3. Instead of crashing, Python jumps to the `except` block
4. The error message prints, and the program continues

### Example 2: Permission Errors

**What is a permission error?** Operating systems control who can read, write, or execute files. If you try to write to a file you don't have permission to modify (like system files, or files owned by another user), you get a `PermissionError`.

```python
# Try to write to a protected file
try:
    with open("protected.txt", "w") as fileHandle:
        fileHandle.write("data")
except PermissionError:
    # This runs if you don't have permission
    print("Error: Cannot write to this file")
```

**Common causes of permission errors:**
- Trying to modify system files
- Writing to directories you don't own
- Opening a file that another program has locked
- On shared computers, accessing other users' files

### Example 3: Safe File Operations

Instead of using try-except, you can check if a file exists before trying to open it:

```python
import os

filename = "data.txt"

# Check if file exists before trying to open it
if os.path.exists(filename):
    with open(filename, "r") as fileHandle:
        content = fileHandle.read()
        print(content)
else:
    print(f"File {filename} does not exist")
```

**The `os.path.exists()` function** checks whether a file or directory exists, returning `True` or `False`. This is useful when you want to do different things depending on whether the file exists.

### When to Use Each Approach

**Use `os.path.exists()` when:**
- You want to check before attempting the operation
- The file might legitimately not exist
- You need to create the file if it's missing

**Use try-except when:**
- You expect the operation to usually succeed
- You need to handle multiple types of errors
- You're dealing with permissions or other runtime issues

### Multiple Exception Types

You can catch different types of errors separately:

```python
try:
    with open("data.txt", "r") as fileHandle:
        content = fileHandle.read()
        value = int(content)  # Might fail if content isn't a number
except FileNotFoundError:
    print("File doesn't exist")
except PermissionError:
    print("Don't have permission to read this file")
except ValueError:
    print("File contents are not a valid number")
```

### Why This Matters

When you write programs that others will use, handling errors gracefully makes your program more professional:
- Instead of crashing with cryptic messages, give users helpful feedback
- Allow the program to continue even when one operation fails
- Log errors for debugging while keeping the program running

You will learn more about exception handling later in the course. For now, focus on understanding that file operations can fail, and Python provides mechanisms to handle these failures.