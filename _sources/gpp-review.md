# Weeks 1-4 GPP Review

Here are a selected (and slighlty edited if necessary) set of GPPs from the past 4 weeks. 

**NOTE**: These were selected with an eye toward covering all of the material we have worked on over the last 4 weeks. But inclusion of material here does not suggest that material NOT covered here will NOT be on the test! This document if designed to get you practicing on GPP problems away from the computer. We recommend doing this for ALL GPP problems!

# Week1B: Intro to Python!

- Enter an expression over two lines using the `\`
- So, without entering this code, what would `round(-5.7)` return?
- Create two number variables and print their values using one print statement.
- Create an integer variable and a float variable and print their types.
- Create an integer variable `num`, and print the values of `num` raised to the third power, and `num` raised to the fifth power.
- Write an expression using scientific notation that would result in the number 30000.
- Create a variable to store the cost of a bagel (let's say $2.25). Create a constant to store the tax rate (let's say 3 percent). What would the total cost of the bagel be? Round this to two decimal places since it's money.
- Create variables `x = 10` and `y = 3`. Calculate their sum and difference. Use `//` and `%` to divide them.

# Week 2A Morning Assignment: Working with Sequences
## Part 1: String Exploration
### Task 1.1: String Basics

Given this string: sentence = "The quick brown fox jumps over the lazy dog"

Write code to:
1. Print the first character
2. Print the last character
3. Print the length of the sentence
4. Check if the word "fox" is in the sentence

## Part 2: List Operations
### Task 2.1: List Creation and Access

Create a list containing the names of your three team members
`teamNames = ["bob", "sally", "george"]`

Write code to:
1. Print the first team member's name
2. Print the last team member's name
3. Print how many people are on your team
4. Add a fourth name "TA Helper" to the list

### Task 2.2: List Modification Challenge

Start with this list:
`numbers = [10, 20, 30, 40, 50]`

Without retyping the entire list, use list methods to:
1. Change the third number from `30` to `35`
2. Add the number 60 to the end
3. Insert the number 5 at the beginning (Careful, this is a little harder!)
4. Remove the number 40 from the list
5. Remove the last number from the list

Print the final list - it should be `[5, 10, 20, 35, 50]`

## Part 3: Mixed Practice
### Task 3.1: Data Processing
Your team has collected some survey data. Help clean and process it:

Raw survey responses (some have extra spaces, inconsistent capitalization)
`responses = ["yes", "NO", "maybe", "YES", "no", "Maybe", "yes"]`

Write code to:
1. Clean each response (convert to lowercase)
2. Count how many "yes", "no", and "maybe" responses there are
3. Create separate lists for each type of response
4. Create separate tuples for each type of response

# Week 2B Morning Assignment: Boolean Logic Practice

## Part 2: Weather Decision System

Create a program that helps decide what to wear based on weather conditions.

**Your Task:** Complete the TODO sections with appropriate Boolean expressions using parentheses.

**Important:** Use explicit Boolean comparisons throughout!

```python
# Get weather conditions
temperature = float(input("Enter temperature (°F): "))
isRaining = input("Is it raining? (yes/no): ").lower() == "yes"
isWindy = input("Is it windy? (yes/no): ").lower() == "yes"
humidity = float(input("Enter humidity percentage: "))

# Decision logic - complete these conditions using explicit comparisons
needsJacket = (temperature < 60) or isWindy
needsUmbrella = # TODO: Complete this condition
isComfortable = # TODO: Complete this condition (temp between 65-80, not raining, humidity < 70)
stayInside = # TODO: Complete this condition (very cold OR very hot OR raining AND windy)

# Output recommendations using explicit Boolean checks
print("\n--- Weather Recommendations ---")
if needsJacket == True:
    print("Bring a jacket!")

if needsUmbrella == True:
    print("Take an umbrella!")

if isComfortable == True:
    print("Perfect weather to be outside!")

if stayInside == True:
    print("Maybe consider staying indoors today")
```

## Part 3: Grade Calculator with Conditions

Create a grade evaluation system using clear Boolean naming and explicit comparisons:

**Your Task:** Complete the Boolean expressions using proper naming and parentheses.

```python
# Student information
homeworkScore = float(input("Enter homework average (0-100): "))
examScore = float(input("Enter exam score (0-100): "))
attendanceRate = float(input("Enter attendance rate (0-100%): "))
hasExtraCredit = input("Extra credit completed? (yes/no): ").lower() == "yes"

# Grade calculations
finalScore = (homeworkScore * 0.4) + (examScore * 0.6)
if hasExtraCredit == True:
    finalScore = finalScore + 5  # 5 point bonus

# Conditions to determine - work together on these using good Boolean naming
isPassing = # TODO: final score >= 60 AND attendance >= 75%
hasHonors = # TODO: final score >= 90 AND attendance >= 95%
needsImprovement = # TODO: homework < 70 OR exam < 65 OR attendance < 80%
isExcellent = # TODO: final score >= 95 AND attendance == 100 AND hasExtraCredit

# Output results using explicit Boolean comparisons
print(f"\nFinal Score: {finalScore:.1f}")
print(f"Attendance: {attendanceRate}%")

if isExcellent == True:
    print("Excellent work! You're a star student!")
elif hasHonors == True:
    print("Honors achievement!")
elif isPassing == True:
    print("Passing grade - well done!")
else:
    print("Not passing - please see instructor")

if needsImprovement == True:
    print("Areas for improvement identified")
```

# Week 3A Morning Assignment: Coding Basics Wrap-up and Selection Statements

## Part 1: f-strings, escape characters, the `input()` function and type casting

### Problem 1.2: Series Resistor Voltage Report

Write a Python program that:

1. Prompts the user to enter **two resistors**, including:  
   - The **name** of each resistor (string)  
   - The **resistance in ohms** (integer)  

2. Prompts the user to enter the **current in amps** (float), which is the same through both resistors in series.

3. Calculates:  
   - The **voltage across each resistor** using Ohm’s Law:  
     ```
     V = I * R
     ```  
   - The **total voltage** across both resistors:  
     ```
     V_total = V1 + V2
     ```  

4. Prints a **formatted report** including:  
   - Each resistor’s name in **uppercase**  
   - Resistance using the word `"ohms"`  
   - Current with **3 decimal places**  
   - Voltage with **2 decimal places**  
   - Columns aligned using **`\t`**  
   - A title and separator lines using **`\n`**  

**Example run:**
```
Enter name of resistor 1: R1
Enter resistance of R1 (ohms): 220
Enter name of resistor 2: R2
Enter resistance of R2 (ohms): 330
Enter current (amps): 0.125

SERIES RESISTOR REPORT
-----------------------------
Name        Resistance    Current    Voltage
R1          220 ohms      0.125 A   27.50 V
R2          330 ohms      0.125 A   41.25 V
-----------------------------
Total Resistance: 550 ohms
Total Voltage:    68.75 V
```

**Notes:**  
- Use `int()` to convert resistances to integers.  
- Use `float()` to convert current to a floating-point number.  
- Use f-strings for all formatted output.  
- Use escape characters `\n` for new lines and `\t` for column alignment.

## Part 2: Basic selection statements

### Problem 2.2: Prize Number Generator

Instead of soliciting user input, we could also use a random number as an input to test. Very soon we will talk about better ways to generate *real* random numbers, but for now, we'll stick with a more brute force approach.

Write a Python program that generates a “random-ish” number between 1 and 100 **without using any imports**. (We will cover imports in Week3B).

Your program should:

1. Ask the user to type something unpredictable (letters, numbers, or a mix).  
2. Convert the user input into a number between 1 and 100 using a simple calculation (for example, using the length of the input).  
3. Print the generated number.  
4. Check if the number is smaller than 5.  
   - If it is, print a message telling the user they have won a prize.  
   - Otherwise, print a message saying there is no prize.  

**Example run:**
```
Type something random: hello
The generated number is: 5
Sorry, no prize this time.
```
```
Type something random: a
The generated number is: 1
Congratulations! You won a prize!
```

**Hints:**
- Use `len()` to get the length of the input.  
- Use arithmetic (`%`, `+`) to keep the number in the 1–100 range.  
- Use an `if/else` statement to check if the number is smaller than 5.  
- Use `print()` to display messages.

## Part 3: Nested selection statements

### Problem 3.3: Selections AND booleans

In **Week 2**, we learned about *booleans* and the boolean relational operators: `==`, `!=`, `<`, `>`, `<=`, `>=`, `not`, `and`, and `or`. Using those operators.

Write a Python program that asks the user to enter:  
- The number of cats  
- The number of dogs  

Then use **booleans** and the boolean relational operators (`==`, `!=`, `<`, `>`, `<=`, `>=`, `not`, `and`, `or`) to compare the two values.  

Your program should print messages according to the following rules:  

1. If there is at least one cat, print `Squeee!`  
2. If there are 5 cats or more, print `Purr...`  
3. If there are 5 dogs or more, print `Arf!`  
4. If there are either more than 10 cats **or** more than 10 dogs, print `Where frends?`  
5. If there are more than 10 cats **and** more than 10 dogs, print `Frends!`  
6. If there are exactly as many cats as dogs, print `Perfection.`  

To get practice with more boolean logic, also add these extra checks:  
7. If the number of cats and dogs is **not equal**, print `Not balanced.`  
8. If there are fewer cats than dogs, print `More dogs than cats.`  
9. If there are at least twice as many cats as dogs, print `Cats rule!`  
10. If there are at least twice as many dogs as cats, print `Dogs rule!`  
11. If there are **no cats and no dogs** (zero of both), print `No pets at all!`  

**Example run:**
```
Enter the number of cats: 12
Enter the number of dogs: 12
Squeee!
Purr...
Arf!
Where frends?
Frends!
Perfection.
```

**Requirements:**  
- Use at least **6 different boolean operators** (`==`, `!=`, `<`, `>`, `<=`, `>=`, `not`, `and`, `or`) in your solution.  
- Each rule should be checked with an `if` statement (not `elif`), because more than one condition may be true.

# Week 3B Morning Assignment: `for` and `while` loops

## Part 1: `for` loops
### Part 1.7: Running products

 A running product is the product of numbers. A running sum is initialized to 0, so the first time something is added to it, it does not change. A running product, on the other hand, is initialized to 1. The following should calculate a running product of the integers from 1 to 5.

 Identify what is wrong and write code that fixes it.


 ```python
 runprod = 1
for i in range(6):
    runprod = runprod * i
    print('The product so far is', runprod)
```

### Part 1.8: Loops and selection

Now let's work on combining loops with selection (`if`, `if-else`) statements. Write code that will loop to generate 5 random integers, each in the range from 1 to 30. For each, print the integer and whether or not it was less than 15.

Use

```python
from random import randint
```

like you did in question 1.6 to solve this problem.

### Part 1.9: `for` and random `char` from a string

Write a program that will choose a **random character** from the string 'xyz'. If the character is 'x', it will loop to print '!' 30 times in a row, but if the character is not 'x', it will just print 'Sad'.

To solve this problem, we will need *a different function* from the `random` library: `choice`. See if you can figure out how to gain access to `choice`, following the template above.

**Hint**: use `help()` after you import `choice` to examine what `choice` does!
```
Help on method choice in module random:

choice(seq) method of random.Random instance
    Choose a random element from a non-empty sequence.
```

## Part 2: `while` loops
### Part 2.2 `while()` and random numbers

Write a loop that will generate and print random integers, each in the range from 1 to 20, until one of the random integers is greater than 15. Then, after the loop, print the random integer that ended the loop.

Use `randint` from `random` like you did above!

### Part 2.3: While Loops — Counting and Averages

Ahead of time, we do not know how many times the action of a `while` loop will be executed. However, it is frequently useful to **count** the number of times that the action has been executed.

Write a script that:  
- Prompts the user to enter **positive numbers**, one at a time.  
- Continues until the user enters a **negative number**.  
- Keeps track of both:  
  - The **sum** of the positive numbers entered.  
  - The **count** of how many numbers were entered.  
- At the end:  
  - If no positive numbers were entered, print an **error message**.  
  - Otherwise, print the **average** of the positive numbers.  
  - Also print how many tries it took, in the form:  
    ```
    It took xx tries.
    ```
  - If the user stops the loop after the very first input, print:  
    ```
    It took 1 try.
    ```

# Week 4A Morning Assignment: Nested `for` loops

### Problem 2: Formatted output with nested loops

Your task is to practice nested loops by writing a Python program that generates a simple pattern.

- Loop over the numbers 0 through 4.
- On each line, first print the number, followed by a colon (:).
- After the colon, print as many asterisks (*) as the number itself.
- Each line should show the number and its matching stars.

Example Output:

```
0:
1:*
2:**
3:***
4:****
```

Hint: You will need a loop inside a loop — the outer loop handles the numbers, and the inner loop handles printing the stars.

### Problem 6: Nested `while` and `for` loops

Write a script that will prompt the user for positive numbers. As long as the user is entering positive numbers, Print `Ni!` that many times on a single line for each. For example, if the user enters 3, `Ni!Ni!Ni!` would be printed.

### Problem 9: Running sum

Use a nested `for` loop to calculate a running sum of all of the integers in all of the lists in the variable `nestnumlist`
```python
nestnumlist = [[1, 3, 11], [2, 5], [4, 7, 2, 1]]
```

# Week 4B Morning Assignment: Advanced loops using list comprehensions

### Problem 2: Using complex `range` calls in `for` loops

* Loop to print `6 5 4 3 2` on one line.
* Write a program that calculates the sum of a series that uses every other denominator and alternates the sign of each term. For example, if `n = 10`, the series is `1/1 - 1/3 + 1/5 - 1/7 + 1/9`. Assume `n` is an integer greater than 1.

### Problem 4: Enumerate

* Use `enumerate` and a `for` loop to print the indices and individual characters from a string you chose.
* Create a list of strings. Then, for each string, print
  * the index of the string (naturally, e.g. 1,2,3.. instead of 0,1,2..)
  * the number of characters in the string
  * the individual characters.
  * For example, if the first string is "Hello", the first line of output would be `Word 1 "Hello" has 5 characters: H e l l o`

### Problem 5: Vectorizing with built-in operations
#### 5.3: Create a list of numbers. Find the minimum using the built-in **min** function, and also by using a loop.

### Problem 6: List comprehensions

#### 6.2: Get the absolute value of every element in a list of numbers. Do this 2 ways: using a list comprehension and using a `for` loop.

#### 6.4: Generate a set of vowels from a given string using a list comprehension. **Hint**: this will require using an `if` as part of the comprehension.

#### 6.5: Create a list of numbers. Write code that will create a new list which is, for every element in the original list, that number plus its index. For example, [4, 8, 2] would become [4+0, 8+1, 2+2] or [4, 9, 4]. Do this 3 ways: using a list comprehension with `range`, using a list comprehension with `enumerate`, and using a `for` loop.

#### 6.7: In a list comprehension, you can have multiple `for` clauses. Create a list of x+y values, where x ranges from 1 to 3 and y ranges from 1 to 4, using a list comprehension.