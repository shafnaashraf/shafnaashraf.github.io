---
title: "Python Tuples - The Sealed Box Version of a List"
chapter: 5
level: beginner
---

# Python Tuples: When You Don't Want Things to Change

## 1. The Big Picture (For Non-Techies)

In the last chapter, your shopping cart (**list**) let you freely add, remove, and rearrange items. Now imagine something different: a **sealed gift box**. Once it's packed and sealed at the factory, you can look inside and see what's there, but you can't swap an item out without breaking the seal.

That sealed box is a **tuple**. It holds multiple items, just like a list — but once created, its contents **cannot be changed**.

---

## 2. Creating a Tuple

A tuple is written using **round brackets `( )`**, with items separated by commas.

```python
tup = (23, 45, 67, 43)
type(tup)
```
```
<class 'tuple'>
```

Interestingly, the brackets are actually optional — Python looks at the **commas**, not the brackets, to decide it's a tuple:

```python
tup1 = 1, 23, 43, 54, 65, 54
type(tup1)
```
```
<class 'tuple'>
```

**Analogy:** It's like how a wedding ring is "a ring" whether or not it comes in a fancy box. The commas are what truly define a tuple; the parentheses are just for clarity.

### ⚠️ The classic single-item trap

```python
tup = (45)
type(tup)
```
```
<class 'int'>
```

This is **not** a tuple — it's just the number `45` with parentheses around it (like writing `(2 + 2)` in math, the brackets don't create anything new). To make a tuple of one item, you **must add a trailing comma**:

```python
tup = 45,
type(tup)
```
```
<class 'tuple'>
```

**Analogy:** Imagine labeling an empty gift box "contains 1 item" — without that label (the comma), people will assume it's just a loose item, not a box at all.

---

## 3. Tuples Support the Same "Read" Operations as Lists

Even though tuples can't be changed, you can still freely look inside, measure, and calculate:

```python
tup = (23, 45, 67, 43)

tup[1]
```
```
45
```

```python
min(tup)
```
```
23
```

```python
max(tup)
```
```
67
```

```python
sum(tup)
```
```
178
```

**Analogy:** Think of a sealed museum display case. You can peer through the glass, count the items, find the smallest and largest one — you just can't reach in and rearrange them.

---

## 4. The Core Difference: Mutability

This is the single most important concept in this chapter, so let's slow down on it.

- **List = mutable** → you *can* change it after creation.
- **Tuple = immutable** → you *cannot* change it after creation.

**Analogy:** A **list** is like a whiteboard — write on it, erase, rewrite as much as you like. A **tuple** is like a printed photograph — once it's printed, you can look at it, share it, copy it, but you can't edit what's actually printed on the paper.

Let's see this in action.

### Lists can be modified:

```python
l = [23, 45, 34]
l[1] = 450
l
```
```
[23, 450, 34]
```

You can even replace a whole slice of a list at once:

```python
l[0:2] = [1, 2]
l
```
```
[1, 2, 34]
```

### Tuples refuse to be modified:

```python
tup[1] = 450
```
```
Traceback (most recent call last):
  ...
TypeError: 'tuple' object does not support item assignment
```

**Analogy:** It's like trying to erase a word on a printed photograph with a pencil — the paper simply won't let you "edit" it. Python throws an error instead of allowing the change.

---

## 5. The Sneaky Exception: Mutable Items Inside a Tuple

Here's where it gets interesting. A tuple itself is sealed — but if one of the items *inside* the tuple is a **list**, that inner list is still changeable!

```python
tup2 = (34, "shehin", [3, 4, 6])
```

Trying to replace the tuple's own slot still fails:

```python
tup2[0] = 350
```
```
Traceback (most recent call last):
  ...
TypeError: 'tuple' object does not support item assignment
```

But reaching *inside* the list that lives inside the tuple works fine:

```python
tup2[2][0] = 1
tup2
```
```
(34, 'shehin', [1, 4, 6])
```

**Analogy:** Picture a sealed gift box (the tuple) that contains a notepad (the list) among its items. You can't swap out what's *in the box* — you can't remove the notepad and put a different item in its place. But the notepad itself still has blank pages, and you're free to scribble on those pages whenever you like. The box's *contents list* is locked; what an item *does internally* is a separate matter.

---

## 6. Checking Membership with `in`

You can check whether a value exists inside a tuple using the `in` keyword:

```python
34 in tup2
```
```
True
```
```python
1 in tup2
```
```
False
```

**Analogy:** This is like patting down the sealed box from outside and asking, "Is there a coin in here?" — you get a simple `True` or `False` without needing to open or change anything.

(Note: `1 in tup2` is `False` here even though we just set the inner list's first item to `1`. That's because `in` checks the tuple's direct items — `34`, `'shehin'`, and `[1, 4, 6]` — not the items buried inside the nested list.)

---

## 7. Tuple Unpacking — Assigning Multiple Variables at Once

One of the most elegant tuple tricks is **unpacking**: assigning each item in a tuple to its own variable, all in a single line.

```python
tup1 = (1, "shafma", 7.9)
num, name, num1 = tup1

num
```
```
1
```
```python
name
```
```
'shafma'
```
```python
num1
```
```
7.9
```

**Analogy:** Imagine a sealed box with 3 compartments, each holding a different gift. Unpacking is like opening the box once and handing each compartment's gift directly to a specific person — person "num" gets compartment 1, "name" gets compartment 2, and so on — all in one motion, instead of reaching in three separate times.

> ⚠️ The number of variables on the left **must exactly match** the number of items in the tuple, or Python will throw a `ValueError`.

---

## 8. Quick Reference Cheat Sheet

| Operation | Code | What it does |
|---|---|---|
| Create | `tup = (1, 2, 3)` | Make a new tuple |
| Create (no brackets) | `tup = 1, 2, 3` | Commas alone define a tuple |
| Single-item tuple | `tup = (5,)` | Trailing comma is required |
| Access by index | `tup[0]` | Get item at position 0 |
| Smallest/Largest | `min(tup)` / `max(tup)` | Extreme values |
| Total | `sum(tup)` | Add all numbers |
| Check membership | `x in tup` | Returns `True`/`False` |
| Unpack | `a, b, c = tup` | Assign each item to a variable |
| Modify | ❌ Not allowed | Tuples are immutable |
| Modify a *nested* list | `tup[i][j] = x` | Allowed — the inner list is still mutable |

---

## 9. List vs Tuple: Side-by-Side

| Feature | List `[ ]` | Tuple `( )` |
|---|---|---|
| Can change after creation? | ✅ Yes (mutable) | ❌ No (immutable) |
| Syntax | Square brackets | Round brackets (or just commas) |
| Use case | Data that will grow/shrink/change | Data that should stay fixed (e.g. coordinates, a date, RGB color values) |
| Speed | Slightly slower | Slightly faster (since Python doesn't need to track changes) |
| Real-world analogy | Whiteboard / shopping cart | Printed photo / sealed gift box |

**Why would you ever want something that *can't* change?** Because sometimes that's exactly the safety you want. If you're storing something like a person's date of birth `(1995, 8, 23)`, you *want* a guarantee that no part of your program can accidentally overwrite it later. Tuples give you that guarantee for free.

---

## 10. Try It Yourself

1. Create a tuple with your name, age, and city.
2. Unpack it into 3 separate variables and print each one.
3. Try changing one value directly (e.g. `mytuple[0] = "new value"`) and observe the error.
4. Create a tuple that contains a list as one of its items, then modify only that inner list.
5. Use `in` to check if a particular value exists in your tuple.

---

**Previous:** [← Lists](./listIntro.md)
 
