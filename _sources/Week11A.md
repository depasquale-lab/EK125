# Week 11 Pre-read: Debugging and Profiling

## Learning Objectives
By the end of this reading, you should understand:
- What debugging is and why it's a critical programming skill
- Common debugging strategies that work across languages
- What code profiling is and why it matters
- How to identify performance bottlenecks in your code
- The relationship between correctness and performance

## 1. Understanding Code Execution Flow

Before you can debug effectively, you need to understand how your code executes. Programs run line by line, but this flow can be interrupted by:
- Function calls (jumping to another piece of code)
- Loops (repeating the same lines multiple times)
- Conditional statements (skipping certain lines)
- Errors (stopping execution entirely)

When debugging, you're trying to find where the actual execution flow differs from what you intended.

## 2. What is Debugging?

Debugging is the process of finding and fixing errors in your code. The term comes from the early days of computing when a moth caused a malfunction in a computer - engineers had to "debug" the machine. Today, we use the term for finding any problem in code.

There are three main types of errors:

**Syntax Errors**: The code violates the language's rules and won't run at all.
```python
# Python example
if x = 5:  # Error: should use == not =
    print("Hello")
```

```matlab
% MATLAB example
if x = 5  % Error: should use == not =
    disp('Hello')
end
```

**Runtime Errors**: The code runs but crashes due to an illegal operation.
```python
# Python example
numbers = [1, 2, 3]
print(numbers[10])  # Error: index out of range
```

```matlab
% MATLAB example
numbers = [1, 2, 3];
disp(numbers(10))  % Error: Index exceeds array dimensions
```

**Logic Errors**: The code runs without crashing but produces incorrect results. These are the hardest to find.
```python
# Python example - calculating average
def calculateAverage(scores):
    total = sum(scores)
    average = total / len(scores) + 1  # Bug: shouldn't add 1
    return average
```

```matlab
% MATLAB example - calculating average
function average = calculateAverage(scores)
    total = sum(scores);
    average = total / length(scores) + 1;  % Bug: shouldn't add 1
end
```

The code runs, but the average is wrong. This is a logic error.

### What is an IDE?

An **Integrated Development Environment (IDE)** is a program that combines multiple programming tools into a single application. Instead of using separate programs for writing code, running it, and finding bugs, an IDE provides all these tools in one place.

Think of it like the difference between:
- Cooking with individual tools scattered across your kitchen (cutting board in one place, knives in a drawer, stove across the room)
- Cooking at a well-organized workstation where everything you need is within reach

MATLAB is an IDE - it includes an editor for writing code, a command window for running code, tools for viewing variables, and debugging features all in one interface. Google Colab, which you've been using for Python, is also a type of IDE (web-based rather than desktop-based).

## 3. How IDE Debugging Tools Help

You've already learned basic debugging strategies like reading error messages and using print statements. IDEs provide specialized tools that make these strategies more powerful and efficient.

### Reading Error Messages with IDE Support

When an error occurs, the IDE doesn't just print an error message - it actively helps you locate and understand the problem.

```python
# Python example
x = [1, 2, 3]
y = x[5]  
# IndexError: list index out of range
```

```matlab
% MATLAB example
x = [1, 2, 3];
y = x(5);  
% Index exceeds array dimensions
```

The IDE enhances error messages by:
- Highlighting the exact line where the error occurred in your code
- Letting you click the line number to jump directly to the problem
- Showing the call stack (which functions called which) to trace how you got there
- Displaying current variable values so you can see what caused the error

Instead of hunting through your code to find line 47, the IDE takes you there instantly.

### Beyond Print Statements: Inspecting Variables Interactively

Print statements require you to decide in advance what to check:

```python
# Python example
def calculateGrade(scores):
    totalScore = sum(scores)
    print(f"Total score: {totalScore}")  # Must decide to print this
    
    numberOfScores = len(scores)
    print(f"Number of scores: {numberOfScores}")  # And this
    
    result = totalScore / numberOfScores
    print(f"Average: {result}")  # And this
    return result
```

But what if you also need to see what `scores` contains? Or what if you want to try a different calculation? You'd have to add more print statements, run the code again, and wait for results.

The IDE's debugging tools let you pause execution and inspect **everything** interactively:
- See all variable values at once without printing them
- Try calculations on the fly to test hypotheses
- Modify variables to test different scenarios
- All without changing your source code

### Making Isolation Systematic with Breakpoints

When isolating problems, you might comment out code sections to narrow down where the bug is:

```python
# Python example: Commenting out code to find the bug
def processData(data):
    cleaned = cleanData(data)
    # print(f"After cleaning: {cleaned}")
    
    # normalized = normalizeData(cleaned)
    # print(f"After normalizing: {normalized}")
    
    # result = analyzeData(normalized)
    # return result
```

This is tedious: you modify code, run it, see results, then modify again. The IDE's breakpoint feature lets you pause execution at any line without modifying your code. You can:
- Pause before suspicious code to verify inputs are correct
- Check variable values at that exact moment
- Execute the next line and immediately see how variables change
- Continue or step through code line by line

### Stepping Through Code: Watching Execution Flow

When code behaves unexpectedly, you often don't know which part is wrong. Stepping lets you execute one line at a time and watch exactly what happens.

Consider code with complex control flow:

```matlab
function result = processScores(scores)
    if length(scores) > 0
        validScores = scores(scores >= 0);
        if length(validScores) > 0
            result = mean(validScores);
        else
            result = 0;
        end
    else
        result = -1;
    end
end
```

Without stepping, you might not know which branch executed. With stepping:
- Execute one line at a time
- See which `if` conditions evaluate to true
- Watch exactly which branch the code takes
- Verify that each line does what you expect

The IDE shows you a cursor on the current line, so you always know where you are in the execution flow.

## 4. IDE Debugging Features

Modern programming environments provide tools that go beyond simple print statements. These tools let you pause code execution and inspect what's happening in detail.

### Breakpoints
A breakpoint tells the program to pause execution at a specific line. When code reaches a breakpoint:
- Execution stops before running that line
- You can examine all variable values
- You can execute commands to test hypotheses
- You can step through code one line at a time

In MATLAB, you set breakpoints by clicking next to the line number in the editor (a red dot appears).

```matlab
function area = calculateTrapezoidArea(base1, base2, height)
    % Set breakpoint on the next line to inspect inputs
    averageBase = (base1 + base2) / 2;  % <- Breakpoint here
    
    % When paused, you can examine variables
    area = averageBase * height;
end
```

When execution pauses at the breakpoint, you can verify that `base1`, `base2`, and `height` contain the values you expect before the calculation proceeds.

### Stepping Through Code
Once paused at a breakpoint, you can control execution:
- **Step**: Execute the current line and pause at the next line
- **Step In**: If the current line calls a function, jump into that function
- **Step Out**: Finish the current function and pause in the calling code
- **Continue**: Resume normal execution until the next breakpoint

This lets you watch exactly how your code executes, line by line, and see how variable values change.

### Examining Variables While Paused
While paused at a breakpoint, you can:
- View all current variable values
- Execute commands to test calculations
- Modify variable values to test different scenarios
- Verify your assumptions about what the code is doing

```matlab
% Example: While paused in MATLAB debug mode
% You can type variable names to see their values:
>> base1
base1 = 10

>> base2
base2 = 15

% You can even run calculations:
>> testArea = (base1 + base2) / 2 * 5
testArea = 62.5
```

This is far more powerful than print statements because you can interactively explore your code's state without modifying the source code.

### When to Use Breakpoints vs Print Statements

Use print statements when:
- You want to see how values change over many iterations
- The error is intermittent or hard to reproduce
- You need a permanent record of what the code did

Use breakpoints when:
- You need to inspect many variables at once
- You want to test hypotheses interactively
- You need to step through complex logic carefully
- The bug occurs at a specific point you've identified

## 5. Understanding Code Performance

Not all code that works correctly performs well. Some programs take seconds while equivalent programs take minutes or hours. This matters when:
- Processing large datasets
- Running simulations many times
- Creating interactive applications
- Meeting project deadlines with limited computing resources

### What is Profiling?
Profiling means measuring how long each part of your code takes to execute. This helps you find **bottlenecks**: the specific lines or functions that consume most of the running time.

Think of profiling like finding traffic jams on a commute. If your drive takes an hour, profiling might reveal that 50 minutes are spent on one particular highway segment. Fix that bottleneck by taking a different route, and your commute might drop to 15 minutes. Similarly, if your program takes 10 minutes to run, profiling might show that 9 minutes are spent in a single function. Optimize that function, and your program might run in 1 minute.

### Why Profile?
Before optimizing code, you need to know where the problems are. Programmers are often wrong about which parts are slow. Profiling gives you data instead of guesses.

**The Golden Rule of Optimization**: Measure first, optimize second.

Many beginning programmers try to optimize everything, making code complex and hard to read. Profiling tells you to optimize only the 10% of code that takes 90% of the time.

### Measuring Execution Time
Both Python and MATLAB provide simple ways to measure how long code takes:

```python
# Python example using time module
import time

startTime = time.time()
# Code to measure
total = 0
for i in range(1000000):
    total = total + i
endTime = time.time()

print(f"Loop took {endTime - startTime} seconds")
```

```matlab
% MATLAB example using tic/toc
tic  % Start timer
total = 0;
for i = 1:1000000
    total = total + i;
end
elapsedTime = toc;  % Stop timer

disp(['Loop took ', num2str(elapsedTime), ' seconds'])
```

### Comparing Approaches
Often, you want to compare different implementations of the same logic:

```python
# Python example: Which is faster?
import time

# Approach 1: Using a loop
startTime = time.time()
total = 0
for i in range(1000000):
    total = total + i
time1 = time.time() - startTime

# Approach 2: Using built-in sum
startTime = time.time()
total = sum(range(1000000))
time2 = time.time() - startTime

print(f"Loop: {time1:.4f} seconds")
print(f"Sum: {time2:.4f} seconds")
print(f"Speedup: {time1/time2:.1f}x faster")
```

```matlab
% MATLAB example: Which is faster?

% Approach 1: Using a loop
tic
total = 0;
for i = 1:1000000
    total = total + i;
end
time1 = toc;

% Approach 2: Using built-in sum
tic
total = sum(1:1000000);
time2 = toc;

disp(['Loop: ', num2str(time1), ' seconds'])
disp(['Sum: ', num2str(time2), ' seconds'])
disp(['Speedup: ', num2str(time1/time2), 'x faster'])
```

In both languages, the built-in `sum` function is typically much faster than writing your own loop.

## 6. The Profiler: Finding Bottlenecks Automatically

Manually timing different sections of code works for small programs, but becomes tedious for large projects with many functions. Profilers automatically measure time spent in each function and on each line.

### What Profilers Provide
A profiler runs your code and tracks:
- Total time for each function
- Number of times each function was called
- Time spent on each individual line within functions
- Which functions call which other functions

MATLAB's profiler provides a visual report that uses color coding:
- Red/dark sections: Most time spent here (investigate these first)
- Yellow/medium sections: Moderate time spent
- Green/light sections: Little time spent (probably fine)

To use MATLAB's profiler:
```matlab
profile on          % Start profiling
myFunction();       % Run your code
profile viewer      % Open visual report
```

The profiler report shows you exactly where your program spends its time, letting you focus optimization efforts on the code that matters most.

## 7. Using MATLAB's Debugger and Profiler

Now that you understand the concepts, here are the specific steps for using MATLAB's debugging and profiling tools.

### Setting and Using Breakpoints in MATLAB

**To set a breakpoint:**
1. Open your `.m` file in the MATLAB Editor
2. Find the line where you want to pause execution
3. Click the small dash (`-`) to the left of the line number
4. A red circle appears, indicating a breakpoint is set

**Alternative method using commands:**
```matlab
% Set breakpoint at line 10 in myFunction.m
dbstop in myFunction at 10

% Set breakpoint when an error occurs
dbstop if error

% List all breakpoints
dbstatus

% Clear a specific breakpoint
dbclear in myFunction at 10

% Clear all breakpoints
dbclear all
```

**When your code hits a breakpoint:**
- Execution pauses before executing that line
- The command prompt changes to `K>>` (debug mode)
- A green arrow shows the current line
- The Editor highlights the current line

**Controlling execution while paused:**
- **Continue** (or `dbcont`): Resume normal execution until next breakpoint
- **Step** (or `dbstep`): Execute current line and pause at next line
- **Step In** (or `dbstep in`): If current line calls a function, enter that function
- **Step Out** (or `dbstep out`): Finish current function and return to caller
- **Quit Debug Mode** (or `dbquit`): Stop debugging and return to normal mode

**Examining variables in debug mode:**
```matlab
% While paused at K>> prompt, type variable names
K>> x
x = 5

K>> length(myArray)
ans = 10

K>> mean(scores)
ans = 87.5

% You can also execute any MATLAB command
K>> plot(data)
K>> sum(values) / length(values)
```

### Using MATLAB's Profiler

**To profile your code:**

**Method 1: Using the GUI**
1. Go to the MATLAB Home tab
2. Click "Run and Time" button
3. Select your script or function
4. MATLAB runs your code and opens the Profiler report

**Method 2: Using commands**
```matlab
% Start profiling
profile on

% Run your code
myFunction(inputData);

% Stop profiling and view results
profile viewer

% Or get profile data programmatically
profileData = profile('info');
```

**Reading the Profiler Report:**

The profiler shows a list of all functions that ran, sorted by time spent. For each function, you see:
- **Total Time**: How long was spent in this function
- **Self Time**: Time spent in this function excluding functions it called
- **Calls**: How many times this function was called

Click on a function name to see line-by-line timing:
- Lines are color-coded (red = slow, yellow = medium, green = fast)
- Each line shows how many times it executed
- Each line shows total time spent on that line

**What to look for:**
1. Functions with high total time (these are bottlenecks)
2. Functions called many times unnecessarily
3. Individual lines that take surprisingly long
4. Loops that could be vectorized

**Example profiling workflow:**
```matlab
% Profile your code
profile on
resultsData = processLargeDataset(data);
profile viewer

% Look at the report, identify slow function
% Suppose findMatches() takes 90% of time

% Optimize findMatches(), then profile again
profile on
resultsData = processLargeDataset(data);
profile viewer

% Compare times to see improvement
```

**Common profiler commands:**
```matlab
% Clear previous profiling data
profile clear

% Turn off profiler
profile off

% Get detailed profiling statistics
stats = profile('info');

% View specific function details
profsave(stats, 'profileResults')  % Save to HTML file
```

### Debugging and Profiling Together

**Typical workflow:**
1. Write code and test basic functionality
2. If bugs appear, set breakpoints to investigate
3. Use stepping to understand execution flow
4. Fix bugs and verify correctness
5. If code is too slow, run profiler
6. Identify bottleneck functions from profiler report
7. Set breakpoints in slow functions to understand why they're slow
8. Optimize the slow sections
9. Profile again to verify improvement

**Example debugging session:**
```matlab
% You have a bug in calculateStatistics.m
% Set a breakpoint at the start of the function
dbstop in calculateStatistics at 1

% Run your code
results = calculateStatistics(data);

% Code pauses at breakpoint
% Check input
K>> size(data)
K>> min(data)
K>> max(data)

% Step through line by line
K>> dbstep
K>> dbstep

% Notice variable has unexpected value
K>> filteredData
K>> length(filteredData)  % Should be 100 but is 0!

% Found the bug - now fix it
K>> dbquit
```

## 8. Common Performance Issues
Based on profiling data, you might find:

**Growing arrays in loops** (slow):
```matlab
% Slow: array grows each iteration
result = [];
for i = 1:10000
    result = [result, i^2];
end
```

**Pre-allocating arrays** (fast):
```matlab
% Fast: array size set once
result = zeros(1, 10000);
for i = 1:10000
    result(i) = i^2;
end
```

**Using vectorization instead of loops** (fastest):
```matlab
% Fastest: no loop at all
i = 1:10000;
result = i.^2;
```

Profiling helps you identify these patterns in your own code.

## 9. Connecting Debugging and Profiling

These tools work together:
- **Debugging** helps you find what's wrong (correctness)
- **Profiling** helps you find what's slow (performance)

Workflow:
1. Write code that works correctly (use debugging if needed)
2. If code runs too slowly, use profiler to find bottlenecks
3. Optimize only the slow parts
4. Debug again to ensure optimizations didn't break anything
5. Profile again to measure improvement

Never optimize code before it works correctly. Never assume you know what's slow without measuring. The profiler gives you facts, not guesses.