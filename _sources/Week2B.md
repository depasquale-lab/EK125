# Class 2B: Booleans and Basic Logic

## Learning Objectives
By the end of this reading, you should understand:
- What Boolean values are and how they work
- How to name Boolean variables intelligently
- Boolean operators (and, or, not)
- Comparison operators and their behavior with different data types
- How to use Booleans in conditional statements
- Truth tables for logical operations
- Operator precedence in Boolean expressions
- Short-circuit evaluation
- Advanced Boolean patterns and best practices

## 1. Introduction to Booleans

A Boolean is a data type that can only have two values: `True` or `False`. Named after mathematician George Boole (1815-1864), Booleans are fundamental to programming logic and decision-making. In computer science, Booleans form the foundation of all logical operations and control flow.

```python
isRaining = True
isWeekend = False
print(type(isRaining))  # <class 'bool'>
print(isinstance(isRaining, bool))  # True
```

### Historical Context
George Boole developed Boolean algebra in the mid-19th century as a way to represent logic mathematically. This work became crucial for computer science, as digital computers operate using binary logic (on/off, 1/0, true/false).

### Boolean Values in Memory
In Python, `True` and `False` are actually special instances of integers:
```python
print(int(True))   # 1
print(int(False))  # 0
print(True + True) # 2 (Don't ever do this, it's wrong and python is bad for allowing it. It should be ashamed.)
```

## 2. Intelligent Naming of Boolean Variables

One of the most important skills in programming is choosing clear, descriptive names for your Boolean variables. A well-named Boolean variable should pose a question that the Boolean value answers.

### The Golden Rule: Boolean Names Should Ask Questions

**Good Boolean names clearly indicate what `True` means:**

```python
# Excellent Boolean names - crystal clear what True means
hasCats = True          # User has cats
isValid = True          # Something is valid  
isLoggedIn = False      # User is logged in
canDrive = True         # Person can drive
hasPermission = False   # User has permission
isComplete = True       # Task is complete
shouldContinue = False  # Program should continue
```

**Poor Boolean names - ambiguous what `True` means:**

```python
# Bad Boolean names - unclear what True represents
cats = True            # Does this mean "has cats"? "likes cats"? "is a cat"?
valid = False          # What is valid? The input? The user? The state?
user = True            # Is this a user? Does a user exist? Is user active?
status = False         # What status? What does False mean for status?
```

### Common Boolean Naming Patterns

**1. `has` prefix - indicates possession or presence:**
```python
hasChildren = True
hasInsurance = False
hasValidLicense = True
hasRequiredPermissions = False
```

**2. `is` prefix - indicates state or condition:**
```python
isAdult = True
isActive = False
isVisible = True
isAuthenticated = False
```

**3. `can` prefix - indicates ability or permission:**
```python
canEdit = True
canDelete = False
canAccessAdmin = True
canWithdraw = False
```

**4. `should` prefix - indicates recommended action:**
```python
shouldSaveFile = True
shouldShowWarning = False
shouldRetry = True
shouldExit = False
```

**5. `will` prefix - indicates future action:**
```python
willExpire = True
willOverwrite = False
willSendEmail = True
```

### Before and After Examples

**Poor naming:**
```python
age = 25
adult = (age >= 18)        # Unclear: is this person an adult?
weekend = dayOfWeek in ["saturday", "sunday"]  # Unclear meaning
rain = weatherData["precipitation"] > 0  # Ambiguous
```

**Better naming:**
```python
age = 25
isAdult = (age >= 18)           # Clear: person is an adult
isWeekend = dayOfWeek in ["saturday", "sunday"]  # Clear: today is weekend
isRaining = weatherData["precipitation"] > 0     # Clear: it is raining
```

### Negative Boolean Names - Use with Caution

Be careful with negative Boolean names as they can create confusing double negatives:

```python
# Confusing negative naming
isNotLoggedIn = False
if not isNotLoggedIn:  # Double negative - hard to read!
    showDashboard()

# Better positive naming
isLoggedIn = True
if isLoggedIn:  # Much clearer!
    showDashboard()
```

**When negative names make sense:**
```python
# Sometimes negative names are more natural
isEmpty = (len(items) == 0)
isInvalid = not isValidFormat(userInput)
isDisabled = not userAccount.isActive
```

### Boolean Function Names

Functions that return Booleans should also follow question-naming patterns:

```python
def isValidEmail(email):
    return "@" in email and "." in email

def hasEnoughFunds(balance, amount):
    return balance >= amount

def canAccessResource(user, resource):
    return user.isActive and user.hasPermission(resource)

def shouldShowDisclaimer(userAge, hasAcceptedTerms):
    return userAge < 18 or not hasAcceptedTerms
```

## 3. Comparison Operators

Comparison operators evaluate relationships between values and return Boolean results. These operators work with various data types, not just numbers.

| Operator | Meaning | Example |
|----------|---------|---------|
| `==` | Equal to | `(5 == 5)` returns `True` |
| `!=` | Not equal to | `(5 != 3)` returns `True` |
| `>` | Greater than | `(7 > 3)` returns `True` |
| `<` | Less than | `(2 < 8)` returns `True` |
| `>=` | Greater than or equal | `(5 >= 5)` returns `True` |
| `<=` | Less than or equal | `(3 <= 7)` returns `True` |

### Comparing Different Data Types

```python
# Numbers
age = 20
canVote = (age >= 18)  # True
isMinor = (age < 18)   # False

# Strings (lexicographic comparison)
name1 = "Alice"
name2 = "Bob"
print(name1 < name2)  # True (A comes before B alphabetically)

# String length comparisons
word1 = "hello"
word2 = "world"
print(len(word1) == len(word2))  # True (both have 5 characters)

# Case sensitivity matters
print("Hello" == "hello")  # False
print("Hello".lower() == "hello")  # True
```

### Chaining Comparisons
Python allows chaining comparisons, which is both powerful and readable:

```python
score = 85
# Instead of: (score >= 80) and (score < 90)
isB = (80 <= score < 90)  # True - much cleaner!

temperature = 72
isComfortable = (65 <= temperature <= 75)  # True
```

## 4. Boolean Operators

### AND Operator (`and`)
Returns `True` only when both conditions are `True`. This is like requiring two keys to open a lock.

```python
hasLicense = True
hasInsurance = True
hasRegistration = True
canDrive = hasLicense and hasInsurance and hasRegistration  # True

# Real-world example
age = 25
hasJob = True
creditScore = 750
canGetLoan = (age >= 21) and hasJob and (creditScore >= 700)  # True
```

### OR Operator (`or`)
Returns `True` when at least one condition is `True`. This is like having multiple ways to achieve something.

```python
isWeekend = False
isHoliday = True
isSick = False
canSleepIn = isWeekend or isHoliday or isSick  # True

# Real-world example
hasCollegeDegree = False
hasExperience = True
hasCertification = True
isQualified = hasCollegeDegree or (hasExperience and hasCertification)  # True
```

### NOT Operator (`not`)
Reverses the Boolean value. This is useful for expressing negative conditions clearly.

```python
isRaining = False
isSunny = not isRaining  # True

# Multiple negations can be confusing - be careful!
isNotUnavailable = not (not isRaining)  # Same as isRaining

# Better to use positive logic when possible
isAvailable = True  # Clearer than isNotUnavailable = not isUnavailable
```

## 5. Truth Tables

Understanding truth tables is crucial for predicting Boolean expression results.

### AND Truth Table
| **AND** | A=False | A=True |
|---------|---------|--------|
| B=False | **False** | **False** |
| B=True | **False** | **True** |

### OR Truth Table
| **OR** | A=False | A=True |
|--------|---------|--------|
| B=False | **False** | **True** |
| B=True | **True** | **True** |

### NOT Truth Table
| **NOT** | **Result** |
|---------|------------|
| A=False | **True** |
| A=True | **False** |

### Complex Truth Table Example
```python
# Let's trace through: (A and B) or (not C)
# When A=True, B=False, C=True
A = True
B = False
C = True

step1 = A and B      # True and False = False
step2 = not C        # not True = False
result = step1 or step2  # False or False = False
```

## 6. Operator Precedence and Parentheses

Just like mathematical operators, Boolean operators have precedence rules:

1. `not` (highest precedence)
2. `and`
3. `or` (lowest precedence)

```python
# These expressions are equivalent:
result1 = not True or False and True
result2 = (not True) or (False and True)  # False or False = False

# But this is different:
result3 = not (True or False) and True  # not True and True = False
```

### Best Practice: Use Parentheses for Clarity
```python
# Hard to read
eligibleForDiscount = (age >= 65) or isStudent and hasValidId or isVeteran

# Much clearer
eligibleForDiscount = (age >= 65) or (isStudent and hasValidId) or isVeteran
```

## 7. Short-Circuit Evaluation

Python uses short-circuit evaluation, meaning it stops evaluating as soon as the result is determined.

### AND Short-Circuiting
```python
def expensiveFunction():
    print("This is expensive to compute!")
    return True

hasPermission = False
# expensiveFunction() is never called because hasPermission is False
result = hasPermission and expensiveFunction()
```

### OR Short-Circuiting
```python
isWeekend = True
# checkWorkSchedule() is never called because isWeekend is True
canRelax = isWeekend or checkWorkSchedule()
```

### Practical Applications
```python
# Safe division
divisor = 0
# This won't cause a division by zero error
safeDivision = (divisor != 0) and (100 / divisor > 10)

# Safe list access
myList = [1, 2, 3]
index = 5
# This won't cause an index error
hasElement = (len(myList) > index) and (myList[index] == 3)
```

## 8. Using Booleans in Conditional Statements

### Simple Conditions
```python
temperature = 75
humidity = 60

if (temperature > 70) and (humidity < 70):
    print("Perfect weather for a picnic!")
elif (temperature > 70) or (humidity < 40):
    print("Pretty good weather")
else:
    print("Maybe stay inside today")
```

### Complex Decision Trees
Sometimes you need to make decisions based on multiple factors with different priorities. Here's an example of a clothing recommendation system that considers temperature as the primary factor, then adjusts for weather conditions.

The logic prioritizes temperature first (cold vs. cool vs. warm), then considers weather conditions within each temperature range, flowing from most restrictive conditions to least restrictive:

```python
def determineClothing(temperature, isRaining, isWindy):
    # Primary decision: temperature range
    if temperature < 32:  # Cold weather (below freezing)
        if isRaining:
            return "Winter coat and waterproof boots"
        elif isWindy:
            return "Heavy winter jacket and scarf"
        else:
            return "Warm winter coat"
    elif temperature < 50:  # Cool weather 
        if isRaining and isWindy:
            return "Rain jacket and layers"
        elif isRaining:
            return "Light jacket and umbrella"
        elif isWindy:
            return "Windbreaker"
        else:
            return "Light sweater"
    else:  # Warm weather (50°F and above)
        if isRaining:
            return "Light rain jacket"
        else:
            return "T-shirt and shorts"
```

**Why this structure matters:** You might notice that we're testing temperature multiple times (`< 32`, then `< 50`), which might seem inefficient. However, the order is critically important! The `if-elif-else` structure ensures that:

1. **Only one branch executes** - once a condition is met, the rest are skipped
2. **Order determines precedence** - we check coldest temperatures first because they override other considerations
3. **Each temperature range gets its own weather logic** - what you wear when it's raining at 25°F is very different from raining at 75°F

If we reversed the order and checked `temperature < 50` first, a temperature of 25°F would incorrectly match the "cool weather" branch instead of the "cold weather" branch!

### Guard Clauses
Sometimes it's clearer to handle edge cases first:

```python
def processOrder(items, hasPayment, isLoggedIn):
    # Guard clauses handle problems early
    if not isLoggedIn:
        return "Please log in first"
    
    if items == []:
        return "Cart is empty"
    
    if not hasPayment:
        return "Please add payment method"
    
    # Main logic only runs if all conditions are met
    return "Order processed successfully"
```

## 9. Common Pitfalls and Best Practices

### Assignment vs. Equality
```python
# Wrong - assignment (single =)
if score = 100:  # SyntaxError: invalid syntax
    # Be careful in other languages though, many will let you do this! Boo other languages!

# Correct - comparison (double ==)
if score == 100:
    print("Perfect score!")

# Also correct - for checking truthiness
if score:  # True if score is non-zero
    print("You have a score!")
```

### Comparing to Boolean Values
```python
# Avoid this shorthand - it's unclear what's being tested
isReady = True
if isReady:  # What is this checking? That isReady exists? That it's True?
    print("Ready!")

# Much better - explicit and clear
if isReady == True:  # Crystal clear - checking if isReady equals True
    print("Ready!")

# For negative conditions, also be explicit
if isReady == False:
    print("Not ready yet")
```

**Note:** We won't use these shorthand forms in class, but you'll see them in the wild when reading other people's code.

### Truthy and Falsy Values
In Python, some non-Boolean values are considered "truthy" or "falsy":

**Falsy values:**
- `False`
- `0`, `0.0`, `0j` (numeric zeros)
- `""`, `''` (empty strings)
- `[]`, `()`, `{}` (empty collections)
- `None`

**Truthy values:** Everything else

```python
# Examples of truthy/falsy evaluation
score = 85
if score:  # True because 85 is truthy
    print("You have a score!")

name = ""
if not name:  # True because empty string is falsy
    print("Please enter your name")

items = []
if items == []:  # Explicit check for empty list
    print("Your cart is empty")
else:
    print(f"You have {len(items)} items")
```

### De Morgan's Laws
These logical equivalencies can help simplify complex conditions:

```python
# De Morgan's Law 1: not (A and B) == (not A) or (not B)
# These are equivalent:
condition1 = not (hasLicense and hasInsurance)
condition2 = (not hasLicense) or (not hasInsurance)

# De Morgan's Law 2: not (A or B) == (not A) and (not B)
# These are equivalent:
condition3 = not (isWeekend or isHoliday)
condition4 = (not isWeekend) and (not isHoliday)
```

## 10. Advanced Boolean Patterns

### The Ternary Operator
Python's ternary operator provides a concise way to assign values based on conditions:

```python
# Traditional if-else
if age >= 18:
    status = "adult"
else:
    status = "minor"

# Ternary operator
status = "adult" if (age >= 18) else "minor"

# Can be chained, but use sparingly
grade = "A" if (score >= 90) else "B" if (score >= 80) else "C" if (score >= 70) else "F"
```

### Multiple Condition Checking
```python
def checkStudentEligibility(gpa, credits, hasApplication):
    # Check multiple conditions systematically
    meetsGpaRequirement = (gpa >= 3.0)
    meetsCreditsRequirement = (credits >= 60)
    hasCompletedApplication = hasApplication
    
    # All conditions must be true
    isEligible = meetsGpaRequirement and meetsCreditsRequirement and hasCompletedApplication
    
    # Provide detailed feedback
    if not meetsGpaRequirement:
        print("GPA requirement not met (need 3.0 or higher)")
    if not meetsCreditsRequirement:
        print("Credit requirement not met (need 60 or more)")
    if not hasCompletedApplication:
        print("Application not submitted")
        
    return isEligible
```

## 11. Practice Questions for Self-Check

1. What does `True and False or True` evaluate to? (Answer: True)
2. What is the result of `not (5 > 3 and 2 < 4)`? (Answer: False)
3. When would you use the `or` operator in a conditional statement?
4. What's the difference between `=` and `==` in Python?
5. What does `not (True or False) and True` evaluate to? (Answer: False)
6. Is `"hello" and "world"` truthy or falsy? (Answer: Truthy - returns "world")
7. What happens with `[] or "default"`? (Answer: Returns "default")
8. How does short-circuit evaluation work with `False and expensiveFunction()`?

## 12. Real-World Applications

### User Authentication
```python
def authenticateUser(username, password, isAccountActive):
    hasValidCredentials = (len(username) > 0) and (len(password) >= 8)
    accountStatus = isAccountActive and not isAccountLocked
    return hasValidCredentials and accountStatus
```

### Business Rules
```python
def calculateShipping(weight, distance, isPriority):
    freeShippingEligible = (weight > 50) or (distance < 100)
    rushDelivery = isPriority and (distance < 200)
    
    if freeShippingEligible and not rushDelivery:
        return 0
    elif rushDelivery:
        return 25
    else:
        return 10
```

### Data Validation
```python
def validateFormData(age, email, phone):
    validAge = (18 <= age <= 120)
    validEmail = ("@" in email) and (len(email) >= 6)
    validPhone = (len(phone) >= 10) and phone.isdigit()
    
    return validAge and validEmail and validPhone
```

## Key Takeaways

- Booleans are essential for program logic and decision-making
- Boolean variable names should ask questions that their values answer
- Comparison operators create Boolean values from comparisons
- Boolean operators (`and`, `or`, `not`) combine Boolean values according to specific rules
- Understanding truth tables helps predict logical operation results
- Operator precedence matters: `not` before `and` before `or`
- Short-circuit evaluation can improve performance and prevent errors
- Use parentheses to make complex expressions clear
- Avoid comparing directly to `True` or `False`
- Understanding the structure of decision trees helps write better conditional logic

## Looking Ahead

Understanding Boolean logic is fundamental to programming because it forms the basis for:
- Conditional statements and control flow
- Loop conditions and termination
- Function return values and validation
- Algorithm design and optimization
- Database queries and filtering
- User interface logic and form validation

**Next:** In class, we'll practice writing complex Boolean expressions and see how they're used in real programming scenarios. We'll also explore debugging techniques to help you find and fix Boolean logic errors in your code.