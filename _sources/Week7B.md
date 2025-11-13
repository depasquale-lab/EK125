# Week 7B Preread: User-Defined Functions and Modular Program introduction

## User-defined functions (Morning)

We have seen many built-in functions in Python. You can also write your own functions, which are called user-defined functions.  Related user-defined functions can be stored in user-defined modules, which can then be imported into programs for use.

When we write a function, we write the function definition. The function can then be called, much like built-in functions. 

The general form of a function definition is:

```python
def functionname(parameters):
    """ Documentation string. """
    # Function body
```

The first line is called the function header. It consists of the reserved word `def`, the name of the function, parameters (if any) in parentheses, followed by a colon. The rest of the function, which is indented, is the function body. This typically begins with a documentation string (docstring), in triple quotes. The docstring should be sentence(s) describing what the function does, beginning with a capitalized word and ending with a period.  After the docstring there are the statements in the body of the function.  It is common to indent the body of the function 4 spaces.

The function is called by giving the name of the function, and passing arguments in parentheses that will correspond to the parameters in the function header:

```python
functionname(arguments)
```

Sometimes the arguments in the function call are referred to as actual parameters and the parameters in the function header are referred to as formal parameters.

Although this is somewhat arbitrary, we will begin with two types of functions:

- Functions that calculate and return a value
- Functions that accomplish a task

Two main reasons for writing our own functions are: 

- To be able to reuse code by calling the function whenever it is needed
- To write modular programs

If there is code that will be repeated at different times, perhaps using different values, it can be made into a function and then called whenever needed.

Modular programs are programs that consist of a series of functions that do the actual work. The functions are called by a main program, which can be implemented as either a script or function.

### Functions with Return

We have seen examples of built-in functions that calculate and return a value, such as the `round` function. User-defined functions can also accomplish this using the `return` statement.

As an example, we will write a function that returns the area of a square. In order to calculate the area of a square, the function needs the length of each side of the square, so the side length must be passed as an argument to the function. Here is a function definition for a function named sqrarea that accomplishes this:

```python
def sqrarea(sidelen):
    """This function calculates the area of a square."""
    sqar = sidelen**2
    return sqar
```

The function can then be called by using the name of the function and passing a number for the length of a side of the square:

```python
>>> sqrarea(3)
9
```

In the function call, the side length, 3, was passed to the parameter `sidelen` in the function. The function then squared this, and returned the result.  We say that when the function is called, control is sent to the function, and the function begins executing. The return statement not only returns the value (in this case, 9), but also sends control out of the function.

In general a docstring describes what a function does and can be displayed using the help function:

```python
>>> help(sqrarea)
Help on function sqrarea in module __main__:
```

```
sqrarea(sidelen)
    This function calculates the area of a square.
```

The variable `sqar`, which was used in the function body, was not necessary. The return statement could just return an expression:

```python
def sqrarea(sidelen):
    """This function calculates the area of a square."""
    return sidelen**2
```

The value returned from the function would normally be stored in a variable, e.g.

```python
>>> mysquare = sqrarea(3)
>>> mysquare
9
```

In this example, one argument was passed to the function.  For a function to calculate the area of a rectangle, both the length and width of the rectangle must be passed to the function, so there will be two parameters in the function header and two arguments passed in the function call:

```python
def rectarea(rlen, rwid):
    """This function calculates the area of a rectangle."""
    return rlen * rwid
```
```
>>> rectarea(2, 4)
8
```

The values of the arguments are passed to the corresponding parameters in the function, so the first argument, 2, is passed to the first parameter `rlen`, and the second argument 4 is passed to the second parameter `rwid`.

Notice that the `return` statement returned the result of an expression; an intermediate variable could be used but is not necessary.

In Python, the `return` statement can only return one object. If it is desired to have a function return more than one thing, they can be stored in one data structure (such as a tuple or list), and that can be returned.  In the following example, a function calculates and returns both the area and the perimeter of a square, by storing both in a list.

```python
def sqrarea_perim(sidelen):
    """Calculates the area and perimeter of a square."""
    sqar = sidelen**2
    sqperim = 4 * sidelen
    results = [sqar, sqperim]
    return results
```
```
>>> sqrarea_perim(11)
[121, 44]
```

Using a tuple instead of a list is perhaps easier to understand, and it is easier to use the results.

```python
def sqrarea_perim(sidelen):
    """Calculates the area and perimeter of a square."""
    sqar = sidelen**2
    sqperim = 4 * sidelen
    return sqar, sqperim
```
```
>>> area, perim = sqrarea_perim(11)
>>> print(area, perim)
121 44
```

### Functions with no Return

In many cases functions calculate and return values, but in some cases functions just accomplish a task, such as printing.

For example, the following function receives as parameters a string and an integer `n`, and prints the string n times in a row on one line. After that, it moves the cursor down.

```python
def printstrs(instr, n):
    """This function prints instr n times."""
    for i in range(n):
        print(instr,end='')
    print()
```

Here are two examples of calling the function:

```python
>>> printstrs('x',3)
xxx
```
```python
>>> printstrs("Hi", 5)
HiHiHiHiHi
```

Notice that the function does not have a `return` statement, since it is not calculating anything.  However, all functions return something regardless of whether there is a return statement or not. The default value that is returned is a special value `None`, as we can see by passing a call to the function `printstrs` to the print function.  In the following example, `aa` and then the newline gets printed by the function, and then the result returned by the function call, `None`, is printed once the function stops executing and returns control.

```python
>>> print(printstrs('a',2))
aa
None
```

The `printstrs` function prints the line `aa` and returns `None`. The print function then prints the returned value, `None`.  This is equivalent to:

```python
>>> x = printstrs('a',2)
>>> print(x)
```

It is not always necessary to pass arguments to functions (whether they explicitly return something or not).  For example, we may wish to have a function that just prints a set of instructions. 

```python
def printinstruct():
    """This function just prints stuff."""
    print('Please sit up')
    print('Do not slouch!')
    print('Chew with your mouth closed')
```
```
>>> printinstruct()
Please sit up
Do not slouch!
Chew with your mouth closed
```

A function like this that just prints helps to facilitate modular programs. Of course, the instructions printed by a function would normally refer to actions that the user must take when running a program!

### Scope

The scope of a variable or object is where that object is valid. We will discuss scope more fully in a following lecture, but introduce it here so as to not sow confusion. 

For functions, everything that is defined in the function is valid only in that function. We say that variables defined in a function are `local` to that function, meaning that their scope is the function. Formally, what happens is that every function has its own symbol table, which contains the names of the variables and their values. Once a function is called and control is sent to the function, its symbol table is created. The function’s symbol table only exists while the function is executing.

For example, for the `sqarea` function, the symbol table contains the local variable `sqar`, as well as the parameter `sidelen`.

```python
def sqrarea(sidelen):
    """This function calculates the area of a square."""
    sqar = sidelen**2
    return sqar
```

After the function is called, attempting to reference either `sqar` or `sidelen` will result in an error. This is because after the function stops executing, the symbol table, the variable `sqar`, and the parameter `sidelen` no longer exist.

```python
>>> area = sqrarea(4)
>>> print(sqar)

NameError: name 'sqar' is not defined
```

### Lambda Functions

Anonymous functions, called lambda functions in Python, are very short one line functions that do not require a formal function definition using `def`.  Lambda functions are created using the keyword `lambda`. The general form is:

```python
lambda argument(s): expression
```

The function begins with the reserved word `lambda`, then argument(s) followed by a colon and then an expression that uses the arguments; the result of the expression is returned.

If the lambda function is stored in a variable, the variable can be used to call the function. For example,

```python
>>> times3 = lambda num: num * 3
>>> times3(11)
33
```

One advantage of a lambda function is that it is shorter than a function definition and does not require the header with `def` or the `return` statement. A disadvantage is that the “body” of the function is confined to one simple expression.

## Modular Programs (Afternoon)

The following is an introduction to what we will discuss in the afternoon. This content is not strictly necessary to complete the pre-class assessment or the GPP, but it might be useful!

### Modular Programs

In general in programming, a modular program is one in which the program is broken down into separate tasks, and each task is implemented as a function. This facilitates the top-down design approach of taking a problem, breaking it down into pieces, and implementing each as a function.  The program then consists basically of calling the functions to complete each of the tasks.

A basic algorithm for many programs is:

- Get input(s)
- Calculate result(s), using the input(s)
- Print and/or display the result(s)

So, a simple outline of many programs would be to have:

- Function(s) to get the input(s)
- Function(s) to calculate result(s) using the input(s)
- Function(s) to display the result(s)

As an example, we will write a program that will prompt the user for the cost of an item in a store, and then calculate and print the total cost with a tax of 3%. The program will call 3 functions:

- A function to prompt for and return the cost
- A function to calculate the total cost including the tax
- A function to print the total cost

Here are the function definitions.

```python
def getcost():
    """ Prompts for item cost. """
    cost = float(input('Enter the item cost: '))
    return cost

def calctax(cost):
    """ Calculates and returns cost plus tax. """
    tax = cost * .03
    return cost + tax

def printtotcost(totcost):
    """ Prints cost plus tax. """
    print(f'The total cost is ${totcost:.2f}')
```

The functions could be called from a script, which is frequently called a main program. The program consists of calls to the functions. We will discuss scripts in future lectures. Other times the main program is implemented as a function rather than a script.


```python
# main program
cost = getcost()
totalcost = calctax(cost)
printtotcost(totalcost)
```

The output might look like this:

```
Enter the item cost: 11.11
The total cost is $11.44
```

Of course, to be more complete, the `getcost` function would error-check to make sure that the user enters a valid cost.

### Function Stubs

When writing a program that consists of a script calling multiple functions, the best practice is not to write the entire program and then execute it to see the results. Frequently, there are errors. When an error is encountered in a particular function, the problem may be in that function, or it might be the result of an incorrect argument passed to the function, that was obtained from another function. A more effective method for constructing the program is to use function stubs, which are place-holders for the actual functions.

The approach is:

- Sketch out the algorithm
- Decide what the functions are going to do
- Write the script, consisting of the calls to the functions (including the arguments that will get passed back and forth)
- Write function stubs for the individual functions
- Change each function stub to the actual function, one at a time

This methodical method of writing a program helps cut down on mistakes, and makes it easier to find bugs when they occur.

Function stubs should mimic what the function is eventually going to do, including using the arguments and returning the correct type(s).

For example, let’s say we are going to write a program that will prompt the user for a temperature in degrees Celsius, convert that to Fahrenheit, and print the results. 

We might have functions to:

- Prompt the user for degrees C, error-checking to make sure it’s valid, and return the result
- Receive the degrees C and from that calculate and return degrees F
- Print both degrees C and degrees F in a nice sentence format

The script for the main program might look like this:

```python
# Convert C to F
degC = getdegc()
degF = c2f(degC)
printCF(degC, degF)
```

Example function stubs might look like this:

```python
def getdegc():
    “””Prompts user for degrees Celsius.”””
    return 33

def c2f(degreesc):
    “””Converts degrees C to degrees F”””
    return degreesc + 5

def printCF(degreesc, degreesf):
    “””Prints degrees C and degrees F”””
   print(degreesc, degreesf)
```

The idea is to execute the program with the function stubs and make sure that values are being passed back and forth correctly. Eventually, after prompting the user and error-checking, the `getdegc` function will return a positive number. So, for now, we just return a positive number. Eventually, once we figure out the conversion, the `c2f` function will convert C to F. For now, we’ll just add 5 so we know that this function is correctly receiving the degrees C and using this number to calculate and return degrees F. Then, the `printCF` function will eventually print in a nice sentence format, but for now it will just print the values.  

The headers for the functions should not change. Putting the actual doc strings in the function stubs is a good idea.

Then, once that is working, modify the stubs one at a time. For example, you might start by modifying the `getdegc` function to prompt the user. Then, modify that function to include the error-checking.  Going about this systematically really does cut down on errors and makes it easier to find them.