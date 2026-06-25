# 🐍 Python Tutorial: Modules in Python

> **Series:** Python from Beginner to Advanced | **Topic 4:** Modules
>
> **Goal:** Understand what modules are, why they exist, the different ways to import them, and how to explore Python's rich standard library — plus how to install third-party packages.

---

## 🧠 What is a Module? (The Analogy)

Imagine a **toolbox**. You don't carry every tool you own in your pockets all day — that would be impractical. Instead, you have a toolbox where tools are organised by purpose: one drawer for electrical tools, one for plumbing, one for woodworking.

When you need a screwdriver, you open the right drawer and take it out.

A **module** in Python is that drawer. It's a file full of related functions, constants, and classes — grouped together by purpose. You **import** the drawer (module) when you need it, rather than having everything loaded all the time.

```
Your Python program
│
├── math        → maths functions  (sqrt, ceil, floor, pi ...)
├── random      → random number generation
├── os          → interact with the operating system
├── datetime    → dates and times
├── json        → read/write JSON data
└── your_own.py → modules YOU write!
```

---

## 🗺️ Three Ways to Import

| Syntax | What it does | Example usage |
|--------|-------------|---------------|
| `import module` | Import the whole module | `math.sqrt(25)` |
| `from module import name` | Import one specific item | `sqrt(25)` |
| `from module import *` | Import everything (not recommended) | `sqrt(25)` |
| `import module as alias` | Import with a shorter name | `m.sqrt(25)` |
| `from module import name as alias` | Import item with a new name | `sq(25)` |

---

## 1️⃣ `import module` — Import the Whole Module

```python
import math

# Access everything through the module name using dot notation
num = 64
square_root = math.sqrt(num)
print(square_root)         # 8.0

mark = 73.2
rounded_up   = math.ceil(mark)    # always rounds UP  → 74
rounded_down = math.floor(mark)   # always rounds DOWN → 73
print(rounded_up)    # 74
print(rounded_down)  # 73

# Constants live in modules too
print(math.pi)    # 3.141592653589793
print(math.e)     # 2.718281828459045  (Euler's number)
```

> 💡 **Dot notation** (`math.sqrt`) makes it crystal clear *where* `sqrt` is coming from. When you or a teammate reads the code later, you know immediately it's from the `math` module — not some function you defined elsewhere.

---

## 2️⃣ `from module import name` — Import Specific Items

When you only need one or two things from a module, you can import just those items. Then you use them **directly**, without the module prefix.

```python
from math import sqrt, ceil, pi

num = 144
print(sqrt(num))   # 12.0   ← no "math." prefix needed

score = 87.4
print(ceil(score)) # 88

radius = 7
area = pi * radius ** 2
print(round(area, 2))   # 153.94
```

> ✅ **When to use this:** When you need a small number of items from a module and want cleaner, shorter code.

### ⚠️ The name collision risk

```python
# Dangerous if you also define your own sqrt somewhere!
from math import sqrt

def sqrt(x):              # ← this overwrites the imported sqrt!
    return "my version"

print(sqrt(25))   # "my version" — not what you expected
```

> 🔒 Using `import math` and `math.sqrt()` avoids this entirely — the module name acts as a **namespace** (a safe container for names).

---

## 3️⃣ `import module as alias` — Give It a Shorter Name

Long module names can clutter your code. Aliases let you shorten them.

```python
import math as m

print(m.sqrt(81))    # 9.0
print(m.pi)          # 3.141592653589793
print(m.factorial(6))  # 720  (6! = 6×5×4×3×2×1)
```

```python
# This is standard practice in data science — you'll see these everywhere:
import numpy as np          # the convention for NumPy
import pandas as pd         # the convention for Pandas
import matplotlib.pyplot as plt   # the convention for Matplotlib
```

> 💡 These aliases (`np`, `pd`, `plt`) are universal conventions in the Python community — use them and your code will be immediately readable to any data scientist worldwide.

---

## 4️⃣ `from module import name as alias`

You can also alias individual imported names:

```python
from math import sqrt as square_root
from math import factorial as fact

print(square_root(49))  # 7.0
print(fact(5))          # 120
```

---

## 5️⃣ `from module import *` — Import Everything (Use with Caution)

```python
from math import *   # imports ALL public names from math

print(sqrt(36))   # 6.0
print(pi)         # 3.14...
print(ceil(4.1))  # 5
```

> ⚠️ **Avoid this in real projects.** It floods your namespace with dozens of names — you won't know what came from where, and name collisions become likely. It's fine for quick experiments in a shell, but not in production code.

---

## 6️⃣ Exploring a Module — `dir()` and `help()`

How do you find out what's *inside* a module without reading the docs?

```python
import math

# List everything in the module
print(dir(math))
# ['__doc__', '__loader__', ..., 'acos', 'acosh', 'asin', 'atan', 'atan2',
#  'ceil', 'comb', 'copysign', 'cos', 'cosh', 'degrees', 'dist', 'e', 'erf',
#  'erfc', 'exp', 'expm1', 'fabs', 'factorial', 'floor', 'fmod', 'frexp',
#  'fsum', 'gamma', 'gcd', 'hypot', 'inf', 'isclose', 'isfinite', 'isinf',
#  'isnan', 'isqrt', 'lcm', 'ldexp', 'lgamma', 'log', 'log10', 'log1p',
#  'log2', 'modf', 'nan', 'perm', 'pi', 'pow', 'prod', 'radians', 'remainder',
#  'sin', 'sinh', 'sqrt', 'tan', 'tanh', 'tau', 'trunc']

# Get help on a specific function
help(math.log)
# log(x, [base=math.e])
# Return the logarithm of x to the given base...
```

---

## 7️⃣ The `math` Module — A Closer Look

Since `math` is the most common beginner module, here's a full tour:

```python
import math

# ── Rounding ────────────────────────────────────────────────
print(math.ceil(4.1))     # 5   → always rounds UP
print(math.floor(4.9))    # 4   → always rounds DOWN
print(math.trunc(-3.7))   # -3  → chops decimal (towards zero)
print(round(4.5678, 2))   # 4.57 → built-in round (not math module)

# ── Powers & Roots ──────────────────────────────────────────
print(math.sqrt(144))     # 12.0
print(math.pow(2, 8))     # 256.0  (returns float; ** returns int)
print(math.isqrt(50))     # 7      (integer square root, no decimal)

# ── Logarithms ──────────────────────────────────────────────
print(math.log(100, 10))  # 2.0   (log base 10 of 100)
print(math.log2(1024))    # 10.0  (log base 2)
print(math.log10(1000))   # 3.0   (shortcut for base 10)

# ── Trigonometry ────────────────────────────────────────────
# Note: Python's trig functions use RADIANS, not degrees
angle_deg = 90
angle_rad = math.radians(angle_deg)   # convert to radians first

print(math.sin(angle_rad))   # 1.0
print(math.cos(angle_rad))   # ~0.0 (not exactly 0 due to float precision)
print(math.degrees(math.pi)) # 180.0  (radians back to degrees)

# ── Factorial & Combinations ────────────────────────────────
print(math.factorial(5))     # 120   (5! = 5×4×3×2×1)
print(math.comb(10, 3))      # 120   ("10 choose 3" — combinatorics)
print(math.perm(5, 2))       # 20    (permutations: 5 choices, pick 2)

# ── Constants ────────────────────────────────────────────────
print(math.pi)    # 3.141592653589793
print(math.e)     # 2.718281828459045
print(math.tau)   # 6.283185307179586  (2π)
print(math.inf)   # inf  (positive infinity)
print(math.nan)   # nan  (Not a Number)
```

---

## 8️⃣ Other Essential Standard Library Modules

Python ships with a massive **standard library** — hundreds of modules ready to use with no installation.

### `random` — Random Numbers

```python
import random

# Random float between 0 and 1
print(random.random())             # e.g. 0.5714...

# Random integer between a and b (inclusive)
dice_roll = random.randint(1, 6)
print(dice_roll)                   # e.g. 4

# Random float between a and b
temp = random.uniform(18.0, 35.0)
print(round(temp, 1))             # e.g. 27.3

# Pick a random item from a list
fruits = ["mango", "apple", "cherry", "guava"]
print(random.choice(fruits))       # e.g. "cherry"

# Pick multiple items (without repetition)
print(random.sample(fruits, 2))    # e.g. ["guava", "apple"]

# Shuffle a list in place
cards = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
random.shuffle(cards)
print(cards)                       # e.g. [7, 2, 9, 1, 5, ...]

# Set a seed for reproducible results (useful for testing)
random.seed(42)
print(random.randint(1, 100))      # always 52 with seed 42
```

### `datetime` — Dates and Times

```python
from datetime import datetime, date, timedelta

# Current date and time
now = datetime.now()
print(now)                         # 2026-06-25 14:32:10.123456
print(now.year)                    # 2026
print(now.month)                   # 6
print(now.day)                     # 25
print(now.strftime("%d %B %Y"))    # 25 June 2026  (formatted string)

# Just today's date
today = date.today()
print(today)                       # 2026-06-25

# Date arithmetic
birthday    = date(1995, 11, 20)
days_lived  = (today - birthday).days
print(f"Days lived: {days_lived:,}")   # Days lived: 11,174

# Add/subtract time
in_two_weeks = today + timedelta(weeks=2)
print(in_two_weeks)                # 2026-07-09

# Parse a date string
event = datetime.strptime("15-08-2026", "%d-%m-%Y")
print(event.strftime("%A, %d %B %Y"))  # Saturday, 15 August 2026
```

### `os` — Operating System Interface

```python
import os

# Current working directory
print(os.getcwd())            # /home/user/projects

# List files in a directory
files = os.listdir(".")
print(files)                  # ['main.py', 'data.csv', 'readme.md']

# Check if a path exists
print(os.path.exists("data.csv"))   # True or False

# Join paths safely (handles / vs \ across OS)
path = os.path.join("folder", "subfolder", "file.txt")
print(path)    # folder/subfolder/file.txt  (or folder\subfolder\file.txt on Windows)

# Get file size
size = os.path.getsize("data.csv")
print(f"{size:,} bytes")

# Environment variables
home = os.environ.get("HOME", "not found")
print(home)   # /home/username
```

### `json` — Working with JSON Data

```python
import json

# Python dict → JSON string (serialise)
user = {
    "name": "Omar",
    "age": 31,
    "skills": ["Python", "SQL", "Excel"],
    "active": True
}

json_string = json.dumps(user, indent=2)
print(json_string)
# {
#   "name": "Omar",
#   "age": 31,
#   "skills": ["Python", "SQL", "Excel"],
#   "active": true
# }

# JSON string → Python dict (deserialise)
raw = '{"city": "Dubai", "population": 3604000}'
data = json.loads(raw)
print(data["city"])        # Dubai
print(type(data))          # <class 'dict'>
```

### `sys` — System Information

```python
import sys

print(sys.version)        # Python version
print(sys.platform)       # 'linux', 'darwin', 'win32'
print(sys.argv)           # command-line arguments passed to your script

# Immediately stop a program (exit code 0 = success, 1 = error)
# sys.exit(0)
```

---

## 9️⃣ Writing Your Own Module

Any `.py` file *is* a module. You can import your own files just like built-in ones.

### Step 1 — Create `calculator.py`

```python
# calculator.py

PI = 3.14159

def add(a, b):
    """Return the sum of a and b."""
    return a + b

def subtract(a, b):
    """Return a minus b."""
    return a - b

def circle_area(radius):
    """Return the area of a circle."""
    return PI * radius ** 2
```

### Step 2 — Import and use it in `main.py`

```python
# main.py  (in the same folder as calculator.py)

import calculator

print(calculator.add(10, 4))           # 14
print(calculator.subtract(10, 4))      # 6
print(calculator.circle_area(5))       # 78.53975
print(calculator.PI)                   # 3.14159

# Or import specific items
from calculator import add, circle_area

print(add(7, 3))           # 10
print(circle_area(3))      # 28.27431
```

### The `if __name__ == "__main__":` guard

```python
# calculator.py

def add(a, b):
    return a + b

# This block ONLY runs when you run calculator.py directly.
# It does NOT run when calculator is imported by another file.
if __name__ == "__main__":
    print("Testing the calculator module...")
    print(add(2, 3))   # 5
```

> 💡 This is one of Python's most important idioms. It lets you include test/demo code in a module file without that code running every time the module is imported.

---

## 🔟 Third-Party Packages — `pip`

The standard library is huge — but the Python community has published **hundreds of thousands** more packages on [PyPI (pypi.org)](https://pypi.org). You install them with `pip`, Python's package manager.

```bash
# Install a package
pip install requests

# Install a specific version
pip install requests==2.31.0

# List installed packages
pip list

# Uninstall a package
pip uninstall requests

# Save your project's dependencies to a file
pip freeze > requirements.txt

# Install all dependencies from a file (great for sharing projects)
pip install -r requirements.txt
```

### Popular third-party packages

| Package | Purpose | Install |
|---------|---------|---------|
| `requests` | HTTP requests / calling APIs | `pip install requests` |
| `numpy` | Fast numerical computing | `pip install numpy` |
| `pandas` | Data analysis & dataframes | `pip install pandas` |
| `matplotlib` | Charts and data visualisation | `pip install matplotlib` |
| `flask` | Lightweight web framework | `pip install flask` |
| `django` | Full-featured web framework | `pip install django` |
| `sqlalchemy` | Database ORM | `pip install sqlalchemy` |
| `pytest` | Testing framework | `pip install pytest` |

```python
# Quick example with 'requests' (HTTP calls)
import requests

response = requests.get("https://api.github.com")
print(response.status_code)    # 200 (means OK)
print(response.json()["current_user_url"])
```

---

## 🧩 Quick Reference Summary

```python
# ── Import styles ──────────────────────────────────────────
import math                        # full module
from math import sqrt, pi          # specific items
import math as m                   # with alias
from math import sqrt as sq        # item with alias

# ── Common math functions ──────────────────────────────────
import math
math.sqrt(64)          # 8.0
math.ceil(4.1)         # 5
math.floor(4.9)        # 4
math.factorial(5)      # 120
math.log(100, 10)      # 2.0
math.pi                # 3.14159...

# ── Explore a module ───────────────────────────────────────
import random
print(dir(random))     # list everything inside
help(random.choice)    # docs for a specific function

# ── Write your own ─────────────────────────────────────────
# mymodule.py → define functions there
# main.py     → import mymodule and use them

# ── Install third-party packages ───────────────────────────
# pip install requests
import requests
```

---

## 🏋️ Practice Exercises

**Beginner**
1. Use the `math` module to calculate the hypotenuse of a right triangle with sides 6 and 8. *(Hint: `math.hypot()`)*
2. Use `random.randint()` to simulate rolling two dice 5 times and print both results each time.

**Intermediate**
3. Use `datetime` to calculate how many days until New Year's Day (1 January 2027).
4. Create your own module `converter.py` with functions: `km_to_miles(km)`, `kg_to_lbs(kg)`, `celsius_to_fahrenheit(c)`. Import and test all three from a separate file.

**Advanced**
5. Write a script that reads a JSON string containing a list of students `[{"name": "...", "score": ...}]`, calculates the average score, and prints who is above and below average. Use only the `json` module.
6. Use `os` and `datetime` to create a function that lists all files in the current directory modified within the last 7 days.

---

## ✅ What You Learned

- A **module** is a `.py` file containing related functions, classes, and constants — like a labelled toolbox drawer
- `import module` loads the whole module; access items with **dot notation** (`module.item`)
- `from module import name` imports specific items — no prefix needed but watch for name collisions
- `import module as alias` gives a shorter name (`math as m`, `numpy as np`)
- `dir(module)` lists everything in a module; `help(function)` shows its documentation
- The `math` module covers: `sqrt`, `ceil`, `floor`, `factorial`, `log`, `pow`, trig functions, and constants like `pi` and `e`
- `random`, `datetime`, `os`, `json`, and `sys` are essential standard library modules
- Any `.py` file you create is a module — use `if __name__ == "__main__":` to protect test code
- **Third-party packages** are installed with `pip` and extend Python almost infinitely

---

---

*Part of the **Python from Beginner to Advanced** tutorial series.*
