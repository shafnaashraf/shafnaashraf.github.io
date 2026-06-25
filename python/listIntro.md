---
title: "Python Lists - The Shopping Cart of Programming"
chapter: 4
level: beginner
---

# Python Lists: Your First Real Data Container

## 1. The Big Picture (For Non-Techies)

Imagine you go grocery shopping. You don't carry items in your hands one by one — you put them in a **shopping cart**. The cart can hold many different things: apples, milk, a magazine, even another smaller basket.

In Python, a **list** is that shopping cart. It's a single container that holds many values, in a specific order, and you can add, remove, rearrange, or look through them whenever you want.

Before lists, we only knew how to store **one value at a time**:

```python
num1 = 60
num1
```
```
60
```

This is like having a single sticky note with "60" written on it. Fine for one number, but what if you have 7 numbers? You'd need 7 sticky notes — messy and hard to manage. A list solves this by putting all of them in **one cart**.

---

## 2. Creating a List

A list is created using **square brackets `[ ]`**, with items separated by commas.

```python
nums = [1, 2, 3, 76, 34, 23, 345]
nums
```
```
[1, 2, 3, 76, 34, 23, 345]
```

Think of `nums` as the name tag on the shopping cart, and the numbers inside are the items in it.

You can make a list of anything — not just numbers:

```python
name = ['navin', 'harsh', 'kiran']
name
```
```
['navin', 'harsh', 'kiran']
```

Here the cart holds names instead of numbers. This is like a cart for a fruit shop versus one for a toy shop — same kind of cart, different items inside.

### A list can even hold mixed types

```python
mix = [1, 'd', 'fdad', 644]
mix
```
```
[1, 'd', 'fdad', 644]
```

**Analogy:** A real shopping cart doesn't care if you put apples, a phone, and a book in it together. Python lists are the same — they don't force every item to be the same "type."

---

## 3. Indexing - Picking One Item From the Cart

Every item in a list has a **position number**, called an **index**. Python starts counting from **0**, not 1 — this trips up almost every beginner, so let's nail it with an analogy.

**Analogy:** Think of a row of lockers in a school. The very first locker is labeled "Locker 0," not "Locker 1." So if you want the *first* locker, you ask for locker number 0.

```python
nums = [1, 2, 3, 76, 34, 23, 345]

nums[0]
```
```
1
```

```python
nums[4]
```
```
34
```

| Index | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|-------|---|---|---|---|---|---|---|
| Value | 1 | 2 | 3 | 76| 34| 23| 345|

### Negative indexing - counting from the back

Python lets you count backward too, starting at `-1` for the last item.

**Analogy:** Imagine standing at the back of a queue and counting people from there. The person right in front of you (the last one in line) is "-1," the one before them is "-2," and so on.

```python
nums[-1]
```
```
345
```

---

## 4. `len()` - How Many Items Are in the Cart?

```python
len(nums)
```
```
7
```

**Analogy:** This is like asking, "How many items did I put in my cart?" before going to checkout. Python counts every item and tells you the total.

---

## 5. Slicing - Taking Out a Section of the Cart

Sometimes you don't want just one item — you want a **chunk** of the list. This is called **slicing**, written as `list[start:stop]`.

```python
nums[:3]
```
```
[1, 2, 3]
```

```python
nums[2:]
```
```
[3, 76, 34, 23, 345]
```

**Analogy:** Imagine your shopping cart items are lined up on a conveyor belt at checkout.
- `nums[:3]` means "give me the first 3 items from the belt, starting from the front."
- `nums[2:]` means "skip the first 2 items, give me everything from the 3rd item onward."

A quick rule to remember: the **stop number is never included**. `nums[:3]` gives indexes 0, 1, 2 — not 3.

---

## 6. Lists Inside Lists (Nested Lists)

You can even put a cart **inside another cart**.

```python
nums = [1, 2, 3, 76, 34, 23, 345]
name = ['navin', 'harsh', 'kiran']

mix = [nums, name]
mix
```
```
[[1, 2, 3, 76, 34, 23, 345], ['navin', 'harsh', 'kiran']]
```

**Analogy:** Think of a big moving box that has two smaller boxes inside it — one box full of numbered tags, another full of name tags. The big box (`mix`) has only 2 items: "small box 1" and "small box 2."

To get inside the small box, you index twice:

```python
mix[0]
```
```
[1, 2, 3, 76, 34, 23, 345]
```

```python
mix[0][1]
```
```
2
```

This means: "Open the first small box (`mix[0]`), then grab the 2nd item inside it (`[1]`)."

```python
mix[1][2]
```
```
'kiran'
```

### Common mistake: using a comma instead of double brackets

```python
mix[0, 1]
```
```
Traceback (most recent call last):
  ...
TypeError: list indices must be integers or slices, not tuple
```

Python doesn't understand "row 0, column 1" like a spreadsheet would. You must open one box at a time: `mix[0][1]`, not `mix[0, 1]`.

---

## 7. Combining Lists with `+`

```python
mix = nums + name
mix
```
```
[1, 2, 3, 76, 34, 23, 345, 'navin', 'harsh', 'kiran']
```

**Analogy:** This is like pouring two separate carts into one big cart, one after another — not nested this time, just merged into a single flat list. Compare this with `[nums, name]` from before, which kept them as two separate boxes inside one box.

---

## 8. Modifying a List — Adding Items

### `append()` — add one item to the end

```python
nums.append(33)
nums
```
```
[1, 2, 3, 76, 34, 23, 345, 33]
```

**Analogy:** You're at the checkout line, and you remember one more item — you just toss it at the back of the cart.

### `insert(position, value)` — add an item at a specific spot

```python
nums.insert(1, 55)
nums
```
```
[1, 55, 2, 3, 76, 34, 23, 345, 33]
```

**Analogy:** Instead of adding to the back, you're squeezing a new item into a specific spot in the cart — like sneaking a candy bar in right next to the bread.

### `extend()` — add multiple items at once

```python
nums.extend([5, 35, 30])
nums
```
```
[1, 55, 2, 3, 76, 34, 23, 345, 33, 5, 35, 30]
```

**Analogy:** Instead of adding one item, you dump in a whole bag of new items at once.

> **append vs extend:** `append([5, 35, 30])` would add the whole `[5, 35, 30]` as **one single item** (a box inside the cart). `extend([5, 35, 30])` opens that box and adds each item individually. This is one of the most common beginner confusions!

---

## 9. Removing Items

### `remove(value)` — remove by value

```python
nums.remove(345)
```
Removes the **first** occurrence of `345` from the list.

**Analogy:** You search the cart for a specific item (say, an expired yogurt) and take it out — you don't care where it was sitting, just that it's gone.

### `pop(index)` — remove by position (and get it back!)

```python
nums.pop(7)
```
```
23
```

**Analogy:** You reach into a specific spot in the cart, pull out exactly that item, and it's handed to you — useful when you want to both remove *and* use the item.

### `del` — delete by position or by range

```python
del nums[2:5]
```

**Analogy:** Instead of removing one item, you grab a whole section of the cart (slice) and throw it out at once.

---

## 10. Searching and Counting

### `count(value)` — how many times does a value appear?

```python
nums.count(1)
```
```
1
```

**Analogy:** "How many cans of the exact same soda do I have in my cart?" Python counts matching items for you.

> Note: a list is not "callable" like a function. Writing `nums(76)` (using round brackets instead of square ones) throws an error:
> ```
> TypeError: 'list' object is not callable
> ```
> Round brackets `()` are for calling functions; square brackets `[]` are for accessing list items.

---

## 11. Reordering a List

### `reverse()` — flip the order

```python
nums.reverse()
```

**Analogy:** Imagine flipping your cart's conveyor belt direction — last item becomes first, first becomes last.

### `sort()` — arrange in order

```python
nums.sort()
```

**Analogy:** This is like a cashier neatly arranging all your items from smallest to largest before scanning them.

---

## 12. Built-in Helper Functions

These aren't list *methods* (you don't write `.something()`), but separate functions that *take* a list as input.

```python
min(nums)   # smallest value
max(nums)   # largest value
sum(nums)   # total of all values
```

**Analogy:** Think of these as a calculator sitting next to your cart. You hand it the whole cart, and it tells you the cheapest item (`min`), the most expensive item (`max`), or the total bill (`sum`).

These also work on lists of text, using alphabetical order:

```python
name = ['navin', 'harsh', 'kiran']
min(name)
```
```
'harsh'
```
```python
max(name)
```
```
'navin'
```

**Analogy:** Just like numbers are compared by size, words are compared like a dictionary lookup — whichever word would appear first/last alphabetically wins.

---

## 13. Replacing a Section with Slice Assignment

You're not limited to changing one item at a time — you can replace a whole chunk:

```python
nums[2:4] = [100, 74]
```

**Analogy:** Instead of swapping out one item from the cart, you remove a whole section and replace it with new items in one motion — like swapping out a whole shelf in your cart for a different set of products.

---

## 14. Quick Reference Cheat Sheet

| Operation | Code | What it does |
|---|---|---|
| Create | `nums = [1,2,3]` | Make a new list |
| Access by index | `nums[0]` | Get item at position 0 |
| Negative index | `nums[-1]` | Get last item |
| Length | `len(nums)` | Count items |
| Slice | `nums[1:3]` | Get a sub-section |
| Add to end | `nums.append(x)` | Add one item at the end |
| Add at position | `nums.insert(i, x)` | Insert item at index `i` |
| Add many | `nums.extend([..])` | Add multiple items |
| Remove by value | `nums.remove(x)` | Delete first match of `x` |
| Remove by index | `nums.pop(i)` | Delete & return item at `i` |
| Delete range | `del nums[i:j]` | Delete a slice |
| Count occurrences | `nums.count(x)` | How many times `x` appears |
| Reverse | `nums.reverse()` | Flip order |
| Sort | `nums.sort()` | Arrange ascending |
| Combine | `a + b` | Merge two lists into one |
| Smallest/Largest | `min(nums)` / `max(nums)` | Extreme values |
| Total | `sum(nums)` | Add all numbers |

---

## 15. Try It Yourself

1. Create a list of your 5 favorite movies.
2. Print the 1st and last movie using indexing.
3. Add a new movie using `append()`.
4. Remove one movie using `remove()`.
5. Sort the list alphabetically.
6. Print how many movies are left using `len()`.

---

**Next up:** [Tuples — The "Sealed Box" Version of a List →](#)
