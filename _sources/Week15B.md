# Week 15B Pre-read: AI-Assisted Programming

## Learning Objectives
By the end of this reading, you should understand:
- How different AI engines complement each other for complex problems
- Current capabilities and limitations of AI coding tools in late 2025
- Practical workflows for integrating AI into your development process
- How to verify and validate AI-generated solutions
- What programming skills will matter in 2026 and beyond

## 1. The AI Coding Landscape in Late 2025

### What AI Tools Can Do Well
- Delete all of your files (https://old.reddit.com/r/google_antigravity/comments/1p82or6/google_antigravity_just_deleted_the_contents_of/)
- Generate boilerplate code quickly
- Explain error messages in plain language
- Suggest multiple approaches to solve a problem
- Convert code between languages (Python to MATLAB, for example)
- Write documentation and comments for existing code
- Identify common bugs in your code
- Help with user interface iterations

### What AI Tools Cannot Do (or Do Poorly)
- Understand your specific project requirements without clear context
- Know about your code-specific constraints (like using camelCase instead of snake_case)
- Guarantee correct code every time
- Debug complex logic errors without guidance
- Understand nuanced performance requirements
- Replace your need to understand the code


### Available Tools and Their Strengths

As of late 2025, you have access to several major AI coding assistants, each with different characteristics:

**ChatGPT (GPT-5 and variants)**
- Strong at generating boilerplate code and common patterns
- Excellent human language output with little context
- Excellent for explaining concepts and debugging errors
- Large context window for working with substantial codebases
- Can struggle with very recent library updates

**Claude (Anthropic)**
- Particularly good at understanding context and nuance
- Strong at refactoring and code review
- Excellent at explaining trade-offs between approaches
- Handles complex multi-file projects well

**GitHub Copilot**
- Integrated directly into your IDE (VS Code, JetBrains, etc.)
- Fast inline suggestions as you type
- Good at completing patterns it recognizes
- Best for accelerating coding you already understand

**TerrierGPT (BU's instance)**
- **BLUF:** Approved for  data classified as [confidential, but not restricted use.](https://www.bu.edu/policies/data-classification-policy/)
- Configured for educational use
- Understands course-specific constraints
- May have access to course materials and examples

**Specialized Tools**
- Cursor: AI-first code editor with deep integration
- Replit AI: Good for quick prototypes and learning
- Codeium: Free alternative to Copilot

### What These Tools Actually Do

AI coding assistants are large language models trained on massive amounts of code from GitHub, documentation, Stack Overflow, and other sources. They predict what code should come next based on patterns they've seen millions of times. They don't "understand" code the way you do - they're doing sophisticated pattern matching.

This means:
- They're excellent at common patterns and well-established libraries
- They struggle with novel problems or unusual constraints
- They may confidently suggest code that looks right but has subtle bugs
- They perform better with clear, specific prompts
- They often need iteration to get exactly what you want

## 2. Multi-Engine Problem Solving

One of the most powerful techniques is using multiple AI engines to triangulate on a solution. Different models have different training data and architectures, so they make different mistakes and have different strengths.

### The Triangle Approach

For complex or critical problems, ask 2-3 different AI engines and compare their responses:

```python
# Problem: You need to parse complex log files with irregular formatting

# ChatGPT suggests: Using regex with error handling
def parseLogLine(line):
    pattern = r'(\d{4}-\d{2}-\d{2})\s+(\w+):\s+(.*)'
    match = re.match(pattern, line)
    if match:
        return {'date': match.group(1), 'level': match.group(2), 
                'message': match.group(3)}
    return None

# Claude suggests: More defensive parsing with explicit structure
def parseLogLine(line):
    parts = line.split(maxsplit=2)
    if len(parts) >= 3:
        date = parts[0]
        level = parts[1].rstrip(':')
        message = parts[2]
        return {'date': date, 'level': level, 'message': message}
    return None

# Copilot suggests: Using existing libraries
import logging.handlers
# Then points you to Python's logging module documentation
```

When you see different approaches:
1. Test each one with your actual data
2. Understand the trade-offs (regex is powerful but fragile, string splitting is simple but limited, libraries are robust but might be overkill)
3. Pick the approach that best fits your constraints
4. Consider hybrid solutions that combine the best ideas

### When Engines Agree and Disagree

**Agreement usually means**: The problem is common and the solution is well-established. You can be more confident in the result, but still test it.

**Disagreement often means**: 
- The problem has multiple valid solutions with different trade-offs
- The problem is less common or more specialized
- Your prompt might be ambiguous

When engines disagree, ask follow-up questions:
- "Why might someone choose approach A over approach B?"
- "What are the edge cases where each approach fails?"
- "Which approach is more maintainable?"

### Combining Insights

```python
# You ask ChatGPT: "How do I download data from an API with rate limiting?"
# Response focuses on the requests library and basic retry logic

# You ask Claude: "Same question"
# Response discusses exponential backoff and handling 429 status codes

# You ask Copilot: "Same question" 
# Suggests using the tenacity library for automatic retries

# Your synthesis:
import requests
from tenacity import retry, stop_after_attempt, wait_exponential

@retry(stop=stop_after_attempt(3), 
       wait=wait_exponential(multiplier=1, min=4, max=10))
def fetchWithRateLimit(url):
    response = requests.get(url)
    if response.status_code == 429:  # Rate limited
        raise Exception("Rate limit hit")
    response.raise_for_status()
    return response.json()

# You've combined: requests (ChatGPT), explicit 429 handling (Claude), 
# and tenacity library (Copilot)
```

## 3. Effective Prompting Techniques

### Context is Everything

AI engines have no memory between sessions and can't see your screen. You need to provide strong assistance. For instance:

```python
# Weak prompt:
# "Fix this bug"

# Strong prompt:
# "I have a function that should calculate compound interest, but it's giving 
# wrong results. Here's my code:
#
# def calculateInterest(principal, rate, years):
#     return principal * rate ** years
#
# When I call calculateInterest(1000, 0.05, 10), I expect around 1628.89 
# but I'm getting 0.0000000001. Using Python 3.11 in Google Colab."
```

The strong prompt tells the AI:
- What you're trying to accomplish
- What's actually happening
- What you expected
- Your environment

### Iterative Refinement

You rarely get perfect code on the first try. Think of it as a conversation:

```python
# First prompt:
# "Write a function to validate email addresses"

# AI gives you something basic with .count("@") == 1

# Second prompt:
# "This doesn't handle cases like 'user@domain' with no TLD. Can you make 
# it more robust?"

# AI adds checks for dots after the @

# Third prompt:
# "Good, but can you explain what each condition checks for? I need to 
# understand this for my exam."

# AI adds explanatory comments

# Fourth prompt:
# "Perfect. Now rewrite using camelCase variables to match my course style."
```

### Asking for Alternatives

AI is great at giving you several options to choose from. In most contexts, you want to make a justified choice, don't settle for the first solution:

```python
# Prompt:
# "I need to remove duplicates from a list while preserving order. 
# Show me three different approaches with pros/cons of each."

# You might get:
# 1. Using dict.fromkeys() - fast, clean, Python 3.7+
# 2. Using a loop with seen set - explicit, educational
# 3. Using collections.OrderedDict - works in older Python

# Now you can choose based on your constraints
```

## 4. Real Limitations in Late 2025

### What AI Still Gets Wrong

**Version Confusion**
AI models are trained on code from multiple time periods. They might suggest:
- Deprecated functions that no longer exist
- Old syntax that's been replaced
- Features that don't exist yet in the version you're using

```python
# AI might suggest for Python 3.8:
match status:  # This doesn't exist until Python 3.10!
    case 200:
        print("OK")

# Or for older numpy:
import numpy as np
result = np.std(data, ddof=1)  # ddof parameter naming changed across versions
```

Always be prepared rto check the documentation for your specific library versions.

**Library Hallucinations**
AI frequently invents functions that sound plausible but don't exist:

```python
# AI might suggest:
import requests
response = requests.get(url)
data = response.to_dict()  # No such method!

# The real method:
data = response.json()
```

**[Cargo Cult Imports](https://en.wikipedia.org/wiki/Cargo_cult)**
AI often includes unnecessary imports because they commonly appear together in training data:

```python
# AI might give you:
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

def addNumbers(a, b):
    return a + b  # Doesn't use any of these imports!
```

### Context Window Limitations

Even with large context windows, AI struggles with:
- Very large codebases (thousands of lines)
- Complex state that evolves across many functions
- Subtle interactions between distant parts of code

For big projects, you need to break problems into smaller pieces.

### Domain-Specific Knowledge

AI is trained on public code, which means:
- Strong: Web development, data science, common algorithms
- Weak: Specialized scientific computing, proprietary APIs, cutting-edge libraries
- Variable: Course-specific requirements, local conventions, custom tools

### The Confidence Problem

AI always sounds confident, even when wrong. It doesn't say "I'm not sure" or "This might not work." You need to be the skeptic.

```python
# AI might confidently tell you:
# "To connect to a quantum computer, use the quantum.connect() function"
# (There is no standard quantum.connect() function - this is made up)
```

## 5. Verification Strategies

### The Three-Step Check

**Step 1: Read and Understand**
Before deploying **any** code-- AI-generater or otherwise--, trace through it mentally (or on paper):
```python
# AI gives you:
def isPrime(n):
    if n < 2:
        return False
    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return False
    return True

# Trace with n=9:
# - n=9, not less than 2, continue
# - range(2, 4) gives [2, 3]
# - i=2: 9 % 2 = 1, not 0, continue
# - i=3: 9 % 3 = 0, return False
# Correct! 9 is not prime
```

**Step 2: Test Edge Cases**
AI typically tests the happy path. You need to ask it to test the edges:

```python
# For any function, test:
testCases = [
    None,           # If applicable
    [],             # Empty container
    [1],            # Single element
    [1, 1, 1],      # All same
    [-1, 0, 1],     # Mix of negative, zero, positive
]
```

**Step 3: Cross-Reference**
Check AI suggestions against:
- Official documentation
- Stack Overflow (sorted by votes)
- Your course materials
- Other AI engines

### Systematic Testing

AI is incredible for testing, as it takes all the drudgery out of setting up the testing harnesses and creating the boilerplate code for the numerous tests required.

```python
# Create a test harness for any AI-generated function:
def testFunction(functionToTest, testCases, expectedResults):
    """
    Test a function with multiple cases.
    
    functionToTest: The function to test
    testCases: List of input tuples (args, kwargs)
    expectedResults: List of expected outputs
    """
    passed = 0
    failed = 0
    
    for i, (args, kwargs) in enumerate(testCases):
        expected = expectedResults[i]
        try:
            result = functionToTest(*args, **kwargs)
            if result == expected:
                print(f"Test {i+1}: PASS")
                passed = passed + 1
            else:
                print(f"Test {i+1}: FAIL - Expected {expected}, got {result}")
                failed = failed + 1
        except Exception as error:
            print(f"Test {i+1}: ERROR - {error}")
            failed = failed + 1
    
    print(f"\n{passed} passed, {failed} failed")

# Example usage:
testCases = [
    (([],), {}),           # Empty list
    (([1, 2, 3],), {}),    # Normal case
    (([1],), {}),          # Single element
]
expected = [0, 2, 1]

testFunction(getAverage, testCases, expected)
```

## 6. Debugging AI-Generated Code

### When AI Code Fails

AI code fails in predictable ways. Here's how to fix it:

**Pattern 1: Missing Error Handling**
```python
# AI gives you:
def readConfig(filename):
    file = open(filename, 'r')
    data = file.read()
    file.close()
    return data

# This fails if file doesn't exist. Add handling:
def readConfig(filename):
    try:
        file = open(filename, 'r')
        data = file.read()
        file.close()
        return data
    except FileNotFoundError:
        print(f"Config file {filename} not found")
        return None
```

**Pattern 2: Off-by-One Errors**
```python
# AI might give you:
def getLastNItems(items, n):
    return items[-n:]

# Test it:
print(getLastNItems([1, 2, 3, 4, 5], 3))  # [3, 4, 5] - correct
print(getLastNItems([1, 2, 3], 5))        # [1, 2, 3] - unexpected!
print(getLastNItems([], 3))               # [] - might want error instead
```

**Pattern 3: Type Assumptions**
```python
# AI assumes strings but gets numbers:
def processInput(data):
    return data.upper()  # Fails if data is not a string!

# Make it robust:
def processInput(data):
    if isinstance(data, str) == True:
        return data.upper()
    return str(data).upper()  # Convert to string first
```

### Debugging Workflow

1. **Isolate the Problem**
```python
# AI gives you a complex function that's failing
# Break it into pieces:

# Original AI code:
def analyzeData(data):
    cleaned = [x for x in data if x > 0]
    squared = [x**2 for x in cleaned]
    return sum(squared) / len(squared)

# Debug by testing each step:
data = [-1, 0, 1, 2, 3]
cleaned = [x for x in data if x > 0]
print(f"Cleaned: {cleaned}")  # Check this works

squared = [x**2 for x in cleaned]
print(f"Squared: {squared}")  # Check this works

result = sum(squared) / len(squared)
print(f"Result: {result}")    # Check this works
```

2. **Add Instrumentation**
```python
# Add print statements at key points:
def mysteriousFunction(numbers):
    result = []
    for i in range(len(numbers)):
        print(f"Processing index {i}, value {numbers[i]}")  # Debug
        if numbers[i] % 2 == 0:
            result.append(numbers[i] * 2)
            print(f"  Appended {numbers[i] * 2}")  # Debug
    print(f"Final result: {result}")  # Debug
    return result
```

3. **Simplify and Rebuild**
```python
# If AI code is too complex to debug, rebuild simpler:
# Start with the simplest version that works:
def processData(data):
    return data  # Does nothing but compiles

# Add one feature at a time:
def processData(data):
    filtered = []
    for item in data:
        if item > 0:
            filtered.append(item)
    return filtered  # Works for positive filtering

# Continue building...
```

## 7. Integrating AI Into Your Workflow

### The Development Cycle with AI

**Phase 1: Understanding**
- Read the problem specification
- Identify inputs, outputs, constraints
- Decide on general approach
- AI role: Generate initial structure, suggest algorithms, create function skeletons

**Phase 2: Implementation**
- Have AI generate the basic structure and boilerplate
- Review and understand what it created
- Identify gaps or issues in the generated code
- AI role: Write the code, you make the decisions

**Phase 3: Testing**
- Have AI generate comprehensive test cases
- Add edge cases specific to your problem that AI might miss
- Run tests and use AI to help debug failures
- AI role: Create test harness and common test cases

**Phase 4: Optimization**
- Profile your code to find bottlenecks. Ask AI to provide the profiler setup. 
- Ask multiple AIs for more efficient algorithms
- Compare approaches and understand trade-offs
- AI role: Suggest optimizations, explain complexity, benchmark alternatives

### Practical Workflows

**Workflow 1: Syntax Helper**
```python
# You know what you want but forget the syntax:
# "How do I sort a list of dictionaries by a specific key in Python?"

# AI reminds you:
students = [{'name': 'Alice', 'grade': 85}, {'name': 'Bob', 'grade': 92}]
sorted_students = sorted(students, key=lambda x: x['grade'])
```

**Workflow 2: Error Interpreter**
```python
# You get an error you don't understand:
# "I'm getting 'TypeError: unsupported operand type(s) for +: 'int' and 'str'' 
# on line 15. Here's my code: ..."

# AI explains: You're trying to add a number and a string
# Then suggests: Convert the string to int first: int(myVariable)
```

**Workflow 3: Alternative Approaches**
```python
# You have working code but want to explore alternatives:
# "I wrote this function using nested loops. Are there other approaches 
# that might be faster or cleaner?"

# AI shows you:
# - List comprehension version
# - NumPy vectorized version  
# - itertools version
# You pick based on your constraints
```

**Workflow 4: Code Review**
```python
# You've written code and want a second opinion:
# "Can you review this function for potential bugs or improvements?"

# AI might catch:
# - Missing edge case handling
# - Variable naming inconsistencies
# - Opportunities to reduce complexity
# - Missing docstrings
```

## 8. Academic Integrity in the AI Era

### The Reality of AI in Education

By late 2025, trying to ban AI in programming courses is like banning calculators in math classes. It's not realistic and doesn't prepare you for actual work. However, there's a difference between using AI as a tool and using it to bypass learning.

### Course Expectations

**Encouraged Uses:**
- Explaining error messages and stack traces
- Suggesting multiple approaches to a problem you're working on
- Helping debug code you've written
- Converting between languages (Python to MATLAB)
- Generating test cases
- Explaining concepts from pre-reads or lectures

**Discouraged Uses:**
- Pasting entire homework problems and submitting the output
- Using AI during assessments without explicit permission
- Not understanding code before you submit it
- Relying on AI when you should be building fundamental skills

### The Understanding Test

If you're using AI for homework, ask yourself:
- Can I explain every line of this code to someone else?
- Could I recreate this solution without AI?
- Do I understand why this approach works?
- Could I debug this if it broke?

If you answer "no" to these questions, you're using AI wrong.

### Future-Proofing Your Skills

The job market in 2026 will expect you to:
- Use AI tools effectively
- Verify AI output
- Understand underlying concepts
- Debug and maintain AI-generated code
- Know when AI is leading you astray

You can't develop these skills by blindly accepting AI output.

## 9. Real-World Scenarios

### Scenario 1: RASTIC upgrade (Dec 5th, 2PM)

**Problem**: Students needed a way to remotely access RASTIC's robotic hardware. This had to be secure, and needed to be accessibly over the web. It would use [NanoMq](https://nanomq.io/), an MQTT broker.

**Single AI Approach**: Let's burn through some Claude tokens!

```
Make a python server which offers an HTTP API to publish a message to NanoMq. Python pushes from HTTP to MQTT over a websocket. It uses an API key to validate if a client is authorized.
```

Then upon examining the code and realizing it was not resilient to network hiccups, we needed a follow-up prompt:
```
1\) The server should infinitely try to establish a first connection
2\) The server should reconnect if it loses its connection.
3\) It should also restart from scratch if its connection is ever down for more than 1 minute.
```

The result was a full implementation which worked on the first try. We had no experience creating our own API or authentication methods. 

#### Technical debt:

We don't know much about security, and we performed a cardinal sin in COMSEC (computer security) which is rolling our own solution. We could not move to production with this code, but it allowed us to create a blazingly rapid PoC (Proof of Concept) with trivial effort.


### Scenario 2: API Integration

**Problem**: You need to fetch weather data from an API and cache it to avoid rate limits.

**Single AI Approach**: Ask ChatGPT, get basic code with requests library.

**Multi-AI Approach**:
```python
# ChatGPT suggests: Basic requests with timeout
import requests

def getWeather(city):
    url = f"https://api.weather.com/v1/weather?city={city}"
    response = requests.get(url, timeout=5)
    return response.json()

# Claude suggests: Adding caching
import requests
import time

cache = {}
cacheTimeout = 300  # 5 minutes

def getWeather(city):
    currentTime = time.time()
    if city in cache:
        cachedTime, cachedData = cache[city]
        if currentTime - cachedTime < cacheTimeout:
            return cachedData
    
    url = f"https://api.weather.com/v1/weather?city={city}"
    response = requests.get(url, timeout=5)
    data = response.json()
    cache[city] = (currentTime, data)
    return data

# Copilot suggests: Using requests_cache library
import requests_cache

requests_cache.install_cache('weather_cache', expire_after=300)

def getWeather(city):
    url = f"https://api.weather.com/v1/weather?city={city}"
    response = requests.get(url, timeout=5)
    return response.json()

# You choose Claude's approach because you understand it and don't need
# an external library, but you note the library for future reference
```

### Scenario 3: Data Processing

**Problem**: Process a CSV file with inconsistent formatting.

```python
# You start with a rough structure:
def processCSV(filename):
    # Read file
    # Clean data
    # Calculate statistics
    pass

# Ask AI: "I need to read a CSV where some lines have extra commas and 
# some values are missing. How should I handle this robustly?"

# AI suggests error handling and pandas:
import pandas as pd

def processCSV(filename):
    try:
        # Read with error handling
        data = pd.read_csv(filename, on_bad_lines='skip')
        
        # Remove rows with any missing values
        cleanData = data.dropna()
        
        # Calculate statistics
        stats = {
            'mean': cleanData.mean(),
            'count': len(cleanData),
            'missing': len(data) - len(cleanData)
        }
        return stats
    except FileNotFoundError:
        print(f"File {filename} not found")
        return None

# You test with your actual data and discover you need more specific
# handling, so you iterate with AI
```

### Scenario 4: Algorithm Optimization

**Problem**: Your nested loop is too slow for large datasets.

```python
# Your original code:
def findCommonElements(list1, list2):
    common = []
    for item1 in list1:
        for item2 in list2:
            if item1 == item2:
                if item1 not in common:  # Avoid duplicates
                    common.append(item1)
    return common

# This is O(n*m*k) - very slow!

# Ask AI: "This function works but is slow for large lists. Can you 
# explain why and suggest a faster approach?"

# AI explains the time complexity and suggests:
def findCommonElements(list1, list2):
    set1 = set(list1)
    set2 = set(list2)
    return list(set1.intersection(set2))

# O(n+m) - much faster!

# You understand: converting to sets eliminates the nested loop
# and set operations are fast
```

### Scenario 4: Debugging Complex Logic

**Problem**: Function sometimes returns wrong results, but you can't figure out why.

```python
# Your code:
def calculateDiscount(price, quantity, customerType):
    discount = 0
    if quantity > 10:
        discount = 0.1
    if quantity > 50:
        discount = 0.2
    if customerType == "premium":
        discount = discount + 0.15
    return price * quantity * (1 - discount)

# It works most of the time but gives weird results sometimes

# Ask AI: "Walk through this function with inputs: price=100, quantity=60, 
# customerType='premium'. What discount should be applied?"

# AI traces through:
# quantity > 10: discount = 0.1
# quantity > 50: discount = 0.2 (overwrites 0.1)
# customerType == "premium": discount = 0.2 + 0.15 = 0.35
# 
# But AI points out: "The premium discount should be based on the 
# quantity discount, not added to it. Also, the if statements overwrite
# each other instead of using elif."

# Fixed version:
def calculateDiscount(price, quantity, customerType):
    if quantity > 50:
        discount = 0.2
    elif quantity > 10:
        discount = 0.1
    else:
        discount = 0
    
    if customerType == "premium":
        discount = discount * 1.5  # Multiply, don't add
    
    return price * quantity * (1 - discount)
```

## 10. Programming Skills in 2026

### What's Changing

By 2026, the programming landscape will look different:

**What AI Will Handle Better:**
- Boilerplate code generation
- Common algorithms and patterns
- Syntax conversion between languages
- Initial test case generation
- Documentation writing
- Basic refactoring

**What Humans Still Need:**
- Problem decomposition and architecture
- Requirement analysis and clarification
- Performance optimization decisions
- Security considerations
- Code review and quality assessment
- Domain expertise integration
- Debugging complex interactions

### Skills That Matter More

**1. System Thinking**
Understanding how components interact is more valuable than memorizing syntax. AI can generate individual functions, but you need to design the system.

```python
# AI can write this function:
def validateUser(username, password):
    # validation logic
    pass

# But you need to decide:
# - Where does this function live in your architecture?
# - How does it interact with the database?
# - What happens on validation failure?
# - How do we handle rate limiting?
# - What security measures are needed?
```

**2. Prompt Engineering**
Getting good results from AI is a skill. The difference between a vague prompt and a precise one is the difference between working code and garbage.

**3. Critical Evaluation**
You need to quickly assess:
- Is this code correct?
- Is it efficient enough?
- Is it maintainable?
- Does it handle edge cases?
- Are there security issues?

**4. Integration Skills**
Combining AI-generated components, handling the connections, managing state, dealing with failures - these require human judgment.

### The Hybrid Workflow

Future programming looks like:
1. Human: Understand problem and design approach
2. AI: Generate initial implementation
3. Human: Review, test, and refine
4. AI: Suggest optimizations
5. Human: Make final decisions and integrate
6. Both: Iterate until complete

### What to Practice Now

**Build Mental Models**
Don't just copy AI code. Understand:
- How do dictionaries actually work?
- What happens in memory when you append to a list?
- Why are some operations slow and others fast?

**Practice Without AI**
Regularly solve problems without AI assistance:
- It builds problem-solving skills
- You learn to recognize patterns
- You develop debugging intuition
- You understand what's hard and what's easy

**Learn to Learn**
AI tools and languages will keep changing. The skill of quickly learning new tools and concepts matters more than memorizing current ones.

### Career Implications

Jobs in 2026 will expect you to:
- Use AI tools fluently
- Produce code faster than previous generations
- Handle more complex systems
- Take responsibility for AI-generated code quality
- Explain and defend technical decisions

The programmers who succeed will be those who use AI to amplify their capabilities, not replace their thinking.
