# 🐍 Python Tutorial: Introduction to Data Types

> **Series:** Python from Beginner to Advanced | **Topic 1:** Data Types
>
> **Goal:** By the end of this page, you'll understand what data types are and how Python uses them — even if you've never written a single line of code before.

---

## 🧠 What is a Data Type? (The Analogy)

Imagine you're organising items in your home. You wouldn't store water in a paper bag, or keep a book in a fish tank. Every item has a *container that makes sense for it*.

In Python, a **data type** is the container your data lives in. Numbers go in one type of container, text in another, and yes/no answers in yet another.

Python needs to know *what kind* of data you have so it knows **what to do with it**. You can add two numbers together — but you can't divide a name in half.

---

## 🗺️ The Python Type Family Tree

```
Python Data Types
│
├── Numeric
│   ├── int        → whole numbers         (e.g., 42, -7, 1000)
│   ├── float      → decimal numbers       (e.g., 3.14, -0.5, 9.99)
│   └── complex    → numbers with √-1      (e.g., 2+3j)
│
├── Boolean        → True or False only    (e.g., True, False)
│
├── Sequence
│   ├── str        → text / string         (e.g., "hello")
│   ├── list       → ordered collection    (e.g., [1, 2, 3])
│   ├── tuple      → immutable list        (e.g., (1, 2, 3))
│   └── range      → number sequences      (e.g., range(0, 10))
│
├── Set            → unique items only     (e.g., {1, 2, 3})
│
└── Mapping
    └── dict       → key-value pairs       (e.g., {"name": "Ali"})
```

> 💡 This tutorial covers **int, float, complex, bool, and range**. Strings, lists, dicts, and others get their own dedicated pages.

---

## 1️⃣ `int` — Whole Numbers

### What is it?

An `int` (short for **integer**) is any whole number — positive, negative, or zero. No decimal point allowed.

**Real-world analogy:** Think of counting people in a room. You can have 5 people, 0 people, or -3 (a debt of 3 people, if that were a thing) — but never 2.5 people.

### Examples

```python
age = 28
temperature = -4
population = 1_400_000_000   # underscores make big numbers readable!
floors_underground = -3

print(age)           # 28
print(temperature)   # -4
print(population)    # 1400000000
```

> 💡 **Pro tip:** Python allows underscores in numbers (`1_000_000`) purely for readability — they're ignored by Python but make large numbers much easier to read.

### Checking the type

```python
x = 42
print(type(x))   # <class 'int'>
```

### Operations with `int`

```python
a = 15
b = 4

print(a + b)    # 19   → addition
print(a - b)    # 11   → subtraction
print(a * b)    # 60   → multiplication
print(a // b)   # 3    → floor division (drops the decimal)
print(a % b)    # 3    → modulo (remainder after division)
print(a ** b)   # 50625 → exponentiation (15 to the power of 4)
```

> 🔍 Notice that `a / b` (regular division) gives `3.75` — a **float**, not an int! Python automatically upgrades to float when needed.

```python
print(a / b)     # 3.75  → <class 'float'>
```

---

## 2️⃣ `float` — Decimal Numbers

### What is it?

A `float` (short for **floating-point number**) is any number with a decimal point.

**Real-world analogy:** Measuring your height in centimetres. You're not exactly 175 cm — you might be 175.4 cm. That precision requires a float.

### Examples

```python
pi = 3.14159
account_balance = 2_450.75
temperature_celsius = -12.5
gravity = 9.81   # m/s²

print(pi)                 # 3.14159
print(account_balance)    # 2450.75
print(type(gravity))      # <class 'float'>
```

### Scientific notation (a bonus feature!)

```python
distance_to_sun = 1.496e11   # 1.496 × 10¹¹ metres
electron_mass = 9.11e-31     # 9.11 × 10⁻³¹ kilograms

print(distance_to_sun)   # 149600000000.0
print(electron_mass)     # 9.11e-31
```

### Float precision — a famous gotcha

```python
print(0.1 + 0.2)   # 0.30000000000000004  😮
```

> ⚠️ This isn't a Python bug — it's a computer science reality. Computers store decimals in binary, and `0.1` can't be represented perfectly in binary (similar to how `1/3` can't be written perfectly in decimal). For financial calculations, use the `decimal` module instead.

```python
from decimal import Decimal
print(Decimal("0.1") + Decimal("0.2"))   # 0.3  ✅
```

### Converting float → int (type casting)

```python
price = 7.89
discounted = int(price)   # truncates (cuts off) the decimal — does NOT round

print(discounted)         # 7
print(type(discounted))   # <class 'int'>
```

> ⚠️ `int()` **truncates**, it does not round. `int(7.99)` gives `7`, not `8`. Use `round()` if you want rounding.

```python
print(round(7.89))    # 8
print(round(7.89, 1)) # 7.9  (round to 1 decimal place)
```

---

## 3️⃣ `complex` — Numbers with an Imaginary Part

### What is it?

A `complex` number has two parts: a **real** part and an **imaginary** part. In Python, the imaginary unit is written as `j` (engineers use `j`; mathematicians use `i`).

**Real-world analogy:** Think of a map with two axes — East-West and North-South. A complex number is like a coordinate: `3 + 4j` means "3 steps East and 4 steps North". You need *both dimensions* to locate the point.

### Examples

```python
z1 = 3 + 4j
z2 = -1 + 2j
z3 = complex(5, -3)   # another way to write 5 - 3j

print(z1)             # (3+4j)
print(z2)             # (-1+2j)
print(z3)             # (5-3j)
print(type(z1))       # <class 'complex'>
```

### Accessing parts

```python
signal = 6 + 8j

print(signal.real)    # 6.0  → the real part
print(signal.imag)    # 8.0  → the imaginary part
```

### Operations

```python
a = 2 + 3j
b = 1 - 1j

print(a + b)   # (3+2j)
print(a * b)   # (5+1j)   → follows (a+bi)(c+di) = (ac-bd) + (ad+bc)i
```

> 📌 **When do you use complex numbers?** Signal processing, electrical engineering, physics simulations, and some machine learning operations. As a beginner, you may not use them often — but it's good to know Python handles them natively.

---

## 4️⃣ `bool` — True or False

### What is it?

A `bool` (short for **Boolean**, named after mathematician George Boole) can only ever be one of two values: `True` or `False`.

**Real-world analogy:** A light switch. It's either **on** or **off** — there's no "kinda on". Every yes/no question in your program is a Boolean.

### Examples

```python
is_logged_in = True
has_premium = False
is_raining = True
account_verified = False

print(is_logged_in)    # True
print(type(is_raining))  # <class 'bool'>
```

### Booleans come from comparisons

```python
age = 20

print(age >= 18)    # True
print(age == 25)    # False
print(age != 30)    # True
print(age < 15)     # False
```

### Boolean operators

```python
has_ticket = True
has_id = False

# AND → both must be True
print(has_ticket and has_id)   # False (needs both)

# OR → at least one must be True
print(has_ticket or has_id)    # True (has_ticket is True)

# NOT → flips the value
print(not has_id)              # True (flips False → True)
```

### Bool is secretly an int!

This is a famous Python quirk — `True` equals `1` and `False` equals `0`:

```python
print(int(True))    # 1
print(int(False))   # 0

print(True + True)  # 2
print(True * 5)     # 5
print(False + 10)   # 10
```

> 💡 This is why you can do things like `sum([True, False, True, True])` to **count how many True values** are in a list. Very handy!

### Converting to bool

```python
print(bool(0))       # False  → zero is always False
print(bool(99))      # True   → any non-zero number is True
print(bool(""))      # False  → empty string is False
print(bool("hello")) # True   → non-empty string is True
print(bool(None))    # False  → None is always False
```

> 🧠 **Truthy vs Falsy:** Python considers these values as "falsy" (behave like False): `0`, `0.0`, `""`, `[]`, `{}`, `None`. Everything else is "truthy".

---

## 5️⃣ `range` — Number Sequences

### What is it?

`range` generates a **sequence of numbers** without storing them all in memory. It's like a recipe for "give me numbers from X to Y" — Python only creates each number when you actually need it.

**Real-world analogy:** A ticket machine at a deli counter. It doesn't print all 500 tickets in advance — it generates the *next* number only when someone presses the button. Efficient!

### Three ways to use `range`

```python
# 1. range(stop) → starts at 0, goes up to (but NOT including) stop
r1 = range(6)
print(r1)         # range(0, 6)
print(list(r1))   # [0, 1, 2, 3, 4, 5]

# 2. range(start, stop) → from start up to (but NOT including) stop
r2 = range(3, 9)
print(list(r2))   # [3, 4, 5, 6, 7, 8]

# 3. range(start, stop, step) → jump by 'step' each time
r3 = range(0, 20, 5)
print(list(r3))   # [0, 5, 10, 15]
```

> ⚠️ **The stop value is NEVER included.** `range(1, 10)` gives you 1 through 9, not 10. Think of it like a half-open interval: `[start, stop)`.

### Counting backwards

```python
countdown = range(10, 0, -1)   # step = -1 (going down)
print(list(countdown))         # [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]

# Even numbers from 10 down to 2
evens_down = range(10, 1, -2)
print(list(evens_down))        # [10, 8, 6, 4, 2]
```

### Range in a `for` loop (the most common use!)

```python
# Print "Hello" 4 times
for i in range(4):
    print(f"Hello #{i + 1}")

# Output:
# Hello #1
# Hello #2
# Hello #3
# Hello #4
```

```python
# Sum numbers from 1 to 100
total = 0
for n in range(1, 101):
    total += n

print(total)   # 5050  (Gauss's famous result!)
```

### Converting range to a set

```python
# A set holds unique values and has no guaranteed order
numbers = set(range(5, 15))
print(numbers)     # {5, 6, 7, 8, 9, 10, 11, 12, 13, 14}

# Odd numbers from 1 to 19
odds = set(range(1, 20, 2))
print(odds)        # {1, 3, 5, 7, 9, 11, 13, 15, 17, 19}

# Multiples of 3 from 3 to 30
multiples_3 = set(range(3, 31, 3))
print(multiples_3) # {3, 6, 9, 12, 15, 18, 21, 24, 27, 30}
```

### Why not just use a list?

```python
import sys

big_range = range(1, 1_000_000)
big_list = list(range(1, 1_000_000))

print(sys.getsizeof(big_range))   # 48 bytes! 😲
print(sys.getsizeof(big_list))    # ~8,000,056 bytes (8 MB)
```

> 💡 A `range` of a million numbers takes **48 bytes** of memory. The same numbers as a list take **8 megabytes**. Range is memory-efficient because it only stores `start`, `stop`, and `step` — not every number.

---

## 🔄 Type Conversion (Casting)

Python lets you convert between types using built-in functions. Think of it like converting currencies — the value changes form, but represents the same underlying thing (sometimes with precision loss).

### The conversion functions

| Function | Converts to | Example |
|----------|-------------|---------|
| `int()` | Integer | `int(9.7)` → `9` |
| `float()` | Float | `float(5)` → `5.0` |
| `bool()` | Boolean | `bool(0)` → `False` |
| `complex()` | Complex | `complex(3, 2)` → `(3+2j)` |
| `str()` | String | `str(42)` → `"42"` |

### Practical examples

```python
# float → int (truncates decimals)
price = 14.95
whole = int(price)
print(whole)         # 14

# int → float (adds decimal point)
score = 87
score_float = float(score)
print(score_float)   # 87.0

# bool → int (True=1, False=0)
passed = True
print(int(passed))   # 1

# int → bool (0=False, anything else=True)
print(bool(0))       # False
print(bool(-7))      # True

# number → string (for display/concatenation)
rating = 5
message = "Your rating: " + str(rating)
print(message)       # Your rating: 5

# string → int (only works if the string is a valid number!)
user_input = "42"
number = int(user_input)
print(number + 8)    # 50
```

### When conversion fails

```python
# This will raise a ValueError!
int("hello")      # ❌ ValueError: invalid literal for int()

# Check before converting
text = "123abc"
if text.isdigit():
    print(int(text))
else:
    print("Not a valid number")  # ✅ Safe
```

---

## 🧪 `type()` and `isinstance()` — Inspecting Types

### `type()` — tells you what type something is

```python
values = [42, 3.14, True, 2+3j, range(5), "hello"]

for v in values:
    print(f"{v!r:15} → {type(v).__name__}")

# Output:
# 42              → int
# 3.14            → float
# True            → bool
# (2+3j)          → complex
# range(0, 5)     → range
# 'hello'         → str
```

### `isinstance()` — checks if something IS a certain type

```python
x = 7

print(isinstance(x, int))      # True
print(isinstance(x, float))    # False
print(isinstance(x, (int, float)))  # True ← checks multiple types at once!
```

> 💡 Prefer `isinstance()` over `type() ==` in real code — it handles inheritance properly and is more Pythonic.

---

## 🧩 Quick Reference Summary

| Type | Example | Memory tip |
|------|---------|------------|
| `int` | `age = 25` | **Whole numbers only** — like counting |
| `float` | `height = 1.78` | **Decimal numbers** — like measuring |
| `complex` | `z = 3 + 2j` | **Two-part numbers** — real + imaginary |
| `bool` | `done = True` | **On/Off switch** — only True or False |
| `range` | `range(1, 11)` | **Number recipe** — lazy sequence generator |

---

## 🏋️ Practice Exercises

Try these in your Python shell or a `.py` file:

**Beginner**
1. Create variables for your age (`int`), your height in metres (`float`), and whether you prefer tea over coffee (`bool`). Print each one along with its type.
2. Convert your age to a float and your height to an int. What do you notice about your height after conversion?

**Intermediate**
3. Use `range()` to print all multiples of 7 between 1 and 100.
4. Without running it first, predict the output of `bool(0.0001)`. Then verify.

**Advanced**
5. Use `sum()` and `range()` to calculate the sum of all even numbers between 1 and 500.
6. Explore: what is `isinstance(True, int)`? Why do you think that is?

---

## ✅ What You Learned

- Python has several built-in **data types** to hold different kinds of information
- `int` holds whole numbers; `float` holds decimals; `complex` holds two-dimensional numbers
- `bool` is a yes/no type — secretly an `int` under the hood (`True=1`, `False=0`)
- `range` is a memory-efficient way to generate number sequences — it stores a recipe, not the numbers
- You can convert between types using `int()`, `float()`, `bool()`, `str()`, and `complex()`
- Use `type()` to inspect a value's type, and `isinstance()` to check type membership

---

> **Next up:** 📝 [Strings in Python — Working with Text](stringIntro.md) 

---

*Part of the **Python from Beginner to Advanced** tutorial series.*
