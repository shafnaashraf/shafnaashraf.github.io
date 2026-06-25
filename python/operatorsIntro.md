# 🐍 Python Tutorial: Operators in Python

> **Series:** Python from Beginner to Advanced | **Topic 2:** Operators
>
> **Goal:** Understand every type of operator Python offers — with real-world analogies and practical examples you can run immediately.

---

## 🧠 What is an Operator? (The Analogy)

Think of operators as **verbs** in a sentence. In English, "add", "compare", "assign" are actions. In Python, operators are symbols that perform **actions on data**.

```
10 + 5 = 15
↑    ↑   ↑
left  operator  right
operand         operand
```

Python has **7 categories** of operators. Let's go through each one.

---

## 🗺️ Operator Categories at a Glance

| Category | Symbols | What it does |
|----------|---------|--------------|
| Arithmetic | `+ - * / // % **` | Math calculations |
| Assignment | `= += -= *= /=` … | Store / update values |
| Unary | `- + ~` | Operate on a single value |
| Relational (Comparison) | `> < >= <= == !=` | Compare two values → bool |
| Logical | `and or not` | Combine True/False conditions |
| Bitwise | `& \| ^ ~ << >>` | Operate on binary bits |
| Identity & Membership | `is is not in not in` | Check identity / containment |

---

## 1️⃣ Arithmetic Operators

**Real-world analogy:** These are your calculator buttons — they do maths.

```python
a = 18
b = 5
```

| Operator | Name | Example | Result | Notes |
|----------|------|---------|--------|-------|
| `+` | Addition | `a + b` | `23` | |
| `-` | Subtraction | `a - b` | `13` | |
| `*` | Multiplication | `a * b` | `90` | |
| `/` | Division | `a / b` | `3.6` | Always returns `float` |
| `//` | Floor Division | `a // b` | `3` | Drops the decimal (rounds down) |
| `%` | Modulo | `a % b` | `3` | Remainder after division |
| `**` | Exponentiation | `a ** b` | `1889568` | `18` to the power of `5` |

```python
a = 18
b = 5

print(a + b)    # 23
print(a - b)    # 13
print(a * b)    # 90
print(a / b)    # 3.6   ← always a float!
print(a // b)   # 3     ← floor division (cuts decimal)
print(a % b)    # 3     ← remainder: 18 = 5×3 + 3
print(a ** b)   # 1889568
```

### 🔍 Floor Division vs Regular Division

```python
# Imagine splitting 17 apples among 4 people
apples = 17
people = 4

each_gets = apples // people   # 4 whole apples each
leftover  = apples % people    # 1 apple left over

print(f"Each person gets {each_gets} apples, with {leftover} left over.")
# Each person gets 4 apples, with 1 left over.
```

### 🔍 Modulo — more useful than it looks!

```python
# Check if a number is even or odd
number = 47
if number % 2 == 0:
    print("Even")
else:
    print("Odd")   # Odd

# Check if today is a "every 3rd day" reminder
day = 9
if day % 3 == 0:
    print("Send reminder!")   # Send reminder!
```

### 🔍 Operator Precedence (PEMDAS/BODMAS)

Python follows the same order of operations as maths:

```python
# ** first, then * /, then + -
result = 2 + 3 * 4 ** 2
#                 └── 4**2 = 16  first
#             └── 3 * 16 = 48    second
#         └── 2 + 48 = 50        third

print(result)   # 50

# Use parentheses to control order
result2 = (2 + 3) * 4 ** 2
print(result2)  # 80   (5 * 16)
```

> 💡 **Rule of thumb:** When in doubt, use parentheses. It makes your intent clear to both Python and the next person reading your code.

---

## 2️⃣ Assignment Operators

**Real-world analogy:** Think of a variable as a labelled box. The assignment operator `=` is the act of **putting something in the box**. Compound operators like `+=` mean "take what's already in the box, do something to it, and put it back."

### Basic Assignment

```python
score = 100        # put 100 in the box labelled 'score'
name  = "Layla"   # put "Layla" in the box labelled 'name'
pi    = 3.14159
```

> ⚠️ `=` in Python means **"store this value"**, NOT "equals" in the maths sense. For equality comparison, use `==`.

### Multiple Assignment (Swap in one line!)

```python
# Assign multiple variables at once
x, y = 8, 3
print(x)   # 8
print(y)   # 3

# Swap values — Python's elegant trick (no temp variable needed!)
x, y = y, x
print(x)   # 3
print(y)   # 8
```

> 💡 In most languages you need a temporary variable to swap two values. Python does it in one line — Python evaluates the right side fully before assigning.

### Same value to multiple variables

```python
a = b = c = 0
print(a, b, c)   # 0 0 0
```

### Compound Assignment Operators

These are **shortcuts** for updating a variable's own value. They read as "take the current value and do this to it":

```python
budget = 500

budget += 200    # budget = budget + 200  →  700  (got paid!)
print(budget)    # 700

budget -= 150    # budget = budget - 150  →  550  (paid rent)
print(budget)    # 550

budget *= 2      # budget = budget * 2    → 1100  (doubled somehow)
print(budget)    # 1100

budget //= 3     # budget = budget // 3  →  366  (split 3 ways)
print(budget)    # 366

budget **= 2     # budget = budget ** 2  → 133956
print(budget)    # 133956

budget %= 1000   # budget = budget % 1000 →  956  (remainder after ÷1000)
print(budget)    # 956
```

### Full compound operator table

| Operator | Meaning | Example | Equivalent to |
|----------|---------|---------|---------------|
| `+=` | Add and assign | `x += 3` | `x = x + 3` |
| `-=` | Subtract and assign | `x -= 3` | `x = x - 3` |
| `*=` | Multiply and assign | `x *= 3` | `x = x * 3` |
| `/=` | Divide and assign | `x /= 3` | `x = x / 3` |
| `//=` | Floor divide and assign | `x //= 3` | `x = x // 3` |
| `%=` | Modulo and assign | `x %= 3` | `x = x % 3` |
| `**=` | Power and assign | `x **= 3` | `x = x ** 3` |

### ❌ `++` and `--` don't exist in Python!

```python
counter = 10

# counter++   ← ❌ SyntaxError in Python! (works in C, Java, JS — not here)
# counter--   ← ❌ SyntaxError

# ✅ Use these instead:
counter += 1   # increment
counter -= 1   # decrement

print(counter)   # 10
```

> 🤔 **Why no `++`?** Python's designers intentionally left it out to avoid confusion. `x++` and `++x` behave differently in C/Java — Python keeps it simple with `+= 1`.

---

## 3️⃣ Unary Operators

**Real-world analogy:** Unary operators work on a **single value** — like flipping a coin (heads → tails) or putting a negative sign in front of a bank balance.

```python
a = 10

print(-a)    # -10  → negation (unary minus)
print(+a)    #  10  → unary plus (rarely used, leaves value unchanged)
```

```python
# Unary minus is useful for flipping signs dynamically
temperature = -7
print(-temperature)   # 7   (absolute flip)

loss = 250
print(-loss)          # -250 (represent as a negative)
```

### Unary with booleans

```python
is_complete = True
print(-is_complete)   # -1   (True = 1, so -True = -1)

flag = False
print(-flag)          # 0    (False = 0, so -False = 0)
```

> 💡 The logical version of "flip" for booleans is `not` (covered in Logical Operators below). Unary `-` gives the numeric negative.

---

## 4️⃣ Relational (Comparison) Operators

**Real-world analogy:** These are like **judges** — they look at two things and give a verdict of `True` or `False`. Is this score higher than that score? Are these two values equal? That's a relational operator's job.

Every comparison returns a **boolean** (`True` or `False`).

```python
a = 12
b = 20
```

| Operator | Name | Example | Result |
|----------|------|---------|--------|
| `>` | Greater than | `a > b` | `False` |
| `<` | Less than | `a < b` | `True` |
| `>=` | Greater than or equal | `a >= 12` | `True` |
| `<=` | Less than or equal | `a <= b` | `True` |
| `==` | Equal to | `a == b` | `False` |
| `!=` | Not equal to | `a != b` | `True` |

```python
a = 12
b = 20

print(a > b)    # False
print(a < b)    # True
print(a >= 12)  # True   (12 >= 12 is True)
print(a <= b)   # True
print(a == b)   # False
print(a != b)   # True
```

### Real-world example — access control

```python
user_age = 16
min_age  = 18

can_enter = user_age >= min_age
print(can_enter)   # False

speed_kmh  = 115
speed_limit = 120

is_speeding = speed_kmh > speed_limit
print(is_speeding)   # False  (115 is not above 120)
```

### Chaining comparisons — Python's elegant shortcut

```python
score = 74

# Instead of: score >= 50 and score < 90
is_pass = 50 <= score < 90
print(is_pass)    # True

# Check if a value is in a valid range
temperature = 22
is_comfortable = 18 <= temperature <= 26
print(is_comfortable)   # True
```

> 💡 Python is one of the few languages that lets you chain comparisons like `18 <= x <= 26`. Most other languages require the `and` version. Much more readable!

### ⚠️ `==` vs `=` — the most common beginner mistake

```python
x = 5         # ← ASSIGNMENT: puts 5 into x
print(x == 5) # ← COMPARISON: asks "is x equal to 5?" → True
print(x == 9) # ← False
```

---

## 5️⃣ Logical Operators

**Real-world analogy:** Logical operators combine multiple conditions like **decision filters**. To get a bank loan, you need a good credit score **AND** a stable income — both conditions must be true. A nightclub might let you in if you're 18+ **OR** have a VIP pass — just one needs to be true.

```python
age    = 22
salary = 45000
```

### `and` — ALL conditions must be True

```python
has_degree    = True
has_experience = False

is_hired = has_degree and has_experience
print(is_hired)   # False  (both needed, experience is missing)

# Bank loan example
credit_score = 750
income = 55000

loan_approved = credit_score >= 700 and income >= 40000
print(loan_approved)   # True  (both conditions met)
```

### `or` — AT LEAST ONE condition must be True

```python
has_passport = False
has_id_card  = True

can_board = has_passport or has_id_card
print(can_board)   # True  (one is enough)

a = 3
b = 12

print(a < 10 or b < 5)   # True   (a<10 is True, so whole thing is True)
print(a > 10 or b < 5)   # False  (both are False)
```

### `not` — flips the boolean

```python
is_raining = False

should_go_out = not is_raining
print(should_go_out)   # True

result = 5 > 3          # True
print(not result)       # False
print(not (10 == 10))   # False
```

### Combining all three

```python
age       = 22
has_ticket = True
is_vip     = False

# Can enter if: (old enough AND has ticket) OR is VIP
can_enter = (age >= 18 and has_ticket) or is_vip
print(can_enter)   # True
```

### Truth tables — the complete picture

**`and`**

| A | B | A and B |
|---|---|---------|
| True | True | **True** |
| True | False | False |
| False | True | False |
| False | False | False |

**`or`**

| A | B | A or B |
|---|---|--------|
| True | True | **True** |
| True | False | **True** |
| False | True | **True** |
| False | False | False |

**`not`**

| A | not A |
|---|-------|
| True | False |
| False | True |

### Short-circuit evaluation — Python is lazy (in a smart way)

```python
# With 'and': if the FIRST is False, Python skips the second entirely
def check():
    print("check() was called!")
    return True

result = False and check()   # check() is NEVER called — Python already knows the answer
print(result)   # False

# With 'or': if the FIRST is True, Python skips the second
result2 = True or check()    # check() is NEVER called
print(result2)  # True
```

> 💡 This is called **short-circuit evaluation**. Python stops evaluating as soon as it knows the outcome. This is useful for performance and for avoiding errors (e.g. checking if a list is non-empty *before* accessing its first element).

---

## 6️⃣ Bitwise Operators

**Real-world analogy:** Computers store everything as `0`s and `1`s (binary). Bitwise operators work directly on those bits — like adjusting individual switches in a row of on/off switches.

```python
a = 12   # binary: 1100
b = 10   # binary: 1010
```

| Operator | Name | Example | Result | Binary logic |
|----------|------|---------|--------|--------------|
| `&` | AND | `a & b` | `8` | `1100 & 1010 = 1000` |
| `\|` | OR | `a \| b` | `14` | `1100 \| 1010 = 1110` |
| `^` | XOR | `a ^ b` | `6` | `1100 ^ 1010 = 0110` |
| `~` | NOT (flip) | `~a` | `-13` | flips all bits |
| `<<` | Left shift | `a << 2` | `48` | multiply by 2² |
| `>>` | Right shift | `a >> 1` | `6` | divide by 2¹ |

```python
a = 12   # 1100 in binary
b = 10   # 1010 in binary

print(a & b)    # 8   → 1000
print(a | b)    # 14  → 1110
print(a ^ b)    # 6   → 0110
print(~a)       # -13 (flips all bits; ~n = -(n+1) always)
print(a << 2)   # 48  (12 × 4)
print(a >> 1)   # 6   (12 ÷ 2)
```

> 📌 Bitwise operators are used in **low-level programming**, **networking** (IP masks), **image processing**, and **cryptography**. You may not use them daily as a beginner, but they're worth knowing.

---

## 7️⃣ Identity & Membership Operators

### Identity Operators — `is` and `is not`

**Real-world analogy:** `==` asks "do these two people have the same name?" — `is` asks "are these literally the same person?" Two people named "Ali" are `==` but not `is`.

```python
a = [1, 2, 3]
b = [1, 2, 3]
c = a            # c points to the SAME list as a

print(a == b)    # True  → same values
print(a is b)    # False → different objects in memory!
print(a is c)    # True  → same object (c is just another name for a)
```

```python
# 'is' is perfect for checking against None
result = None

if result is None:
    print("No result yet")   # ✅ Pythonic
```

> ⚠️ Never use `== None`. Always use `is None`. It's both more correct and more idiomatic Python.

### Membership Operators — `in` and `not in`

**Real-world analogy:** "Is my name on the guest list?" — that's a membership check.

```python
fruits = ["apple", "mango", "cherry"]

print("mango" in fruits)       # True
print("banana" in fruits)      # False
print("banana" not in fruits)  # True
```

```python
# Works on strings too!
email = "user@example.com"

print("@" in email)            # True  ← valid email check starter
print(".com" in email)         # True

sentence = "The quick brown fox"
print("quick" in sentence)     # True
print("lazy" in sentence)      # False
```

```python
# Works on ranges and sets
valid_scores = range(0, 101)   # 0 to 100
print(85 in valid_scores)      # True
print(105 in valid_scores)     # False

allowed_roles = {"admin", "editor", "viewer"}
user_role = "editor"
print(user_role in allowed_roles)   # True
```

---

## 🔁 Operator Precedence — Full Order

When multiple operators appear together, Python evaluates them in this order (highest → lowest):

| Priority | Operator(s) | Description |
|----------|-------------|-------------|
| 1 (highest) | `()` | Parentheses |
| 2 | `**` | Exponentiation |
| 3 | `+x -x ~x` | Unary operators |
| 4 | `* / // %` | Multiplication, division |
| 5 | `+ -` | Addition, subtraction |
| 6 | `<< >>` | Bitwise shifts |
| 7 | `&` | Bitwise AND |
| 8 | `^` | Bitwise XOR |
| 9 | `\|` | Bitwise OR |
| 10 | `== != > < >= <= is is not in not in` | Comparisons |
| 11 | `not` | Logical NOT |
| 12 | `and` | Logical AND |
| 13 (lowest) | `or` | Logical OR |

```python
# Example tracing precedence
x = 2 + 3 * 4 > 10 and not False
#       └──── 3*4 = 12     (step 1: *)
#   └──── 2 + 12 = 14      (step 2: +)
#               └── 14>10 = True  (step 3: >)
#                           └── not False = True (step 4: not)
#                   └── True and True = True      (step 5: and)
print(x)   # True
```

---

## 🧩 Quick Reference Summary

```python
# Arithmetic
print(17 + 4)    # 21
print(17 - 4)    # 13
print(17 * 4)    # 68
print(17 / 4)    # 4.25
print(17 // 4)   # 4
print(17 % 4)    # 1
print(17 ** 4)   # 83521

# Assignment shortcuts
n = 10
n += 5;  print(n)   # 15
n -= 3;  print(n)   # 12
n *= 2;  print(n)   # 24
n //= 5; print(n)   # 4

# Unary
x = 9
print(-x)    # -9

# Relational
print(8 > 3)     # True
print(8 == 8)    # True
print(8 != 3)    # True

# Logical
print(True and False)    # False
print(True or False)     # True
print(not True)          # False

# Identity
a = None
print(a is None)         # True

# Membership
print("e" in "hello")   # True
```

---

## 🏋️ Practice Exercises

**Beginner**
1. Calculate the number of full weeks and remaining days in 100 days using `//` and `%`.
2. A product costs 349.99. Apply a 15% discount using `*=`. Print the final price.

**Intermediate**
3. Without running it first, predict: `10 > 3 and not (5 == 5) or 2 < 4`. Then verify.
4. Swap two variables `p = "hello"` and `q = "world"` in a single line.

**Advanced**
5. Use only arithmetic operators (`+`, `*`, `//`, `%`) to extract the tens digit from any 3-digit number (e.g., from `374`, extract `7`).
6. Write a condition that checks if a number is **divisible by both 3 and 5** using `%` and `and`. Test it on 15, 9, and 25.

---

## ✅ What You Learned

- **Arithmetic operators** do maths — `+`, `-`, `*`, `/`, `//`, `%`, `**`
- **Assignment operators** store values — `=`, and compound shortcuts like `+=`, `-=`
- Python has **no `++` or `--`** — use `+= 1` and `-= 1` instead
- **Unary operators** work on one value — `-x` flips the sign
- **Relational operators** compare values and return `True`/`False`
- Python supports **chained comparisons** like `0 <= x < 100`
- **Logical operators** (`and`, `or`, `not`) combine conditions with short-circuit evaluation
- **Bitwise operators** work on binary representations of numbers
- **Identity** (`is`/`is not`) checks if two variables point to the same object
- **Membership** (`in`/`not in`) checks if a value exists inside a collection

---


---

*Part of the **Python from Beginner to Advanced** tutorial series.*
