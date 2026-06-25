# 🐍 Python Tutorial: Functions in Python

> **Series:** Python from Beginner to Advanced | **Topic 3:** Functions
>
> **Goal:** Understand what functions are, why they exist, and how to write them well — from the simplest definition to advanced patterns like default arguments, `*args`, `**kwargs`, and lambda functions.

---

## 🧠 What is a Function? (The Analogy)

Imagine you work at a coffee shop. Every time a customer orders a latte, you:

1. Grab a cup
2. Pull a shot of espresso
3. Steam the milk
4. Pour and hand it over

You do this **same sequence of steps** dozens of times a day. You wouldn't re-invent the process each time — you'd just say **"make a latte"** and everyone knows what that means.

A **function** in Python is exactly that — you **name a sequence of steps once**, and then you can **call that name** whenever you need those steps done. No copy-pasting, no rewriting.

```
Problem without functions:         With functions:
─────────────────────────          ────────────────
result = 4 + 9                     def add(x, y):
print(result)                          return x + y

result = 11 + 7                    add(4, 9)    # clean!
print(result)                      add(11, 7)
                                   add(100, 250)
result = 100 + 250
print(result)                      # one definition, infinite use
```

---

## 🗺️ Functions at a Glance

```
def function_name(parameters):
    """Docstring: what does this function do?"""
    # code goes here (indented — no curly braces!)
    return result   # optional: send a value back to the caller
```

| Part | Required? | What it does |
|------|-----------|--------------|
| `def` | ✅ Yes | Tells Python "I'm defining a function" |
| `function_name` | ✅ Yes | The name you'll use to call it |
| `()` | ✅ Yes | Holds parameters (inputs); empty `()` if none |
| Indented body | ✅ Yes | The code that runs when called |
| `return` | ❌ Optional | Sends a value back; without it, returns `None` |
| Docstring `"""` | ❌ Optional | Documents what the function does (highly recommended) |

---

## 1️⃣ Defining and Calling a Basic Function

```python
# DEFINE the function (this just teaches Python the steps — nothing runs yet)
def greet():
    print("Hello! Welcome to Python.")

# CALL the function (this actually runs the steps)
greet()   # Hello! Welcome to Python.
greet()   # Hello! Welcome to Python.
greet()   # Hello! Welcome to Python.
```

> 💡 Defining a function is like writing a recipe in a cookbook. Calling it is like actually cooking the dish. The cookbook doesn't cook anything by itself.

### ⚠️ Indentation is everything in Python

```python
def greet():
    print("This is inside the function")   # ← indented = part of function
    print("So is this")                    # ← indented = part of function

print("This is OUTSIDE the function")     # ← no indent = not part of function
```

> 🚫 Python uses **indentation (spaces/tabs)** to define code blocks — there are **no curly braces `{}`** like in JavaScript or Java. This is non-negotiable. Wrong indentation = broken code.

```python
# ❌ This will raise an IndentationError
def greet():
print("oops")   # not indented — Python won't accept this
```

---

## 2️⃣ Functions with Parameters (Inputs)

**Parameters** are the inputs your function needs to do its job. Without inputs, `add()` would always add the same two numbers — useless!

```python
# x and y are PARAMETERS (defined in the function signature)
def add(x, y):
    print(x + y)

# 4 and 9 are ARGUMENTS (the actual values passed when calling)
add(4, 9)     # 13
add(11, 7)    # 18
add(50, 50)   # 100
```

> 📌 **Parameter vs Argument** — a common source of confusion:
> - **Parameter**: the variable name in the `def` line (`x`, `y`) — it's a placeholder
> - **Argument**: the actual value you pass in the call (`4`, `9`) — it's the real data

### Multiple parameters

```python
def describe_city(city, country, population):
    print(f"{city} is in {country} with a population of {population:,}.")

describe_city("Tokyo", "Japan", 13_960_000)
describe_city("Cairo", "Egypt", 21_323_000)
describe_city("Dubai", "UAE", 3_604_000)

# Tokyo is in Japan with a population of 13,960,000.
# Cairo is in Egypt with a population of 21,323,000.
# Dubai is in UAE with a population of 3,604,000.
```

---

## 3️⃣ `return` — Getting a Value Back

`print()` inside a function just *shows* a value on screen. `return` actually **sends the value back** to whoever called the function, so you can use it further.

**Real-world analogy:** Asking a calculator for a result. It doesn't just *tell you* the answer and throw it away — it **hands the answer back to you** so you can write it down or use it in the next calculation.

```python
# With print() — result is displayed but LOST
def add(x, y):
    print(x + y)

result = add(6, 4)
print(result)        # None  ← the function printed 10, but returned nothing


# With return — result is handed BACK
def add(x, y):
    return x + y

result = add(6, 4)
print(result)        # 10  ✅
doubled = result * 2
print(doubled)       # 20  ← we can keep using it!
```

### Practical comparison

```python
def subtract(a, b):
    return a - b

def multiply(a, b):
    return a * b

# Chain function calls together
x = multiply(subtract(10, 4), 3)
#             └── 10-4 = 6
#           └── 6 * 3 = 18
print(x)   # 18
```

### `return` exits the function immediately

```python
def check_age(age):
    if age < 0:
        return "Invalid age"   # ← exits here if age is negative
    if age < 18:
        return "Minor"
    return "Adult"             # ← only reached if both checks above pass

print(check_age(-5))    # Invalid age
print(check_age(15))    # Minor
print(check_age(25))    # Adult
```

### Returning multiple values

```python
def min_max(numbers):
    return min(numbers), max(numbers)   # returns a tuple

lo, hi = min_max([4, 1, 9, 2, 7])
print(lo)   # 1
print(hi)   # 9
```

---

## 4️⃣ Docstrings — Documenting Your Functions

A **docstring** is a string written right after the `def` line, inside triple quotes `"""`. It describes what the function does, its parameters, and what it returns.

```python
def add(x, y):
    """Add two numbers and return their sum."""
    return x + y


def describe_person(name, age, city):
    """
    Print a formatted description of a person.

    Parameters:
        name (str): The person's full name.
        age  (int): The person's age in years.
        city (str): The city where they live.
    """
    print(f"{name}, aged {age}, lives in {city}.")
```

### Accessing the docstring

```python
help(add)
# Help on function add:
# add(x, y)
#     Add two numbers and return their sum.

print(add.__doc__)
# Add two numbers and return their sum.
```

> 💡 **Always write docstrings.** Future-you will thank present-you when you come back to code 3 months later and can't remember what a function does. It also enables tools like `help()`, IDEs, and auto-documentation generators.

---

## 5️⃣ Default Parameter Values

Sometimes a parameter has a sensible **default** — something it should be if the caller doesn't bother specifying.

**Real-world analogy:** A coffee shop where the default milk is whole milk. You can ask for oat milk, but if you say nothing, you get the default.

```python
def greet(name, greeting="Hello"):
    """Greet a person, with a customisable greeting."""
    print(f"{greeting}, {name}!")

greet("Sara")                  # Hello, Sara!      (uses default)
greet("Omar")                  # Hello, Omar!      (uses default)
greet("Layla", "Good morning") # Good morning, Layla!  (overrides default)
greet("Ali",   "Marhaba")      # Marhaba, Ali!
```

```python
def power(base, exponent=2):
    """Raise base to a power (squares by default)."""
    return base ** exponent

print(power(5))       # 25   (5² — default exponent)
print(power(3))       # 9    (3²)
print(power(2, 10))   # 1024 (2¹⁰ — overriding default)
```

### ⚠️ Default parameters must come LAST

```python
# ❌ Wrong — non-default after default
def greet(greeting="Hello", name):   # SyntaxError!
    ...

# ✅ Correct — defaults come after non-defaults
def greet(name, greeting="Hello"):
    ...
```

---

## 6️⃣ Keyword Arguments

When calling a function, you can name the arguments explicitly — order no longer matters.

```python
def book_flight(origin, destination, seat_class):
    print(f"Flight: {origin} → {destination} | Class: {seat_class}")

# Positional — order matters
book_flight("Dubai", "London", "Economy")

# Keyword — order doesn't matter
book_flight(destination="Tokyo", seat_class="Business", origin="Cairo")
# Flight: Cairo → Tokyo | Class: Business
```

> 💡 Keyword arguments make function calls **self-documenting** — you can read what each value means without looking at the function definition.

---

## 7️⃣ `*args` — Variable Number of Positional Arguments

What if you don't know in advance how many arguments will be passed?

```python
def add_all(*numbers):
    """Add any number of values."""
    return sum(numbers)

print(add_all(1, 2))              # 3
print(add_all(5, 10, 15))         # 30
print(add_all(2, 4, 6, 8, 10))    # 30
```

> `*args` collects all extra positional arguments into a **tuple**. The name `args` is a convention — `*values`, `*nums` etc. also work.

```python
def display_menu(*items):
    print("Today's Menu:")
    for i, item in enumerate(items, start=1):
        print(f"  {i}. {item}")

display_menu("Pasta", "Pizza", "Salad", "Soup")
# Today's Menu:
#   1. Pasta
#   2. Pizza
#   3. Salad
#   4. Soup
```

---

## 8️⃣ `**kwargs` — Variable Number of Keyword Arguments

`**kwargs` collects extra **named** arguments into a **dictionary**.

```python
def create_profile(**details):
    """Build a user profile from any set of named details."""
    for key, value in details.items():
        print(f"  {key}: {value}")

create_profile(name="Fatima", age=28, city="Abu Dhabi", role="Engineer")
# name: Fatima
# age: 28
# city: Abu Dhabi
# role: Engineer
```

### Combining everything

```python
def full_example(required, *args, default="hi", **kwargs):
    print(f"required: {required}")
    print(f"args:     {args}")
    print(f"default:  {default}")
    print(f"kwargs:   {kwargs}")

full_example("must", 1, 2, 3, default="hey", x=10, y=20)
# required: must
# args:     (1, 2, 3)
# default:  hey
# kwargs:   {'x': 10, 'y': 20}
```

---

## 9️⃣ Scope — Where Variables Live

**Real-world analogy:** Variables inside a function are like notes written on a whiteboard *inside a meeting room*. Once you leave the room (function ends), the whiteboard is wiped. Variables outside are like a noticeboard in the hallway — everyone can see them.

```python
total = 100   # GLOBAL variable — visible everywhere

def update():
    bonus = 50               # LOCAL variable — only exists inside update()
    print(total + bonus)     # can READ global inside function → 150

update()
print(total)   # 100  ← unchanged
# print(bonus) ← ❌ NameError — bonus doesn't exist out here
```

### Modifying a global variable

```python
count = 0

def increment():
    global count      # ← tell Python "I mean the global count"
    count += 1

increment()
increment()
increment()
print(count)   # 3
```

> ⚠️ Using `global` is generally discouraged in clean code. Prefer returning values and passing them in as arguments.

---

## 🔟 Lambda Functions — One-Line Functions

A **lambda** is a tiny, anonymous function written in a single line. Perfect for short, throwaway logic.

```python
# Regular function
def square(n):
    return n ** 2

# Equivalent lambda
square = lambda n: n ** 2

print(square(7))   # 49
```

```python
# Lambdas shine when used inline
numbers = [3, 1, 8, 5, 2, 9]

sorted_nums = sorted(numbers)
print(sorted_nums)   # [1, 2, 3, 5, 8, 9]

# Sort a list of tuples by the second element
pairs = [(1, "banana"), (3, "apple"), (2, "cherry")]
pairs.sort(key=lambda item: item[1])   # sort alphabetically by fruit name
print(pairs)   # [(3, 'apple'), (1, 'banana'), (2, 'cherry')]
```

> 📌 Lambdas are great for short logic passed to functions like `sorted()`, `map()`, and `filter()`. For anything longer than one expression, write a regular `def` function — readability wins.

---

## 1️⃣1️⃣ Built-in Functions — `print` is a Function Too!

You've been using functions all along! Python ships with dozens of built-in functions:

```python
# You already know these — they're all functions
print("Hello")             # displays output
len([1, 2, 3, 4])          # returns 4
type(3.14)                 # returns <class 'float'>
range(1, 10)               # generates a sequence
int("42")                  # converts to integer
round(3.14159, 2)          # returns 3.14
abs(-17)                   # returns 17
max(4, 9, 1, 7)            # returns 9
min(4, 9, 1, 7)            # returns 1
sum([10, 20, 30])          # returns 60
sorted([3, 1, 2])          # returns [1, 2, 3]
```

> 💡 Every time you write `something()` — you're calling a function. The only difference between built-ins and your own is *who wrote it*.

---

## 🏗️ Real-World Example: Putting It All Together

```python
def calculate_invoice(customer, *items, tax_rate=0.05, currency="AED"):
    """
    Calculate and print a customer invoice.

    Parameters:
        customer  (str):   Customer name.
        *items    (float): One or more item prices.
        tax_rate  (float): Tax rate as a decimal (default 5%).
        currency  (str):   Currency symbol (default AED).

    Returns:
        float: Total amount including tax.
    """
    subtotal = sum(items)
    tax      = subtotal * tax_rate
    total    = subtotal + tax

    print(f"\n--- Invoice for {customer} ---")
    print(f"  Items:    {currency} {subtotal:.2f}")
    print(f"  Tax ({tax_rate*100:.0f}%): {currency} {tax:.2f}")
    print(f"  TOTAL:    {currency} {total:.2f}")
    print(f"{'─' * 30}")

    return total

# Usage
t1 = calculate_invoice("Ahmed Al-Rashid", 120, 85, 45, 30)
t2 = calculate_invoice("Sara Malik", 299.99, tax_rate=0.10, currency="USD")

# --- Invoice for Ahmed Al-Rashid ---
#   Items:    AED 280.00
#   Tax (5%): AED 14.00
#   TOTAL:    AED 294.00
# ──────────────────────────────
# 
# --- Invoice for Sara Malik ---
#   Items:    USD 299.99
#   Tax (10%): USD 30.00
#   TOTAL:    USD 329.99
# ──────────────────────────────
```

---

## 🧩 Quick Reference Summary

```python
# Basic function
def greet():
    print("Hello!")

# With parameters and return
def add(x, y):
    return x + y

# With docstring
def multiply(a, b):
    """Multiply two numbers."""
    return a * b

# With default parameter
def power(base, exp=2):
    return base ** exp

# With keyword arguments (at call site)
power(base=3, exp=4)   # 81

# With *args (any number of positional args)
def total(*nums):
    return sum(nums)

# With **kwargs (any number of keyword args)
def profile(**info):
    return info

# Lambda (one-liner)
double = lambda x: x * 2

# Calling functions
greet()
result = add(3, 7)
print(result)   # 10
```

---

## 🏋️ Practice Exercises

**Beginner**
1. Write a function `circle_area(radius)` that returns the area of a circle (`π × r²`). Use `3.14159` for π.
2. Write a function `is_even(n)` that returns `True` if `n` is even, `False` otherwise.

**Intermediate**
3. Write a function `celsius_to_fahrenheit(c)` and `fahrenheit_to_celsius(f)`. Test them: 100°C should give 212°F and back.
4. Write a function `greet_all(*names)` that prints "Hello, [name]!" for each name passed in.

**Advanced**
5. Write a function `summarise(**scores)` that takes any number of subject-score pairs and prints the subject, score, and a grade (A/B/C/D/F based on the score). Also print the average at the end.
6. Rewrite `calculate_invoice()` from above but add a `discount` parameter (default `0`) that applies a percentage discount before tax.

---

## ✅ What You Learned

- A **function** is a named, reusable block of code — define once, call many times
- Use `def` to define, and `functionName()` to call
- Python uses **indentation** (not `{}`) to define what belongs inside a function
- **Parameters** are placeholders; **arguments** are the actual values you pass
- `print()` *displays* a value; `return` *sends it back* for further use
- Without `return`, a function returns `None`
- **Docstrings** (`"""..."""`) document your function — always write them
- **Default parameters** give fallback values when arguments are omitted
- **Keyword arguments** let you pass values by name, in any order
- `*args` collects unlimited positional arguments as a tuple
- `**kwargs` collects unlimited keyword arguments as a dictionary
- Variables inside functions are **local** (they disappear when the function ends)
- **Lambda** functions are one-line anonymous functions for simple, throwaway logic
- `print`, `len`, `range`, `sum`, and friends are all **built-in functions**

---


---

*Part of the **Python from Beginner to Advanced** tutorial series.*
