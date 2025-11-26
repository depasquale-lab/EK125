# Pre-Reading: User Functions and Modular Programming in MATLAB

## Introduction

You've already learned about functions and modular programming in Python, and you'll find that MATLAB follows many of the same principles. However, MATLAB has some unique syntax and conventions that we'll explore in this reading. By the end, you'll understand how to write well-organized, reusable MATLAB code using user-defined functions.

## Why Functions Matter

Just as in Python, functions help us:
- **Break down complex problems** into manageable pieces
- **Reuse code** without copying and pasting
- **Test and debug** smaller components independently
- **Collaborate** more effectively by dividing work

## MATLAB Function Syntax

### Basic Structure

In Python, you defined functions within a script or module. In MATLAB, each function typically lives in **its own file** with the same name as the function. Here's the basic syntax:

```matlab
function [output1, output2] = functionName(input1, input2)
% FUNCTIONNAME Brief description of what the function does
%   Detailed explanation of the function's purpose, inputs, and outputs.
%
%   Inputs:
%       input1 - description of first input
%       input2 - description of second input
%
%   Outputs:
%       output1 - description of first output
%       output2 - description of second output

    % Function body goes here
    output1 = ...;
    output2 = ...;
end
```

**Key differences from Python:**
- The `function` keyword starts the definition
- Outputs are listed in square brackets on the left side of the equals sign
- The function name and filename must match (e.g., `calculateArea.m` contains `function calculateArea`)
- The `end` keyword closes the function

**All MATLAB functions consist of:**
1. The function header (first line) with:
   - The reserved word `function`
   - If the function returns values: the output argument name(s) followed by `=`
   - The function name (should match the filename)
   - Input arguments in parentheses (separated by commas if more than one)
2. Comments describing what the function does (displayed by the `help` command)
3. The body with all statements, including assigning values to all output arguments
4. The `end` keyword

### Viewing and Getting Help

You can view a function's code using the `type` command:
```matlab
type calculateCircleArea
```

You can view a function's help comments using the `help` command:
```matlab
help calculateCircleArea
```
This displays the comment block immediately after the function header.

### Single Output Functions

When your function returns only one value, you can omit the square brackets:

```matlab
function area = calculateCircleArea(radius)
% CALCULATECIRCLEAREA Calculates the area of a circle
%   area = calculateCircleArea(radius) returns the area of a circle
%   given its radius.

    area = pi * radius^2;
end
```

Save this as `calculateCircleArea.m`.

**Important:** The output argument must be assigned a value somewhere in the function body. This is how the value is returned from the function.

### No Output Functions

Some functions perform actions without returning values (like printing or plotting):

```matlab
function displayGreeting(name)
% DISPLAYGREETING Displays a personalized greeting
%   displayGreeting(name) prints a greeting message to the console.

    fprintf('Hello, %s! Welcome to MATLAB.\n', name);
end
```

**Key points about functions with no outputs:**
- There are no output arguments in the function header
- There is no assignment operator (=) in the header
- The function call is a statement by itself—it **cannot** be embedded in an assignment statement or used in expressions

**Correct usage:**
```matlab
displayGreeting('Alice');  % This works
```

**Incorrect usage:**
```matlab
x = displayGreeting('Alice');  % ERROR: Too many output arguments
```

### Multiple Outputs

MATLAB makes it easy to return multiple values (remember unpacking in Python?):

```matlab
function [mean_val, std_val, max_val] = analyzeData(data)
% ANALYZEDATA Computes statistics for a dataset
%   [mean_val, std_val, max_val] = analyzeData(data) returns the mean,
%   standard deviation, and maximum value of the input data.

    mean_val = mean(data);
    std_val = std(data);
    max_val = max(data);
end
```

**Calling functions with multiple outputs:**

When calling this function, you should capture the returned values in separate variables:
```matlab
[m, s, mx] = analyzeData([1, 2, 3, 4, 5]);
```

Or if you only need some outputs:
```matlab
[m, s] = analyzeData([1, 2, 3, 4, 5]);  % Only returns first two outputs
```

**Important:** If you don't capture all the outputs, only the first value is retained:
```matlab
result = analyzeData([1, 2, 3, 4, 5]);  % Only the mean is stored in result
```

## Variable Scope in MATLAB

### Local Variables

Just like in Python, variables created inside a function are **local** to that function. They don't exist outside the function and don't interfere with variables of the same name elsewhere:

```matlab
function result = addNumbers(a, b)
    sum_val = a + b;  % sum_val is local to this function
    result = sum_val;
end
```

If you have a variable called `sum_val` in your main script, it won't be affected by the `sum_val` inside the function.

### Pass by Value

MATLAB uses **pass by value** for function arguments. This means the function receives a *copy* of the data, not the original:

```matlab
function modifyVector(vec)
    vec(1) = 999;  % Changes the local copy only
    disp(vec);
end

% In your script:
myData = [1, 2, 3, 4, 5];
modifyVector(myData);  % Shows [999, 2, 3, 4, 5]
disp(myData);          % Still shows [1, 2, 3, 4, 5]
```

To get modified data out of a function, you must **return it as an output**:

```matlab
function vec = modifyVector(vec)
    vec(1) = 999;
end

myData = [1, 2, 3, 4, 5];
myData = modifyVector(myData);  % Now myData is updated
```

### Global Variables (Avoid When Possible)

MATLAB does support global variables, but they should be avoided in favor of passing data through function arguments:

```matlab
global CONSTANT_VALUE;  % Declared in both the function and calling script
```

**Best practice:** Use function inputs and outputs instead of global variables.

## Organizing Code with Functions

### One Function Per File (Primary Functions)

In MATLAB, each `.m` file should contain one main function with the same name as the file. This is called a **primary function** and is the only function accessible from outside the file.

**File: `processTemperature.m`**
```matlab
function [tempC, tempK] = processTemperature(tempF)
% PROCESSTEMPERATURE Converts Fahrenheit to Celsius and Kelvin
    tempC = (tempF - 32) * 5/9;
    tempK = tempC + 273.15;
end
```

### Local Functions (Helper Functions)

You can define additional **local functions** in the same file below the primary function. These are only accessible within that file:

**File: `analyzeStudentGrades.m`**
```matlab
function report = analyzeStudentGrades(grades)
% ANALYZESTUDENTGRADES Generates a comprehensive grade report
    
    avg = calculateAverage(grades);
    letter = assignLetterGrade(avg);
    
    report = sprintf('Average: %.2f, Grade: %s', avg, letter);
end

function avg = calculateAverage(scores)
% Local function - only accessible within this file
    avg = mean(scores);
end

function letter = assignLetterGrade(numericGrade)
% Local function - only accessible within this file
    if numericGrade >= 90
        letter = 'A';
    elseif numericGrade >= 80
        letter = 'B';
    elseif numericGrade >= 70
        letter = 'C';
    elseif numericGrade >= 60
        letter = 'D';
    else
        letter = 'F';
    end
end
```

This is similar to defining helper functions within a Python module.

## Best Practices for Modular Design

### 1. Single Responsibility Principle

Each function should do **one thing well**. If you find yourself using "and" to describe what a function does, it might need to be split up.

**Bad:**
```matlab
function processAndPlotData(data)
    % Cleans data AND plots it AND saves the plot
    % Too many responsibilities!
end
```

**Good:**
```matlab
function cleanData = cleanData(rawData)
    % Only handles data cleaning
end

function plotData(data)
    % Only handles plotting
end
```

### 2. Meaningful Names

Function names should be descriptive verbs or verb phrases:
- `calculateVolume` instead of `vol`
- `convertToRadians` instead of `conv`
- `validateInput` instead of `check`

### 3. Clear Documentation

Always include a header comment explaining:
- What the function does
- What inputs it expects
- What outputs it returns
- Any assumptions or limitations

MATLAB's `help` function displays these comments:
```matlab
help calculateCircleArea
```

### 4. Input Validation

Check that inputs meet your expectations:

```matlab
function area = calculateRectangleArea(length, width)
% CALCULATERECTANGLEAREA Computes the area of a rectangle
    
    % Input validation
    if length <= 0 || width <= 0
        error('Length and width must be positive values');
    end
    
    area = length * width;
end
```

### 5. Consistent Formatting

- Use meaningful variable names
- Indent code blocks consistently (4 spaces is standard)
- Add blank lines to separate logical sections
- Comment complex operations

### 6. Testing Functions Independently

Write functions so they can be tested on their own:

```matlab
% Test script for calculateCircleArea
testRadius = 5;
expectedArea = pi * 25;
actualArea = calculateCircleArea(testRadius);

if abs(expectedArea - actualArea) < 1e-10
    fprintf('Test passed!\n');
else
    fprintf('Test failed!\n');
end
```

## Example: Building a Modular Program

Let's say you need to analyze temperature data from a file. Here's how you might break it down:

**File: `analyzeTemperatures.m`** (Primary function)
```matlab
function summary = analyzeTemperatures(filename)
% ANALYZETEMPERATURES Reads and analyzes temperature data from a file
    
    temps = readTemperatureFile(filename);
    temps_cleaned = removeOutliers(temps);
    stats = calculateStatistics(temps_cleaned);
    summary = formatSummary(stats);
end

function temps = readTemperatureFile(filename)
    temps = load(filename);
end

function cleaned = removeOutliers(data)
    meanVal = mean(data);
    stdVal = std(data);
    cleaned = data(abs(data - meanVal) < 3 * stdVal);
end

function stats = calculateStatistics(data)
    stats.mean = mean(data);
    stats.median = median(data);
    stats.std = std(data);
end

function summary = formatSummary(stats)
    summary = sprintf('Mean: %.2f, Median: %.2f, Std: %.2f', ...
                      stats.mean, stats.median, stats.std);
end
```

Notice how each function has a clear, single purpose and how they work together to accomplish the larger task.

## Common Pitfalls to Avoid

1. **Forgetting to return values:** Make sure output variables are assigned before the function ends
2. **Filename mismatch:** The filename must match the primary function name exactly
3. **Modifying inputs and expecting changes:** Remember pass by value—return modified data
4. **Overusing global variables:** Pass data through function arguments instead
5. **Functions that are too long:** If a function is more than 50 lines, consider breaking it up

## Summary

MATLAB functions follow similar principles to Python functions but with different syntax:
- Each primary function lives in its own file
- Outputs are declared in square brackets before the function name
- Variables are local by default and data is passed by value
- Local functions can be used as helpers within a file
- Good modular design makes code reusable, testable, and maintainable

In class, we'll practice writing functions and building modular programs to solve real engineering problems. Come prepared with questions!

## Quick Reference

```matlab
% Single output
function output = myFunction(input)
    output = input * 2;
end

% Multiple outputs
function [out1, out2] = myFunction(in1, in2)
    out1 = in1 + in2;
    out2 = in1 - in2;
end

% No output
function myFunction(input)
    disp(input);
end

% With local helper function
function result = mainFunction(data)
    processed = helperFunction(data);
    result = processed * 2;
end

function out = helperFunction(in)
    out = in + 1;
end
```