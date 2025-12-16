# Week 8 Class 15 Pre-read: Scripts, Comments, and Code Documentation

## Learning Objectives
By the end of this reading, you should understand:
- How code and comments work together to create readable programs
- When and how to write effective comments
- How to organize code into reusable scripts
- Documentation standards for professional Python code
- The difference between interactive coding and production scripts

## 1. Code and Comments: Both Are Necessary

### The Myth of "Self-Documenting" Code
You may hear people talk about "self-documenting code" - the idea that if you write your code clearly enough, you don't need comments. **This is wrong and harmful.**

Here's why:
1. **Most readers aren't Python experts** - Your code might be read by researchers, collaborators, managers, or future maintainers who don't know Python well
2. **Code explains HOW, not WHY** - Even perfect code can't explain business logic, design decisions, or context
3. **Assumptions and intent become invisible** - What's obvious to you today won't be obvious to others (or to you in 6 months)

**The truth:** Good code and good comments are complementary. You need both.

### What Code Can Tell You
```python
price = basePrice * quantity * 1.0825
```

From the code alone, a reader can see:
- We're multiplying two variables and one constant
- The result is stored in `price`
- The constant is 1.0825

### What Code Cannot Tell You
```python
# NYC sales tax is 8.25% (as of 2025)
# Client requires tax included in displayed price
price = basePrice * quantity * 1.0825
```

Now the reader knows:
- Why 1.0825 (not a magic number anymore)
- Where this applies (NYC)
- When this might change (tax rates change)
- Why we're including it (client requirement)

**Without the comment, even an experienced Python programmer wouldn't know these critical facts.**

### Clear Names Help, But Aren't Enough

**Poor naming:**
```python
def calc(x, y, z):
    return x * y * (1 + z)
```
Problems: Cryptic names, no explanation of purpose or units

**Better naming:**
```python
def calculateTotalPrice(basePrice, quantity, taxRate):
    return basePrice * quantity * (1 + taxRate)
```
Better: Clear what it does

**Best: Clear naming WITH comments:**
```python
def calculateTotalPrice(basePrice, quantity, taxRate):
    """
    Calculate final price including tax.
    
    Args:
        basePrice: Price per item in USD
        quantity: Number of items (must be positive)
        taxRate: Tax as decimal (e.g., 0.0825 for 8.25%)
    
    Returns:
        Total price in USD including tax
    """
    return basePrice * quantity * (1 + taxRate)
```
Now readers know: units, constraints, format expectations, and what the return value represents.

#### Example where clear names CANNOT tell the full story:

Even with perfect variable names, critical information can be ambiguous:

python

```python
import math

# BAD - Ambiguous even with clear names
def calculateSineWave(amplitude, frequency, time):
    return amplitude * math.sin(frequency * time)
```

Questions a reader has:

* Is `frequency` in Hz or radians per second?
* Is `time` in seconds or milliseconds?
* What units is the result in?

python

```python
import math

# GOOD - Names AND comments provide complete picture
def calculateSineWave(amplitude, frequency, time):
    """
    Calculate displacement of sine wave at given time.
    
    Args:
        amplitude: Maximum displacement in meters
        frequency: Wave frequency in Hz (cycles per second)
        time: Time point in seconds
    
    Returns:
        Displacement in meters at the specified time
    """
    # Convert frequency (Hz) to angular frequency (rad/s)
    # Angular frequency ω = 2πf
    angularFreq = 2 * math.pi * frequency
    
    # Sine wave equation: y(t) = A × sin(ωt)
    # where A = amplitude, ω = angular frequency, t = time
    return amplitude * math.sin(angularFreq * time)
```

**Another critical example - angle units:**

python

```python
import math

# DANGEROUS - No indication of expected units
def calculateHorizontalSpeed(airplaneVelocity, glideAngle):
    return airplaneVelocity * math.cos(glideAngle)

# Using it:
result = calculateHorizontalSpeed(50, 45)  # Is this 45 degrees or 45 radians?!
```

The problem: `math.cos()` expects radians, but people naturally think in degrees. Without comments, users will make mistakes.

```python
import math

# SAFE - Documentation makes units explicit
def calculateHorizontalDistance(airplaneVelocity_mps, glideAngle_deg):
    """
    Calculate horizontal component of velocity vector.
    
    Args:
        velocity: Speed in m/s
        angleDegrees: Angle from horizontal in degrees (NOT radians)
    
    Returns:
        Horizontal velocity component in m/s
    """
    # Convert degrees to radians for trig functions
    # Python's math library requires radians
    glideAngle_rad = math.radians(glideAngle_deg)
    
    # Horizontal component: v_x = v × cos(θ)
    return airplaneVelocity_mps * math.cos(glideAngle_rad)
```

Now it's impossible to use incorrectly - the parameter name AND documentation make it clear.

### Example: Why Both Are Needed

Consider this code from a data analysis project:

```python
# Without comments (what some call "self-documenting")
def processStudentData(students):
    filtered = [s for s in students if s['credits'] >= 12]
    return filtered
```

Questions a reader might have:
- Why 12 credits?
- What happens to students with fewer credits?
- Is this temporary or permanent filtering?
- Should this threshold change for different terms?

And most importantly, for a non-python programmer who isn't familiar with python's peculiar comprehension syntax:
- What is that code line even doing???
  - Be honest, did you, an EK125 student, feel good when you tried reading that line? How do you think other people would feel coming across it? Doesn't it look like magic, and on its own give you no idea whatsoever about what it's doing? Thus the myth of "self-documenting code".

```python
# With comments (actually readable by non-experts)
def processStudentData(students):
    """
    Filter student list to only full-time students.
    
    University policy defines full-time as 12+ credits per semester.
    Part-time students are excluded from this analysis per IRB protocol #2024-158.
    
    Args:
        students: List of dicts with 'credits' key
        
    Returns:
        List containing only full-time students (12+ credits)
    """
    # Full-time threshold per university registrar definition
    FULL_TIME_CREDITS = 12
    
    # Filter to full-time only for IRB compliance
    filtered = [s for s in students if s['credits'] >= FULL_TIME_CREDITS]
    return filtered
```

Now anyone - including non-programmers on your research team - can understand what's happening and why.

## 2. Writing Effective Comments

### Extract Magic Numbers to Named Constants
```python
# POOR: Magic numbers with no explanation
if age >= 65:
    discount = price * 0.15
elif age >= 18:
    discount = 0

# BETTER: Named constants
SENIOR_AGE = 65
ADULT_AGE = 18
SENIOR_DISCOUNT_RATE = 0.15

if age >= SENIOR_AGE:
    discount = price * SENIOR_DISCOUNT_RATE
elif age >= ADULT_AGE:
    discount = 0

# BEST: Named constants WITH comments explaining business rules
# Senior discount policy (approved by management 2025-01-15)
# Applies to customers 65+ years old
SENIOR_AGE = 65
ADULT_AGE = 18
SENIOR_DISCOUNT_RATE = 0.15  # 15% discount for seniors

if age >= SENIOR_AGE:
    discount = price * SENIOR_DISCOUNT_RATE
elif age >= ADULT_AGE:
    discount = 0
```

**Note on naming:** Constants are an exception to camelCase - use UPPER_SNAKE_CASE for module-level constants to follow industry convention.

### Break Complex Logic into Well-Named Functions
```python
# POOR: Everything in one monolithic function
def processApplication(application):
    # 50 lines of validation logic...
    # 30 lines of scoring logic...
    # 40 lines of decision logic...
    # Even with good names, this is overwhelming
    
# BETTER: Logical separation with descriptive names
def processApplication(application):
    """
    Process college application through full pipeline.
    
    Pipeline steps:
    1. Validate completeness (all required fields present)
    2. Calculate admission score (GPA + test scores + essays)
    3. Make admission decision (compare to threshold)
    
    Args:
        application: Dict with student application data
        
    Returns:
        Decision string: "accepted", "rejected", or "waitlist"
    """
    if not isValidApplication(application):
        return "rejected: incomplete"
    
    # Score calculation uses rubric from admissions committee
    score = calculateApplicationScore(application)
    
    # Thresholds set by admissions committee for 2025 cycle
    decision = makeAdmissionDecision(score)
    
    return decision

def isValidApplication(application):
    """Check that all required fields are present and valid."""
    # Validation logic here

def calculateApplicationScore(application):
    """
    Calculate admission score using 2025 rubric.
    
    Scoring: 40% GPA, 30% test scores, 30% essays
    Maximum score: 100 points
    """
    # Scoring logic here
```

Notice: Even with good function names, we still need comments to explain the scoring weights, thresholds, and business context.

## 3. What to Comment

### Always Comment: Mathematical Formulas and Calculations

**Critical rule:** Every mathematical operation needs a comment explaining the formula and its source. The level of detail should match how esoteric the formula is.

Why? Because:
1. Readers need to verify the implementation is correct
2. Formula sources let others check your work
3. Units and conventions must be explicit
4. Small errors in math cause huge problems

**Common formulas need brief documentation:**
```python
# BAD - No explanation of formula
def celsiusToFahrenheit(celsius):
    return celsius * 9/5 + 32

# GOOD - Brief comment for well-known formula
def celsiusToFahrenheit(celsius):
    """Convert Celsius to Fahrenheit."""
    # Formula: F = C × (9/5) + 32
    return celsius * 9/5 + 32

# ALSO GOOD - Inline comment if function is obvious
def celsiusToFahrenheit(celsius):
    """Convert Celsius to Fahrenheit."""
    return celsius * 9/5 + 32  # F = C × (9/5) + 32
```

**Uncommon formulas need detailed documentation:**
```python
# BAD - No explanation of complex formula
def calculateBMI(weight, height):
    return weight / (height ** 2)

# GOOD - More detail for formula that readers may need to verify
def calculateBMI(weight, height):
    """
    Calculate Body Mass Index.
    
    Args:
        weight: Weight in kilograms
        height: Height in meters
    
    Returns:
        BMI value (kg/m²)
    """
    # BMI = weight(kg) / height(m)²
    # Source: WHO Technical Report Series 894, 2000
    return weight / (height ** 2)
```

**Specialized or derived formulas need full documentation:**

```python
def calculateCompoundInterest(principal, rate, years):
    """
    Calculate compound interest (compounded annually).
    
    Args:
        principal: Initial amount in dollars
        rate: Annual interest rate as decimal (e.g., 0.05 for 5%)
        years: Number of years
        
    Returns:
        Final amount after compound interest
    """
    # Compound interest formula: A = P(1 + r)^t
    # Where: A = final amount, P = principal, r = rate, t = time
    # IMPORTANT: Assumes annual compounding (not monthly or continuous)
    return principal * (1 + rate) ** years

def calculateStandardDeviation(values):
    """
    Calculate population standard deviation.
    
    Args:
        values: List of numeric values
        
    Returns:
        Population standard deviation
    """
    n = len(values)
    mean = sum(values) / n
    
    # Population variance: σ² = Σ(x - μ)² / N
    # Standard deviation: σ = √(variance)
    # NOTE: This is POPULATION std dev (divide by N)
    # For SAMPLE std dev, would use N-1 (Bessel's correction)
    # Source: Introduction to Statistics, OpenStax, Section 2.7
    variance = sum((x - mean) ** 2 for x in values) / n
    return variance ** 0.5

def calculateTrajectory(velocity, angle, time):
    """
    Calculate projectile position (ignoring air resistance).
    
    Args:
        velocity: Initial velocity in m/s
        angle: Launch angle in degrees
        time: Time elapsed in seconds
        
    Returns:
        Tuple of (horizontal_position, vertical_position) in meters
    """
    import math
    
    # Convert angle to radians for trig functions
    angleRad = math.radians(angle)
    
    # Horizontal position: x = v₀ × cos(θ) × t
    # No horizontal acceleration (ignoring air resistance)
    x = velocity * math.cos(angleRad) * time
    
    # Vertical position: y = v₀ × sin(θ) × t - (1/2) × g × t²
    # g = 9.81 m/s² (standard gravity at Earth's surface)
    # Negative term because gravity opposes upward motion
    # Source: Physics for Scientists and Engineers, Serway & Jewett, Ch. 4
    g = 9.81  # m/s²
    y = velocity * math.sin(angleRad) * time - 0.5 * g * time ** 2
    
    return (x, y)
```

**Guidelines for math documentation level:**

| Formula Type | Documentation Level | Example |
|--------------|---------------------|---------|
| Well-known (taught in high school) | Formula only | Area of circle, Pythagorean theorem, C↔F conversion |
| Standard (common in field) | Formula + units | BMI, simple interest, speed = distance/time |
| Specialized (field-specific) | Formula + source + assumptions | Compound interest, projectile motion, statistical tests |
| Custom/derived (not in textbooks) | Full derivation + source data | Your own models, modified formulas, empirical fits |

**Even simple arithmetic needs context:**
```python
# BAD - Magic numbers and operations
totalCost = baseCost * 1.2 * 0.85

# GOOD - Each operation explained
# Apply 20% markup for overhead (company policy)
markedUpCost = baseCost * 1.2

# Apply 15% educational discount (contract #EDU-2025-047)
totalCost = markedUpCost * 0.85
```

### Always Comment: Business Logic and Context
```python
# GOOD - Explains business rules that aren't in the code
price = basePrice * 1.0825  # NYC sales tax is 8.25%

# GOOD - Explains non-obvious behavior  
scores.pop()  # Remove last score (it's a practice run, not real data)

# GOOD - Explains why this algorithm
# Using binary search because student list is pre-sorted by ID
# O(log n) vs O(n) for linear search - matters with 50,000+ students
index = binarySearch(sortedStudents, targetId)

# GOOD - Documents workarounds and gotchas
# datetime.strptime fails with dates before 1900
# Bug report: https://bugs.python.org/issue13305
# Using manual parsing as workaround
year = int(dateString[:4])
month = int(dateString[5:7])
```

### Always Comment: Assumptions and Constraints
```python
def processTemperature(celsius):
    """
    Convert Celsius to Fahrenheit.
    
    ASSUMES: Input is a valid temperature (not None or string)
    ASSUMES: Temperature is physically reasonable (-273.15°C to 1000°C)
    NOTE: Does not validate input - caller must ensure valid data
    """
    # Formula: F = C × (9/5) + 32
    return celsius * 9/5 + 32

def analyzeGrades(grades):
    """
    Calculate grade statistics.
    
    REQUIRES: grades list is non-empty
    REQUIRES: all grades are numeric (int or float)
    REQUIRES: all grades are in range 0-100
    
    Will crash with division by zero if empty list passed.
    """
    return {
        'mean': sum(grades) / len(grades),
        'min': min(grades),
        'max': max(grades)
    }
```

### Goes Without Saying: Comments must match the code
```python
# BAD - Comment doesn't match code (dangerous!)
# Calculate the average
median = sorted(scores)[len(scores) // 2]

# GOOD - Keep comments synchronized with code
# Calculate the median (middle value of sorted list)
median = sorted(scores)[len(scores) // 2]
```

**Critical rule:** If you change code, update the comments. Incorrect comments are worse than no comments because they mislead readers.

### Comment Patterns for Real Code

#### Pattern 1: Section Headers
```python
# ============================================
# DATA LOADING AND VALIDATION
# ============================================

def loadCSV(filename):
    """
    Load CSV file and validate required columns.
    
    Required columns: studentId, name, grade
    """
    # ... implementation ...

def validateData(data):
    """
    Check data integrity.
    
    Verifies: no missing values, grades in 0-100 range, unique IDs
    """
    # ... implementation ...

# ============================================
# STATISTICAL ANALYSIS
# ============================================

def analyzePattern(data):
    """Find grade distribution patterns and outliers."""
    # ... implementation ...
```

#### Pattern 2: Complex Logic Explanation
```python
def calculateGrade(rawScore, curveMethod="standard"):
    """
    Apply grading curve to raw score.
    
    Args:
        rawScore: Raw points (0-100)
        curveMethod: "standard" or "linear"
    
    Returns:
        Curved grade (0-100)
    """
    # Standard curve: sqrt(rawScore) * 10
    # Rationale: Provides more benefit to lower scores
    # Example: 64 -> 80, 81 -> 90, 100 -> 100
    # Approved by department chair 2024-09-01
    if curveMethod == "standard":
        return min(100, int(rawScore ** 0.5 * 10))
    
    # Linear curve: flat 10 point bonus
    # Used for courses where raw scores are already reasonable
    elif curveMethod == "linear":
        return min(100, rawScore + 10)
```

#### Pattern 3: TODO, FIXME, and NOTE
These special comment types help track issues:

```python
def processPayment(amount, cardNumber):
    """Process credit card payment."""
    
    # TODO: Add support for international currencies (EUR, GBP)
    # TODO: Implement retry logic for network failures
    # Ticket: PROJ-458
    
    # FIXME: Current regex doesn't handle Diners Club cards
    # Failing test: test_diners_club_validation()
    if not validateCard(cardNumber):
        return False
    
    # NOTE: Rate limiting enforced at 10 transactions/second
    # Exceeding limit returns HTTP 429 error
    # See: api_documentation.md section 3.2
    return chargeCard(amount, cardNumber)
```

## 4. Docstrings: Formal Documentation

Docstrings are special comments that document functions, classes, and modules. They use triple quotes and appear right after the definition.

### Function Docstrings (Google Style)
```python
def mergeDatasets(primary, secondary, keyField="id", keepDuplicates=False):
    """
    Merge two datasets based on a common key field.
    
    Args:
        primary: Primary dataset (dict or list of dicts)
        secondary: Secondary dataset to merge
        keyField: Field name to match records on (default: "id")
        keepDuplicates: If True, keep duplicate entries (default: False)
    
    Returns:
        Merged dataset with combined fields from both inputs
        
    Raises:
        KeyError: If keyField doesn't exist in datasets
        ValueError: If datasets are incompatible types
    
    Example:
        >>> users = [{"id": 1, "name": "Alice"}]
        >>> scores = [{"id": 1, "score": 95}]
        >>> mergeDatasets(users, scores)
        [{"id": 1, "name": "Alice", "score": 95}]
    """
    # Implementation here
```

### Simpler Function Docstrings
For straightforward functions, a one-line docstring is sufficient:

```python
def calculateAverage(numbers):
    """Return the arithmetic mean of the numbers list."""
    return sum(numbers) / len(numbers)

def isValidEmail(email):
    """Check if email contains @ and has text before and after it."""
    return email.count("@") == 1 and email.index("@") > 0
```

### Module-Level Docstrings
```python
"""
studentPerformance.py

Analyzes student performance across multiple assessments.
Generates statistical reports and identifies at-risk students.

Classes:
    Student: Represents individual student with grades
    Course: Manages collection of students and assessments

Functions:
    loadRoster: Import student list from CSV
    generateReport: Create performance summary
    findAtRisk: Identify students below threshold

Usage:
    python studentPerformance.py roster.csv grades.csv
    
Requirements:
    - Python 3.8+
    - numpy, pandas
"""
```

## 5. From Interactive to Scripts

In this advanced section of the class, we will leave you with some high-level understandings of how python would be used in a professional domain. Don't worry if this doesn't sink in the first time, it's important to us that you have some familiarity with it, even if mastery will take more time than we have in EK125.

### Why Scripts Matter
You might not have realized it, but so far when you've been using Google Colab, you've been using a Jupyter front end. Jupyter is great for exploration and learning, but real software is built with **scripts** - Python files (`.py` files) that:
- Can be run repeatedly with consistent results
- Can be shared with others
- Can be version controlled (Git)
- Can be imported as modules
- Run without needing a browser or notebook interface

### Script vs Interactive Code

**Interactive (Jupyter/Colab/REPL):**
```python
# Run once, immediate feedback
data = [1, 2, 3, 4, 5]
print(sum(data) / len(data))
# 3.0
```

This is great for:
- Learning and exploring
- Quick calculations
- Data analysis and visualization
- Prototyping ideas

**Script (reusable, shareable `.py` file):**
```python
# analyze.py
def calculateAverage(data):
    """Calculate the arithmetic mean of a list."""
    return sum(data) / len(data)

def main():
    """Main program entry point."""
    data = [1, 2, 3, 4, 5]
    average = calculateAverage(data)
    print(f"Average: {average}")

if __name__ == "__main__":
    main()
```

This is better for:
- Production code
- Sharing with collaborators
- Running on servers or clusters
- Building reusable tools

### Anatomy of a Python Script
```python
#!/usr/bin/env python3
"""
gradeAnalyzer.py - Analyze student performance data

Usage: python gradeAnalyzer.py datafile.csv
Author: Your Name
Date: October 2025
"""

# Standard library imports first
import sys
import os

# Third-party imports second
import numpy as np

# Local imports last
from utilities import loadData

# Constants at module level
PASSING_GRADE = 70
OUTPUT_FILE = "results.txt"

# Global configuration (use sparingly)
verbose = False

def calculateStats(grades):
    """Calculate basic statistics for grades."""
    return {
        'mean': np.mean(grades),
        'median': np.median(grades),
        'std': np.std(grades)
    }

def main():
    """Main program entry point."""
    if len(sys.argv) != 2:
        print("Usage: python gradeAnalyzer.py datafile.csv")
        sys.exit(1)
    
    filename = sys.argv[1]
    data = loadData(filename)
    stats = calculateStats(data)
    print(f"Class average: {stats['mean']:.2f}")

if __name__ == "__main__":
    main()
```

### The `if __name__ == "__main__"` Guard
This critical pattern enables dual use of your code:

```python
# mathTools.py
def factorial(n):
    """Calculate n factorial."""
    if n <= 1:
        return 1
    return n * factorial(n - 1)

# Only runs when executed directly, not when imported
if __name__ == "__main__":
    # Test code
    print(f"5! = {factorial(5)}")
    print(f"10! = {factorial(10)}")
    print("Tests passed!")
```

Now you can use it two ways:

```python
# Way 1: Run as a script
# $ python mathTools.py
# 5! = 120
# 10! = 3628800
# Tests passed!

# Way 2: Import as a module
from mathTools import factorial
result = factorial(7)  # No test output!
```

### The Shebang Line
The first line `#!/usr/bin/env python3` is called a "shebang." It tells Unix-like systems (Linux, Mac) how to run the file:

```python
#!/usr/bin/env python3
"""My script"""

def main():
    print("Hello!")

if __name__ == "__main__":
    main()
```

To use it:
```bash
# Make executable (one time only)
chmod +x myScript.py

# Run directly (no need to type "python")
./myScript.py
```

On Windows, the shebang is ignored but doesn't hurt anything.


## Key Practices Summary

| Aspect | Do | Don't |
|--------|-----|-------|
| **Philosophy** | Write clear code AND comprehensive comments | Believe in "self-documenting" code |
| **Comments** | Explain why, context, business rules | State the obvious |
| **Docstrings** | Document purpose, params, returns, assumptions | Repeat function signature only |
| **Scripts** | Include error handling and validation | Assume perfect inputs |
| **Structure** | Separate concerns into well-named functions | Write monolithic code |
| **Arguments** | Use argparse for complex CLIs | Parse sys.argv manually |
| **Output** | Use logging for production code | Use print everywhere |
| **Audience** | Write for someone unfamiliar with Python | Write only for Python experts |

## Common Mistakes to Avoid

1. **Believing code alone is enough**
   - Wrong: "Good variable names mean I don't need comments"
   - Right: "Clear names help, but I still need to explain context, assumptions, and business logic"

2. **Over-commenting trivial operations**
   ```python
   # BAD: Too many obvious comments
   x = 0  # Initialize x to zero
   x = x + 1  # Increment x by one
   print(x)  # Print the value of x
   
   # GOOD: Comment explains non-obvious purpose
   skipCount = 0
   skipCount += 1  # Skip header row in CSV
   print(skipCount)
   ```

3. **Forgetting your audience**
   - Your code will be read by: collaborators, advisors, future you, maintainers
   - Most of these people are not Python experts
   - Comments bridge the gap between code and human understanding

4. **Forgetting the `if __name__ == "__main__"` guard**
   - Always include it so your script can be imported safely

5. **Using print instead of logging in long-running scripts**
   - Use logging for anything that runs more than a few seconds

6. **Not handling command-line arguments properly**
   - Check argument count and validate inputs before using them

## Check Your Understanding

1. Why is "self-documenting code" a myth? What can code tell you and what can't it tell you?
2. Why should you write comments even if your variable names are clear?
3. What's the difference between module, class, and function docstrings?
4. How do scripts differ from interactive notebook code?
5. When should you use `argparse` instead of `sys.argv`?
6. What are type hints and why are they useful?
7. Why use logging instead of print statements in production code?
8. Who is your audience when writing comments? Why does this matter?

# Appendix A: Running Scripts Professionally

### Handling Command-Line Arguments
```python
import sys
import os

def main():
    # Check for required arguments
    if len(sys.argv) < 2:
        print("Error: No input file specified")
        print(f"Usage: python {sys.argv[0]} <inputFile> [outputFile]")
        sys.exit(1)
    
    inputFile = sys.argv[1]
    
    # Optional argument with default
    outputFile = sys.argv[2] if len(sys.argv) > 2 else "output.txt"
    
    # Verify file exists
    if not os.path.exists(inputFile):
        print(f"Error: File '{inputFile}' not found")
        sys.exit(1)
    
    processFile(inputFile, outputFile)
    print(f"Results written to {outputFile}")

if __name__ == "__main__":
    main()
```

### Better Argument Handling with argparse
For more complex scripts, use the `argparse` module:

```python
import argparse

def main():
    parser = argparse.ArgumentParser(
        description="Analyze student grade data"
    )
    
    # Required argument
    parser.add_argument('inputFile', 
                       help='CSV file containing grades')
    
    # Optional arguments
    parser.add_argument('-o', '--output',
                       default='results.txt',
                       help='Output file (default: results.txt)')
    
    parser.add_argument('-v', '--verbose',
                       action='store_true',
                       help='Print detailed output')
    
    args = parser.parse_args()
    
    # Use the arguments
    if args.verbose:
        print(f"Processing {args.inputFile}...")
    
    processFile(args.inputFile, args.output)

if __name__ == "__main__":
    main()
```

This gives you automatic help messages:
```bash
$ python analyze.py --help
usage: analyze.py [-h] [-o OUTPUT] [-v] inputFile

Analyze student grade data

positional arguments:
  inputFile             CSV file containing grades

optional arguments:
  -h, --help            show this help message and exit
  -o OUTPUT, --output OUTPUT
                        Output file (default: results.txt)
  -v, --verbose         Print detailed output
```

### Proper Error Messages

Absolutely nothing is worse when debugging an input error than the system not telling you what it expected vs. what it got. It's due to laziness on the part of the programmer, who did not take the time to give a useful error message. Don't be that person!

```python
def divideNumbers(numerator, denominator):
    """Safely divide two numbers."""
    try:
        return numerator / denominator
    except ZeroDivisionError:
        print(f"Error: Cannot divide {numerator} by zero")
        return None
    except TypeError:
        print(f"Error: Both arguments must be numbers")
        print(f"Got: {type(numerator).__name__} and {type(denominator).__name__}")
        return None
```

### Using Logging Instead of Print
[Pro-level] For production code, use logging instead of print statements:

```python
import logging

# Configure logging at the start of your script
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s'
)

def processData(filename):
    logging.info(f"Processing {filename}")
    
    try:
        # Do work
        logging.debug("Reading file contents")  # Only shows in debug mode
        # ...
    except Exception as e:
        logging.error(f"Failed to process {filename}: {e}")
        return None
    
    logging.info("Processing complete")
    return result
```

Benefits of logging over print:
- Can be turned on/off without changing code
- Automatic timestamps
- Different levels (DEBUG, INFO, WARNING, ERROR)
- Can write to files instead of screen

# Appendix B: Modern Python Documentation

[Pro-level] Python 3.5+ supports type hints - optional annotations that document expected types. They appear after parameter names (with a colon) and after the function signature (with an arrow).

### Basic Type Hint Syntax

**Reading type hints:**
```python
def functionName(parameter: type) -> returnType:
```
- The `:` after a parameter means "this parameter should be of this type"
- The `->` before the colon means "this function returns this type"

**Simple examples:**
```python
def greet(name: str) -> str:
    """Return a greeting message."""
    return f"Hello, {name}!"

def add(a: int, b: int) -> int:
    """Add two integers."""
    return a + b

def calculateAverage(numbers: list) -> float:
    """Calculate mean of a list."""
    return sum(numbers) / len(numbers)
```

### Type Hints with Default Values
When a parameter has both a type hint and a default value:
```python
def createAccount(username: str, age: int = 0, active: bool = True) -> dict:
    """
    Create user account dictionary.
    
    Args:
        username: User's login name (required)
        age: User's age in years (default: 0)
        active: Whether account is active (default: True)
    
    Returns:
        Dictionary with account information
    """
    return {
        'username': username,
        'age': age,
        'active': active
    }
```

Reading this: "`username` must be a string, `age` should be an int with default 0, `active` should be a bool with default True, and the function returns a dict"

### Common Type Hints

```python
# Basic types
def processName(name: str) -> str:
    return name.upper()

def calculateAge(birthYear: int) -> int:
    return 2025 - birthYear

def computePrice(basePrice: float) -> float:
    return basePrice * 1.08

def isValid(data: bool) -> bool:
    return data

# Collection types (Python 3.9+)
def sumNumbers(numbers: list[int]) -> int:
    """Sum a list of integers."""
    return sum(numbers)

def getStudent(students: dict[str, int]) -> str:
    """Get student with highest grade."""
    # dict[str, int] means "keys are strings, values are ints"
    return max(students, key=students.get)

def uniqueItems(items: set[str]) -> list[str]:
    """Convert set to sorted list."""
    # set[str] means "set containing strings"
    return sorted(items)

def getCoordinates(point: tuple[float, float]) -> float:
    """Calculate distance from origin."""
    # tuple[float, float] means "tuple with exactly two floats"
    x, y = point
    return (x**2 + y**2) ** 0.5
```

### Reading Complex Type Hints

**Lists with specific content:**
```python
def processGrades(grades: list[float]) -> dict[str, float]:
    """
    Analyze grade list.
    
    Args:
        grades: list[float] means "a list where each item is a float"
    
    Returns:
        dict[str, float] means "a dict where keys are strings and values are floats"
    """
    return {
        'mean': sum(grades) / len(grades),
        'max': max(grades),
        'min': min(grades)
    }

# Calling it:
classGrades = [85.5, 92.0, 78.5, 90.0]  # list of floats
stats = processGrades(classGrades)
# Returns: {'mean': 86.5, 'max': 92.0, 'min': 78.5}
```

**Multiple possible types (Union):**
```python
def processId(studentId: int | str) -> str:
    """
    Convert student ID to string format.
    
    Args:
        studentId: int | str means "can be either an int OR a string"
    
    Returns:
        String representation of ID
    """
    return str(studentId)

# Both work:
processId(12345)      # int is OK
processId("12345")    # str is also OK
```

**Optional values (might be None):**
```python
def findStudent(name: str, students: list[dict]) -> dict | None:
    """
    Find student by name.
    
    Returns:
        dict | None means "returns a dict if found, or None if not found"
    """
    for student in students:
        if student['name'] == name:
            return student
    return None  # Not found

# Usage:
result = findStudent("Alice", roster)
if result is not None:  # Must check for None!
    print(result['grade'])
```

### Practical Example with Full Type Hints

```python
def calculateGrade(score: float, maxScore: float = 100.0) -> float:
    """
    Calculate percentage grade.
    
    Reading the signature:
    - score: float means "score must be a floating-point number"
    - maxScore: float = 100.0 means "maxScore is a float with default 100.0"
    - -> float means "returns a floating-point number"
    
    Args:
        score: Points earned
        maxScore: Maximum possible points
    
    Returns:
        Percentage grade (0-100)
    """
    return (score / maxScore) * 100.0

def processStudents(names: list[str], grades: list[float]) -> dict[str, float]:
    """
    Create a dictionary mapping student names to grades.
    
    Reading the signature:
    - names: list[str] means "a list where each element is a string"
    - grades: list[float] means "a list where each element is a float"
    - -> dict[str, float] means "returns a dict with string keys and float values"
    
    Args:
        names: List of student names
        grades: List of corresponding grades
        
    Returns:
        Dictionary with name keys and grade values
    """
    return dict(zip(names, grades))

# Usage examples:
studentNames = ["Alice", "Bob", "Charlie"]  # list[str]
studentGrades = [85.5, 92.0, 78.5]          # list[float]
gradeBook = processStudents(studentNames, studentGrades)
# Returns: {'Alice': 85.5, 'Bob': 92.0, 'Charlie': 78.5}
```

### Why Use Type Hints?

1. **Better IDE support:**
   ```python
   def getFullName(firstName: str, lastName: str) -> str:
       return f"{firstName} {lastName}"
   
   # IDE knows result is a string, so it offers string methods
   result = getFullName("Alice", "Smith")
   result.upper()  # IDE autocompletes .upper(), .lower(), etc.
   ```

2. **Catch errors before running:**
   ```python
   def addNumbers(a: int, b: int) -> int:
       return a + b
   
   # Tools like mypy will warn: "Expected int, got str"
   addNumbers("5", "10")  # Type error caught before running!
   ```

3. **Documentation that can't get out of date:**
   ```python
   # The signature tells you everything you need to know
   def calculateBMI(weight: float, height: float) -> float:
       return weight / (height ** 2)
   
   # Clear: weight and height are floats, returns a float
   ```

4. **Easier to understand code:**
   ```python
   # Without type hints - unclear what data types are expected
   def process(data, config, options):
       # What are these? Lists? Dicts? Strings?
       pass
   
   # With type hints - immediately clear
   def process(data: list[dict], config: dict[str, str], options: set[str]) -> bool:
       # data is a list of dicts
       # config is a dict with string keys and string values  
       # options is a set of strings
       # returns a boolean
       pass
   ```

### Important Notes About Type Hints

1. **Type hints are optional** - Python doesn't enforce them at runtime
   ```python
   def add(a: int, b: int) -> int:
       return a + b
   
   # This runs fine even though we passed strings!
   # Python doesn't stop you at runtime
   add("hello", "world")  # Returns "helloworld"
   ```

2. **Use tools like mypy to check types** - Run `mypy script.py` to find type errors before running

3. **Start simple** - You don't need type hints everywhere immediately. Add them to:
   - Public functions (ones others will call)
   - Functions with complex parameters
   - Functions where types aren't obvious

Type hints help:
- IDEs provide better autocomplete
- Catch type errors before running (with mypy)
- Document expected types without comments
- Make code easier to understand for readers

# Appendix C: Professional Script Structure Template

Here's a complete template for professional scripts:

```python
#!/usr/bin/env python3
"""
scriptName.py - One-line description

Longer description of what this script does,
any important details about how it works.

Usage:
    python scriptName.py <requiredArg> [optionalArg]

Author: Your Name
Date: October 2025
Version: 1.0.0
"""

import sys
import os

# Constants
DEBUG_MODE = False
VERSION = "1.0.0"

def helperFunction(data):
    """Helper function with descriptive name."""
    # Implementation
    return result

def main():
    """
    Main program logic.
    
    Returns:
        Exit code (0 for success, non-zero for errors)
    """
    if DEBUG_MODE:
        print(f"Running version {VERSION}")
    
    # Check command-line arguments
    if len(sys.argv) < 2:
        print(f"Usage: python {sys.argv[0]} <requiredArg>")
        return 1
    
    # Main logic
    try:
        result = helperFunction(sys.argv[1])
        print(f"Result: {result}")
        return 0  # Success
    except Exception as e:
        print(f"Error: {e}")
        return 1  # Failure

if __name__ == "__main__":
    exitCode = main()
    sys.exit(exitCode)
```

## 9. When Scripts Become Modules

As your project grows, you'll organize related functions into modules:

```
myProject/
    main.py          # Entry point
    dataLoader.py    # Data loading functions
    analysis.py      # Analysis functions
    plotting.py      # Visualization functions
```

Each module can be both used and tested:

```python
# dataLoader.py
"""Functions for loading and validating data files."""

def loadCSV(filename):
    """Load CSV file and return as list of dictionaries."""
    # Implementation
    return data

def validateData(data):
    """Check that data has required fields."""
    # Implementation
    return isValid

# Test code only runs when executed directly
if __name__ == "__main__":
    print("Testing dataLoader module...")
    testData = loadCSV("test.csv")
    print(f"Loaded {len(testData)} records")
    print("Tests passed!")
```

```python
# main.py
"""Main analysis script."""

from dataLoader import loadCSV, validateData
from analysis import calculateStatistics
from plotting import createHistogram

def main():
    data = loadCSV("grades.csv")
    if not validateData(data):
        print("Error: Invalid data")
        return 1
    
    stats = calculateStatistics(data)
    createHistogram(data, "gradeDistribution.png")
    print(f"Analysis complete. Mean: {stats['mean']:.2f}")
    return 0

if __name__ == "__main__":
    sys.exit(main())
```

