# Python Fundamentals for Data Science - Tutorial 1

## Introduction

Python is one of the most popular programming languages for data science due to its simplicity, readability, and powerful libraries. This tutorial covers the fundamental concepts you need to master before diving into data analysis and machine learning.

## Table of Contents
1. [Numbers and Arithmetic](#numbers-and-arithmetic)
2. [Variables](#variables)
3. [Strings](#strings)
4. [Lists](#lists)
5. [Dictionaries](#dictionaries)
6. [Booleans](#booleans)
7. [Tuples](#tuples)
8. [Sets](#sets)
9. [Comparison Operators](#comparison-operators)
10. [Logical Operators](#logical-operators)
11. [Control Flow (if/elif/else)](#control-flow)
12. [Loops](#loops)
13. [Functions](#functions)
14. [Lambda Expressions](#lambda-expressions)
15. [Map and Filter](#map-and-filter)
16. [Methods](#methods)

---

## Numbers and Arithmetic

Python has two main numeric types:

### Integer and Float
```python
# Integer - whole numbers
age = 25
count = 100

# Float - decimal numbers
temperature = 98.6
price = 19.99
```

### Basic Arithmetic Operations
```python
# Addition
result = 5 + 3  # 8

# Subtraction
result = 10 - 4  # 6

# Multiplication
result = 6 * 7  # 42

# Division (always returns float)
result = 10 / 3  # 3.333...

# Exponentiation
result = 2 ** 4  # 16

# Modulo (remainder)
result = 17 % 5  # 2
```

### Order of Operations
Python follows standard mathematical order of operations (PEMDAS):

```python
# Without parentheses
result = 2 + 3 * 5  # 17 (multiplication first, then addition)

# With parentheses to change order
result = (2 + 3) * 5  # 25 (addition first, then multiplication)
```

### Practical Use of Modulo
The modulo operator is particularly useful for checking if numbers are even or odd:

```python
# Check if number is even
number = 8
if number % 2 == 0:
    print("Even number")
else:
    print("Odd number")
```

---

## Variables

Variables are containers that store data values. Think of them as labeled boxes where you can store information.

### Variable Assignment
```python
# Basic assignment
x = 10
name = "Alice"
pi = 3.14159

# Multiple assignments
a = b = c = 100  # All three variables equal 100
x, y, z = 1, 2, 3  # x=1, y=2, z=3
```

### Variable Reassignment
```python
x = 5
print(x)  # 5

x = x + 3  # Add 3 to current value of x
print(x)  # 8

# Shorthand operators
x += 5  # Same as x = x + 5
x -= 2  # Same as x = x - 2
x *= 3  # Same as x = x * 3
```

### Variable Naming Rules
```python
# Valid variable names
user_name = "John"
age1 = 25
_private = "secret"
firstName = "Jane"  # CamelCase (less common in Python)

# Invalid variable names (will cause errors)
# 2user = "invalid"    # Can't start with number
# user-name = "invalid"  # Can't use hyphens
# $money = 100         # Can't start with special characters
```

**Best Practices:**
- Use descriptive names: `student_count` instead of `sc`
- Use lowercase with underscores: `first_name` instead of `firstName`
- Avoid Python keywords: `class`, `def`, `if`, etc.

---

## Strings

Strings are sequences of characters enclosed in quotes. They're used to represent text data.

### Creating Strings
```python
# Single quotes
message = 'Hello, World!'

# Double quotes
greeting = "Welcome to Python!"

# Mixing quotes (useful for quotes within strings)
quote = "She said, 'Python is amazing!'"
sentence = 'He replied, "I totally agree!"'

# Multi-line strings
long_text = """
This is a long string
that spans multiple lines.
Very useful for documentation.
"""
```

### String Operations
```python
# String concatenation
first_name = "John"
last_name = "Doe"
full_name = first_name + " " + last_name  # "John Doe"

# String repetition
laugh = "Ha" * 3  # "HaHaHa"

# String length
name = "Python"
length = len(name)  # 6
```

### String Formatting
String formatting allows you to insert variables into strings dynamically:

#### Method 1: .format()
```python
name = "Alice"
age = 30
score = 95.5

# Basic formatting
message = "My name is {} and I am {} years old".format(name, age)
print(message)  # "My name is Alice and I am 30 years old"

# Numbered placeholders
message = "Score: {1}, Name: {0}".format(name, score)
print(message)  # "Score: 95.5, Name: Alice"

# Named placeholders
message = "Hello {student}, your grade is {grade}".format(student=name, grade=score)
```

#### Method 2: f-strings (Python 3.6+) - Recommended
```python
name = "Bob"
age = 25

# f-string formatting (cleaner and more readable)
message = f"My name is {name} and I am {age} years old"
print(message)

# With expressions
message = f"Next year I'll be {age + 1} years old"
```

### String Indexing and Slicing
Strings are sequences, so you can access individual characters:

```python
text = "Python"

# Indexing (starts at 0)
first_char = text[0]    # 'P'
last_char = text[-1]    # 'n' (negative indexing from end)

# Slicing
substring = text[1:4]   # 'yth' (characters 1, 2, 3)
beginning = text[:3]    # 'Pyt' (from start to index 3)
ending = text[3:]       # 'hon' (from index 3 to end)
every_second = text[::2]  # 'Pto' (every second character)
```

---

## Lists

Lists are ordered collections that can store multiple items. They're one of the most versatile data types in Python.

### Creating and Modifying Lists
```python
# Creating lists
numbers = [1, 2, 3, 4, 5]
names = ["Alice", "Bob", "Charlie"]
mixed = [1, "hello", 3.14, True]  # Lists can contain different data types
empty_list = []

# Adding elements
numbers.append(6)        # Add to end: [1, 2, 3, 4, 5, 6]
numbers.insert(0, 0)     # Insert at index 0: [0, 1, 2, 3, 4, 5, 6]

# Removing elements
numbers.remove(3)        # Remove first occurrence of 3
popped = numbers.pop()   # Remove and return last element
popped = numbers.pop(0)  # Remove and return element at index 0
```

### List Indexing and Slicing
```python
fruits = ["apple", "banana", "cherry", "date", "elderberry"]

# Indexing
first_fruit = fruits[0]     # "apple"
last_fruit = fruits[-1]     # "elderberry"

# Slicing
first_three = fruits[:3]    # ["apple", "banana", "cherry"]
last_two = fruits[-2:]      # ["date", "elderberry"]
middle = fruits[1:4]        # ["banana", "cherry", "date"]

# Modifying elements
fruits[0] = "apricot"       # Change first element
print(fruits)  # ["apricot", "banana", "cherry", "date", "elderberry"]
```

### Nested Lists
Lists can contain other lists, creating multi-dimensional structures:

```python
# 2D list (like a table)
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

# Accessing nested elements
first_row = matrix[0]       # [1, 2, 3]
center_element = matrix[1][1]  # 5

# Practical example: student grades
students = [
    ["Alice", 85, 92, 78],
    ["Bob", 90, 88, 95],
    ["Charlie", 87, 91, 89]
]

# Get Bob's second grade
bobs_second_grade = students[1][2]  # 88
```

---

## Dictionaries

Dictionaries store data as key-value pairs. Think of them like a real dictionary where you look up a word (key) to find its definition (value).

### Creating and Using Dictionaries
```python
# Creating dictionaries
student = {"name": "Alice", "age": 20, "grade": "A"}
empty_dict = {}

# Accessing values
student_name = student["name"]  # "Alice"
student_age = student.get("age")  # 20 (safer method)

# Adding/updating values
student["email"] = "alice@email.com"  # Add new key-value pair
student["age"] = 21  # Update existing value

print(student)
# {'name': 'Alice', 'age': 21, 'grade': 'A', 'email': 'alice@email.com'}
```

### Nested Dictionaries
```python
# Complex nested structure
company = {
    "name": "TechCorp",
    "employees": {
        "engineering": ["Alice", "Bob"],
        "marketing": ["Charlie", "Diana"]
    },
    "revenue": 1000000
}

# Accessing nested data
engineering_team = company["employees"]["engineering"]  # ["Alice", "Bob"]
first_engineer = company["employees"]["engineering"][0]  # "Alice"
```

### Dictionary Methods
```python
student = {"name": "Alice", "age": 20, "grade": "A"}

# Get all keys, values, or items
keys = student.keys()      # dict_keys(['name', 'age', 'grade'])
values = student.values()  # dict_values(['Alice', 20, 'A'])
items = student.items()    # dict_items([('name', 'Alice'), ('age', 20), ('grade', 'A')])

# Safe removal
removed_grade = student.pop("grade", "Not found")  # Returns and removes "A"
```

---

## Booleans

Booleans represent True or False values and are essential for decision-making in programs.

```python
# Boolean values
is_student = True
is_graduated = False

# Boolean from comparisons
age = 18
is_adult = age >= 18  # True

# Boolean in conditions
if is_student:
    print("Eligible for student discount")

# Truthy and Falsy values
# These are considered False: False, None, 0, 0.0, "", [], {}, set()
# Everything else is considered True

if [1, 2, 3]:  # Non-empty list is True
    print("List has items")

if not []:  # Empty list is False, so 'not []' is True
    print("List is empty")
```

---

## Tuples

Tuples are similar to lists but are immutable (cannot be changed after creation). They're used for data that shouldn't be modified.

```python
# Creating tuples
coordinates = (10, 20)
rgb_color = (255, 128, 0)
single_item = (42,)  # Note the comma for single-item tuples

# Accessing tuple elements
x = coordinates[0]  # 10
y = coordinates[1]  # 20

# Tuples are immutable
# coordinates[0] = 15  # This would cause an error!

# Useful for functions that return multiple values
def get_name_age():
    return "Alice", 25  # Actually returns a tuple

name, age = get_name_age()  # Tuple unpacking
```

### When to Use Tuples vs Lists
- **Use tuples for:** Fixed data like coordinates, RGB values, database records
- **Use lists for:** Data that might change, like shopping carts, todo lists

---

## Sets

Sets are collections of unique elements. They automatically remove duplicates.

```python
# Creating sets
unique_numbers = {1, 2, 3, 4, 5}
colors = {"red", "green", "blue"}

# Creating from lists (removes duplicates)
numbers_with_duplicates = [1, 2, 2, 3, 3, 3, 4]
unique_numbers = set(numbers_with_duplicates)  # {1, 2, 3, 4}

# Adding elements
colors.add("yellow")
colors.update(["purple", "orange"])  # Add multiple elements

# Set operations
set1 = {1, 2, 3, 4}
set2 = {3, 4, 5, 6}

intersection = set1 & set2  # {3, 4} - elements in both sets
union = set1 | set2         # {1, 2, 3, 4, 5, 6} - all elements
difference = set1 - set2    # {1, 2} - elements in set1 but not set2
```

---

## Comparison Operators

Comparison operators compare values and return boolean results.

```python
# Numeric comparisons
print(5 > 3)   # True
print(10 < 8)  # False
print(7 >= 7)  # True
print(4 <= 4)  # True
print(5 == 5)  # True (equality)
print(5 != 3)  # True (not equal)

# String comparisons
print("apple" < "banana")  # True (alphabetical order)
print("Python" == "python")  # False (case sensitive)

# Comparing different types
print(5 == "5")  # False (integer vs string)
print(1 == True)  # True (1 and True are equivalent)
print(0 == False)  # True (0 and False are equivalent)
```

---

## Logical Operators

Logical operators combine boolean expressions.

```python
age = 25
has_license = True
has_car = False

# AND operator - both conditions must be True
can_drive_legally = age >= 18 and has_license  # True

# OR operator - at least one condition must be True
can_travel = has_car or has_license  # True

# NOT operator - reverses the boolean value
cannot_drive = not has_license  # False

# Complex combinations
is_eligible = (age >= 21) and (has_license or has_car)
print(is_eligible)  # True

# Short-circuit evaluation
# With 'and': if first is False, second isn't evaluated
# With 'or': if first is True, second isn't evaluated
result = (5 > 10) and (print("This won't print"))  # print() not executed
```

---

## Control Flow

Control flow statements allow your program to make decisions and execute different code based on conditions.

### if/elif/else Statements
```python
temperature = 75

if temperature > 80:
    print("It's hot outside!")
elif temperature > 60:
    print("Nice weather!")
elif temperature > 40:
    print("A bit chilly")
else:
    print("It's cold!")

# Nested conditions
age = 20
has_id = True

if age >= 18:
    if has_id:
        print("You can enter the club")
    else:
        print("You need an ID")
else:
    print("You must be 18 or older")

# Ternary operator (one-line if-else)
status = "adult" if age >= 18 else "minor"
```

### Practical Examples
```python
# Grade calculator
score = 87

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
elif score >= 60:
    grade = "D"
else:
    grade = "F"

print(f"Your grade is: {grade}")

# Multiple conditions
username = "admin"
password = "secret123"
is_active = True

if username == "admin" and password == "secret123" and is_active:
    print("Login successful")
else:
    print("Login failed")
```

---

## Loops

Loops allow you to repeat code multiple times, which is essential for data processing.

### for Loops
For loops iterate over sequences (lists, strings, etc.) or ranges of numbers.

```python
# Looping through a list
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(f"I like {fruit}")

# Looping through a string
for letter in "Python":
    print(letter)

# Using range()
for i in range(5):          # 0, 1, 2, 3, 4
    print(f"Count: {i}")

for i in range(2, 8):       # 2, 3, 4, 5, 6, 7
    print(i)

for i in range(0, 10, 2):   # 0, 2, 4, 6, 8 (step by 2)
    print(i)

# Enumerate for index and value
names = ["Alice", "Bob", "Charlie"]
for index, name in enumerate(names):
    print(f"{index}: {name}")
```

### while Loops
While loops continue as long as a condition is True.

```python
# Basic while loop
count = 0
while count < 5:
    print(f"Count is: {count}")
    count += 1  # Important: update the condition variable!

# User input loop
user_input = ""
while user_input.lower() != "quit":
    user_input = input("Enter a command (or 'quit' to exit): ")
    if user_input.lower() != "quit":
        print(f"You entered: {user_input}")

# Be careful with infinite loops!
# while True:  # This runs forever
#     print("This will never stop!")
```

### Loop Control Statements
```python
# break - exit the loop immediately
for i in range(10):
    if i == 5:
        break
    print(i)  # Prints 0, 1, 2, 3, 4

# continue - skip the rest of the current iteration
for i in range(5):
    if i == 2:
        continue
    print(i)  # Prints 0, 1, 3, 4 (skips 2)

# Loop with else clause
for i in range(3):
    print(i)
else:
    print("Loop completed normally")  # Only runs if loop wasn't broken
```

### List Comprehensions
List comprehensions provide a concise way to create lists using loops.

```python
# Traditional approach
squares = []
for x in range(5):
    squares.append(x ** 2)
print(squares)  # [0, 1, 4, 9, 16]

# List comprehension (more Pythonic)
squares = [x ** 2 for x in range(5)]
print(squares)  # [0, 1, 4, 9, 16]

# With conditions
even_squares = [x ** 2 for x in range(10) if x % 2 == 0]
print(even_squares)  # [0, 4, 16, 36, 64]

# More complex example
words = ["hello", "world", "python"]
capitalized = [word.upper() for word in words if len(word) > 4]
print(capitalized)  # ['HELLO', 'WORLD', 'PYTHON']
```

---

## Functions

Functions are reusable blocks of code that perform specific tasks. They help organize code and avoid repetition.

### Defining Functions
```python
# Basic function
def greet():
    print("Hello, World!")

# Call the function
greet()  # Output: Hello, World!

# Function with parameters
def greet_person(name):
    print(f"Hello, {name}!")

greet_person("Alice")  # Output: Hello, Alice!

# Function with multiple parameters
def add_numbers(a, b):
    result = a + b
    return result  # Return the result

sum_result = add_numbers(5, 3)
print(sum_result)  # Output: 8
```

### Default Parameters
```python
def greet_with_title(name, title="Mr."):
    print(f"Hello, {title} {name}!")

greet_with_title("Smith")           # Hello, Mr. Smith!
greet_with_title("Johnson", "Dr.")  # Hello, Dr. Johnson!
```

### Keyword Arguments
```python
def create_profile(name, age, city="Unknown", occupation="Unemployed"):
    return f"{name}, {age} years old, lives in {city}, works as {occupation}"

# Positional arguments
profile1 = create_profile("Alice", 25, "New York", "Engineer")

# Keyword arguments (can be in any order)
profile2 = create_profile(age=30, name="Bob", occupation="Teacher")

# Mix of positional and keyword
profile3 = create_profile("Charlie", 35, occupation="Artist")

print(profile2)  # Bob, 30 years old, lives in Unknown, works as Teacher
```

### Variable-Length Arguments
```python
# *args for variable positional arguments
def sum_all(*numbers):
    total = 0
    for num in numbers:
        total += num
    return total

print(sum_all(1, 2, 3))        # 6
print(sum_all(1, 2, 3, 4, 5))  # 15

# **kwargs for variable keyword arguments
def print_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="Alice", age=25, city="Boston")
# name: Alice
# age: 25
# city: Boston
```

### Documentation Strings (Docstrings)
```python
def calculate_area(length, width):
    """
    Calculate the area of a rectangle.
    
    Args:
        length (float): The length of the rectangle
        width (float): The width of the rectangle
    
    Returns:
        float: The area of the rectangle
    """
    return length * width

# Access docstring
print(calculate_area.__doc__)

# In Jupyter notebooks, use Shift+Tab to see docstring
```

---

## Lambda Expressions

Lambda expressions (anonymous functions) are short, one-line functions useful for simple operations.

### Basic Lambda Syntax
```python
# Regular function
def square(x):
    return x ** 2

# Equivalent lambda function
square_lambda = lambda x: x ** 2

print(square(5))        # 25
print(square_lambda(5)) # 25

# Multiple parameters
multiply = lambda x, y: x * y
print(multiply(4, 3))   # 12

# Lambda with conditional
max_of_two = lambda a, b: a if a > b else b
print(max_of_two(10, 5))  # 10
```

### When to Use Lambda
```python
# Good use case: simple operations with map, filter, sort
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x ** 2, numbers))
print(squared)  # [1, 4, 9, 16, 25]

# Sorting with custom key
students = [("Alice", 85), ("Bob", 90), ("Charlie", 78)]
students.sort(key=lambda student: student[1])  # Sort by grade
print(students)  # [('Charlie', 78), ('Alice', 85), ('Bob', 90)]

# Don't use lambda for complex operations - use regular functions instead
```

---

## Map and Filter

Map and filter are built-in functions that work well with lambda expressions for data processing.

### Map Function
Map applies a function to every element in an iterable.

```python
# Using map with lambda
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x ** 2, numbers))
print(squared)  # [1, 4, 9, 16, 25]

# Using map with regular function
def celsius_to_fahrenheit(celsius):
    return (celsius * 9/5) + 32

celsius_temps = [0, 20, 30, 40]
fahrenheit_temps = list(map(celsius_to_fahrenheit, celsius_temps))
print(fahrenheit_temps)  # [32.0, 68.0, 86.0, 104.0]

# Map with multiple iterables
list1 = [1, 2, 3]
list2 = [4, 5, 6]
sums = list(map(lambda x, y: x + y, list1, list2))
print(sums)  # [5, 7, 9]
```

### Filter Function
Filter selects elements from an iterable based on a condition.

```python
# Filter even numbers
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
even_numbers = list(filter(lambda x: x % 2 == 0, numbers))
print(even_numbers)  # [2, 4, 6, 8, 10]

# Filter strings by length
words = ["python", "java", "c", "javascript", "go"]
long_words = list(filter(lambda word: len(word) > 4, words))
print(long_words)  # ['python', 'javascript']

# Filter dictionaries
students = [
    {"name": "Alice", "grade": 85},
    {"name": "Bob", "grade": 92},
    {"name": "Charlie", "grade": 78}
]
high_performers = list(filter(lambda student: student["grade"] >= 90, students))
print(high_performers)  # [{'name': 'Bob', 'grade': 92}]
```

---

## Methods

Methods are functions that belong to objects. Different data types have different methods available.

### String Methods
```python
text = "  Hello, World!  "

# Case methods
print(text.upper())     # "  HELLO, WORLD!  "
print(text.lower())     # "  hello, world!  "
print(text.title())     # "  Hello, World!  "
print(text.capitalize()) # "  hello, world!  "

# Whitespace methods
print(text.strip())     # "Hello, World!" (remove leading/trailing whitespace)
print(text.lstrip())    # "Hello, World!  " (remove leading whitespace)
print(text.rstrip())    # "  Hello, World!" (remove trailing whitespace)

# Split and join
sentence = "Python is awesome"
words = sentence.split()        # ["Python", "is", "awesome"]
words = sentence.split("s")     # ["Python i", " awe", "ome"]

# Join list back to string
rejoined = " ".join(words)
print(rejoined)

# Replace
new_text = sentence.replace("awesome", "great")
print(new_text)  # "Python is great"

# Check methods
print("Python" in sentence)      # True
print(sentence.startswith("Py")) # True
print(sentence.endswith("me"))   # True
print(sentence.isdigit())        # False
print("123".isdigit())           # True
```

### List Methods
```python
numbers = [3, 1, 4, 1, 5, 9, 2, 6]

# Adding elements
numbers.append(7)              # Add to end
numbers.insert(0, 0)           # Insert at specific position
numbers.extend([8, 9])         # Add multiple elements

# Removing elements
numbers.remove(1)              # Remove first occurrence
popped = numbers.pop()         # Remove and return last element
popped = numbers.pop(2)        # Remove and return element at index 2
numbers.clear()                # Remove all elements

# Utility methods
numbers = [3, 1, 4, 1, 5, 9, 2, 6]
print(numbers.count(1))        # 2 (count occurrences)
print(numbers.index(4))        # 2 (index of first occurrence)

# Sorting
numbers.sort()                 # Sort in place
print(numbers)                 # [1, 1, 2, 3, 4, 5, 6, 9]

numbers.sort(reverse=True)     # Sort descending
numbers.reverse()              # Reverse order

# Copy
new_list = numbers.copy()      # Shallow copy
```

### Dictionary Methods
```python
student = {"name": "Alice", "age": 20, "grade": "A"}

# Getting values safely
name = student.get("name")          # "Alice"
height = student.get("height", 0)   # 0 (default value if key doesn't exist)

# Keys, values, items
keys = list(student.keys())         # ["name", "age", "grade"]
values = list(student.values())     # ["Alice", 20, "A"]
items = list(student.items())       # [("name", "Alice"), ("age", 20), ("grade", "A")]

# Update dictionary
student.update({"email": "alice@email.com", "age": 21})
print(student)

# Remove items
removed_grade = student.pop("grade")      # Remove and return value
student.popitem()                         # Remove and return arbitrary item
del student["age"]                        # Delete specific key
```

---

## Practical Examples and Exercises

### Example 1: Student Grade Manager
```python
def calculate_grade(scores):
    """Calculate letter grade based on average score."""
    average = sum(scores) / len(scores)
    
    if average >= 90:
        return "A"
    elif average >= 80:
        return "B"
    elif average >= 70:
        return "C"
    elif average >= 60:
        return "D"
    else:
        return "F"

# Student database
students = {
    "Alice": [85, 92, 78, 96],
    "Bob": [90, 88, 92, 85],
    "Charlie": [76, 82, 79, 88]
}

# Calculate grades for all students
for name, scores in students.items():
    grade = calculate_grade(scores)
    average = sum(scores) / len(scores)
    print(f"{name}: Average = {average:.1f}, Grade = {grade}")
```

### Example 2: Text Analysis
```python
def analyze_text(text):
    """Analyze text and return statistics."""
    # Clean and split text
    words = text.lower().replace(",", "").replace(".", "").split()
    
    # Count words
    word_count = len(words)
    
    # Count unique words
    unique_words = len(set(words))
    
    # Find most common word
    word_frequency = {}
    for word in words:
        word_frequency[word] = word_frequency.get(word, 0) + 1
    
    most_common = max(word_frequency.items(), key=lambda x: x[1])
    
    return {
        "total_words": word_count,
        "unique_words": unique_words,
        "most_common_word": most_common[0],
        "most_common_count": most_common[1]
    }

# Test the function
sample_text = "Python is great. Python is powerful. Python is easy to learn."
results = analyze_text(sample_text)
print(results)
```

### Exercise Ideas

Try these exercises to practice the concepts you've learned:

1. **Calculator Function**: Create a function that takes two numbers and an operation (+, -, *, /) and returns the result.

2. **List Statistics**: Write a function that takes a list of numbers and returns the minimum, maximum, average, and count.

3. **Password Validator**: Create a function that checks if a password meets criteria (length, has numbers, has uppercase/lowercase).

4. **Shopping Cart**: Build a simple shopping cart using dictionaries to store items and quantities.

---

## Advanced Concepts Preview

### Tuple Unpacking
Tuple unpacking allows you to assign multiple variables at once:

```python
# Basic tuple unpacking
point = (10, 20)
x, y = point
print(f"x: {x}, y: {y}")  # x: 10, y: 20

# Useful in loops with lists of tuples
coordinates = [(0, 0), (1, 2), (3, 4)]
for x, y in coordinates:
    print(f"Point: ({x}, {y})")

# Swapping variables
a = 5
b = 10
a, b = b, a  # Now a = 10, b = 5

# Function returning multiple values
def get_stats(numbers):
    return min(numbers), max(numbers), sum(numbers)

minimum, maximum, total = get_stats([1, 2, 3, 4, 5])
```

### The `in` Operator
Check if an element exists in a collection:

```python
# Lists
fruits = ["apple", "banana", "cherry"]
print("apple" in fruits)        # True
print("orange" in fruits)       # False

# Strings
text = "Hello, World!"
print("World" in text)          # True
print("world" in text)          # False (case sensitive)

# Dictionaries (checks keys by default)
student = {"name": "Alice", "age": 20}
print("name" in student)        # True
print("Alice" in student)       # False (checking keys, not values)

# Sets
numbers = {1, 2, 3, 4, 5}
print(3 in numbers)             # True
```

---

## Common Pitfalls and Best Practices

### 1. Mutable Default Arguments
```python
# WRONG - Don't do this!
def add_item(item, my_list=[]):
    my_list.append(item)
    return my_list

# This causes problems because the list is shared between calls
list1 = add_item("apple")
list2 = add_item("banana")  # This will have both "apple" and "banana"!

# CORRECT - Do this instead:
def add_item(item, my_list=None):
    if my_list is None:
        my_list = []
    my_list.append(item)
    return my_list
```

### 2. Variable Scope
```python
# Global vs Local scope
global_var = "I'm global"

def my_function():
    local_var = "I'm local"
    print(global_var)  # Can access global variables
    print(local_var)   # Can access local variables

my_function()
print(global_var)      # Can access global variables
# print(local_var)     # ERROR! Can't access local variables outside function

# Modifying global variables
counter = 0

def increment():
    global counter  # Need 'global' keyword to modify global variables
    counter += 1

increment()
print(counter)  # 1
```

### 3. Copying Lists and Dictionaries
```python
# Shallow copy vs Deep copy
original_list = [1, 2, [3, 4]]

# Assignment creates a reference, not a copy
reference = original_list
reference.append(5)
print(original_list)  # [1, 2, [3, 4], 5] - original is changed!

# Shallow copy
shallow_copy = original_list.copy()  # or list(original_list) or original_list[:]
shallow_copy.append(6)
print(original_list)  # [1, 2, [3, 4], 5] - original unchanged for top level

# But nested objects are still shared
shallow_copy[2].append(7)
print(original_list)  # [1, 2, [3, 4, 7], 5] - nested list is changed!

# For deep copying of nested structures
import copy
deep_copy = copy.deepcopy(original_list)
```

### 4. String Formatting Best Practices
```python
name = "Alice"
age = 30
score = 95.67

# Modern f-string formatting (Python 3.6+) - Preferred
message = f"Hello {name}, you are {age} years old and scored {score:.1f}%"

# Format method (older but still valid)
message = "Hello {}, you are {} years old and scored {:.1f}%".format(name, age, score)

# Avoid old % formatting (unless maintaining legacy code)
message = "Hello %s, you are %d years old" % (name, age)  # Not recommended
```

---

## Summary

This tutorial covered the fundamental Python concepts essential for data science:

**Basic Data Types:**
- Numbers (int, float) and arithmetic operations
- Strings and string formatting
- Booleans for logical operations

**Collections:**
- Lists: ordered, mutable sequences
- Dictionaries: key-value pairs for structured data
- Tuples: immutable sequences for fixed data
- Sets: collections of unique elements

**Control Flow:**
- Comparison and logical operators
- if/elif/else statements for decision making
- for and while loops for iteration
- List comprehensions for concise data processing

**Functions:**
- Defining reusable code blocks
- Parameters, default values, and return statements
- Lambda expressions for simple operations
- Map and filter for functional programming

**Object Methods:**
- String methods for text processing
- List methods for data manipulation
- Dictionary methods for structured data handling

These concepts form the foundation for data analysis, machine learning, and scientific computing in Python. Practice with real datasets and gradually move on to libraries like NumPy, Pandas, and Matplotlib to apply these skills to data science projects.

---

## Next Steps

In the next tutorial, we'll explore:
- File input/output operations
- Error handling with try/except
- Introduction to Python libraries
- Working with CSV files
- Basic data analysis patterns

Keep practicing these fundamentals - they're the building blocks for everything else in Python data science!
