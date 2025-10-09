# Week 7A Preread: Dictionaries and Intro to Pandas

A **Python dictionary** is a built-in data structure that stores **key–value pairs** inside curly braces `{}`, using a colon to separate each key from its value. Dictionaries allow fast lookup, insertion, and deletion by key.

**Pandas** is a data manipulation library built on NumPy. Its core data structures are the one-dimensional `Series` and two-dimensional `DataFrame`, both of which often use dictionaries for construction.

Morning GPP will focus on **dictionaries**. Pandas is optional preread, but will be covered in lecture and homework.

---

## Dictionaries

Unlike lists, **dictionaries are mappings**, not sequences: values are accessed by **keys** (often strings), not by integer positions. The type is `dict`.

### Creating and Accessing a Dictionary

Below we create a dictionary that stores information about a hurricane. Each **key** is a string (`'name'`, `'year'`, `'category'`), and each **value** stores associated data. You access values by **key** (not by integer index).

    bigone = {'name': 'Bertha', 'year': 1952, 'category': 4}

    bigone['year']
    # 1952

Trying to access a key that doesn’t exist causes an error. This is different from lists, where `x[1]` accesses the second element; for dictionaries you must use a **key**.

    bigone['size']
    # KeyError: 'size'

---

### Modifying and Extending Dictionaries

Dictionaries are **mutable**, meaning they can be changed after creation. You can update values, add new pairs, or delete existing ones. The pattern is always `dictionary[key] = value` for setting, and `del dictionary[key]` for deleting.

    bigone['category'] = 5        # change existing value
    bigone['eyewidth'] = 400      # add new key–value pair
    del bigone['eyewidth']        # delete a key–value pair
    len(bigone)                   # number of key–value pairs → 3

---

### Safe Access with `get`

Using square brackets on a missing key raises an error. The `.get()` method avoids this by returning `None` (or a default you supply). This lets your code handle missing keys gracefully instead of crashing.

    bigone.get('size')          # None (key not present)
    bigone.get('size', -999)    # -999 (useful default/flag value)

---

### Checking for Keys and Viewing Contents

Use membership tests to see if a key exists, and `list()` or the view methods to inspect keys/values.

    'year' in bigone            # True
    list(bigone)                # ['name', 'year', 'category']

To loop, use `.keys()`, `.values()`, or `.items()` depending on whether you need keys, values, or both. This is the standard way to traverse a dictionary.

    for k in bigone.keys():
        print('Key:', k)
    # Key: name
    # Key: year
    # Key: category

    for k, v in bigone.items():
        print(f"The value of {k!r} is {v}")
    # The value of 'name' is Bertha
    # The value of 'year' is 1952
    # The value of 'category' is 5

---

### Deleting with `pop`, `popitem`, and `clear`

- `.pop(key)` removes a specific key and returns its value.
- `.popitem()` removes the **last inserted** key–value pair and returns `(key, value)`.
- `.clear()` removes everything.

    val = bigone.pop('category')    # returns 5 and removes it
    # bigone now {'name': 'Bertha', 'year': 1952}

    k, v = bigone.popitem()         # removes the last remaining key–value
    # (k, v) might be ('year', 1952)

    bigone.clear()                  # {}
    # dictionary is now empty

---

### Nested Structures

Values in a dictionary can be lists or other dictionaries. Here we store multiple toppings in a list under the key `'addins'`. Because the value is a list, you can index it or loop over it.

    icdessert = {
        'flavor': 'malted',
        'cone': True,
        'nscoops': 2,
        'addins': ['pecans', 'chocolate chips']
    }

    icdessert['addins'][0]
    # 'pecans'

    print('Your ice cream add-ins are:')
    for topping in icdessert['addins']:
        print(topping.title(), '!!!')
    # Your ice cream add-ins are:
    # Pecans !!!
    # Chocolate Chips !!!

---

### Dictionary Comprehensions

Dictionary comprehensions build key–value pairs programmatically in a compact way. Use them when the mapping follows a clear rule.

    cubedict = {i: i**3 for i in range(5)}
    # {0: 0, 1: 1, 2: 8, 3: 27, 4: 64}

---

### Lists of Dictionaries

To store multiple “records” (e.g., hurricanes), use a list of dictionaries. Each dictionary acts like one row of data. This mirrors how we often process data: **list → iterate → inspect each dictionary**.

    hurricanes = [
        {'name': 'Bertha', 'year': 1952, 'category': 4},
        {'name': 'Bob',    'year': 1990, 'category': 2},
        {'name': 'Camilla','year': 1960, 'category': 5}
    ]

    print('The largest hurricanes were:')
    for h in hurricanes:
        if h['category'] >= 4:
            print(h['name'])
    # The largest hurricanes were:
    # Bertha
    # Camilla

---

## Introduction to Pandas

Pandas builds on NumPy and is typically imported alongside it. Pandas provides labeled, table-like structures and high-level operations for analysis.

    import numpy as np
    import pandas as pd

Pandas introduces two core data structures:
- **Series**: one-dimensional, like a labeled array.
- **DataFrame**: two-dimensional, like a labeled spreadsheet.

---

### Series

Create a Series from a list; Pandas auto-creates an integer index. You can index and slice as in Python, but you also have labels.

    numseries = pd.Series([11, 33, 15, 2])
    numseries
    # 0    11
    # 1    33
    # 2    15
    # 3     2
    # dtype: int64

    numseries[1]
    # 33

    numseries[1:3]
    # 1    33
    # 2    15
    # dtype: int64

Assign custom labels to make the Series more meaningful. Label-based slices are inclusive of the end label.

    numlabels = pd.Series([11, 33, 15, 2], index=['a','b','c','d'])
    numlabels['b':'d']
    # b    33
    # c    15
    # d     2
    # dtype: int64

You can also create a Series directly from a dictionary (keys become labels).

    numdict = {'a':11, 'b':33, 'c':15, 'd':2}
    numlab = pd.Series(numdict)
    numlab
    # a    11
    # b    33
    # c    15
    # d     2
    # dtype: int64

---

### DataFrames

A `DataFrame` is a 2D labeled table. Here we construct it from two Series made from dictionaries: IDs and Exam 1 scores. The **row labels** are student names; the **column labels** are `'ID'` and `'Exam1'`.

    ids = pd.Series({'Joey':123, 'Javier':234, 'Juanita':456, 'Jane':678})
    exam1s = pd.Series({'Joey':95, 'Javier':99, 'Juanita':88, 'Jane':100})
    students = pd.DataFrame({'ID': ids, 'Exam1': exam1s})
    students
    #          ID  Exam1
    # Joey    123     95
    # Javier  234     99
    # Juanita 456     88
    # Jane    678    100

Key attributes and indexing patterns:

    students.index        # Index(['Joey','Javier','Juanita','Jane'], dtype='object')
    students.columns      # Index(['ID','Exam1'], dtype='object')
    students.values       # underlying 2D NumPy array

Access a column (returns a Series), then a single value by label or integer:

    students['ID']              # or students.ID
    students['ID']['Jane']      # 678
    # Note: chained indexing works here but .loc/.iloc are generally safer

Access whole rows with `.loc` (label) or `.iloc` (integer position):

    students.loc['Juanita']
    # ID       456
    # Exam1     88
    # Name: Juanita, dtype: int64

    students.iloc[2]
    # ID       456
    # Exam1     88
    # Name: Juanita, dtype: int64

---

### Modifying and Summarizing DataFrames

Add a new column by assignment; drop a column with `.drop(columns=...)`. Many summary stats are one-liners.

    students['Exam2'] = [100, 99, 98, 97]
    students
    #          ID  Exam1  Exam2
    # Joey    123     95    100
    # Javier  234     99     99
    # Juanita 456     88     98
    # Jane    678    100     97

    students = students.drop(columns='ID')
    students
    #          Exam1  Exam2
    # Joey        95    100
    # Javier      99     99
    # Juanita     88     98
    # Jane       100     97

    students.mean()
    # Exam1    95.5
    # Exam2    98.5
    # dtype: float64

    students.shape     # (4, 2)
    students.head()    # first 5 rows (useful for large tables)
    students.tail()    # last 5 rows

---

## Summary

- **Dictionaries**: Key–value mappings; mutable; great for structured records and flexible manipulation. Use `.get()` for safe access; `.items()` for looping; `.pop()`/`.popitem()`/`.clear()` for deletions.
- **Lists of dictionaries**: Model multiple records; iterate and filter easily.
- **Pandas**: `Series` (1D) and `DataFrame` (2D) provide labeled, tabular structures for analysis; they integrate naturally with dictionaries and NumPy.