# Dictionary in Python 🔑

> Part of the *Python from Beginner to Advanced* series.

---

## 1. What is a Dictionary?

Imagine your **phone's contact list**. You don't remember your friend Sarah's number by its position ("the 5th number in my phone"). Instead, you look her up by **name**, and her **number** shows up.

> *Name (key) → Number (value)*

That's exactly what a Python dictionary is: a collection of **key-value pairs**, where each key works like a label you use to instantly find its value — no searching, no counting positions.

```python
contact = {"sarah": 98765, "john": 87654, "mike": 76543}
```

Here:
- `"sarah"`, `"john"`, `"mike"` → **keys** (the labels)
- `98765`, `87654`, `76543` → **values** (the actual data)

---

## 2. Why do we need a dictionary when we already have List, Tuple, and Set?

Let's compare them using a real-world analogy:

| Data Type | Real-world analogy | How you find data |
|---|---|---|
| **List** | A train, where seats are numbered 0, 1, 2, 3... | By **position** (index) |
| **Tuple** | A locked train (like a List, but seats can't be rearranged) | By **position** (index) |
| **Set** | A pile of unique coins thrown in a bag (no order, no duplicates) | You only know **what's inside**, not arrangement |
| **Dictionary** | A contact list / a locker with name tags | By a **meaningful label (key)**, not position |

So if you wrote a list like this:

```python
student = ["Aisha", 21, "Computer Science"]
```

...how would you remember that `student[1]` is the age and not, say, the year of admission? You'd have to memorize positions. That gets messy fast.

A dictionary solves this by letting you **name** each piece of data:

```python
student = {"name": "Aisha", "age": 21, "course": "Computer Science"}
print(student["age"])   # 21 — instantly clear what this means
```

**In short:** Lists/Tuples are great when *order* matters. Sets are great when you just care about *uniqueness*. Dictionaries are great when you want to **label your data so it's self-explanatory and instantly searchable.**

---

## 3. Creating a Dictionary

A dictionary is written using curly braces `{}`, with each pair written as `key: value`.

```python
scores = {0: 45, 1: 88, 4: 67}
scores
```
```
{0: 45, 1: 88, 4: 67}
```

Notice the keys here are numbers (`0, 1, 4`) — not necessarily continuous, unlike list indexes!

---

## 4. Accessing Values

You access a value by placing its **key** inside square brackets.

```python
scores[4]
```
```
67
```

But what if you ask for a key that doesn't exist?

```python
scores[3]
```
```
Traceback (most recent call last):
  File "<pyshell#1>", line 1, in <module>
    scores[3]
KeyError: 3
```

🧠 **Analogy:** This is like trying to call a contact named "3" that was never saved in your phone. Python says: *"I don't know anyone called that"* → `KeyError`.

### A common mistake: mismatched brackets

```python
movies = {"action": 1, "comedy": 2, "drama": 3}
movies["action"}
```
```
SyntaxError: closing parenthesis '}' does not match opening parenthesis '['
```

Always remember: dictionaries use `{}` to **create**, but `[]` to **access** a value.

```python
movies["action"]
```
```
1
```

If you ask for a key using the wrong "label" — for instance, a position instead of the actual key — you'll still get a `KeyError`:

```python
movies[1]
```
```
Traceback (most recent call last):
  File "<pyshell#2>", line 1, in <module>
    movies[1]
KeyError: 1
```

Even though `1` is a *value* in the dictionary, Python only searches **keys**, not values, when you use `[]`.

```python
movies["drama"]
```
```
3
```

📌 **Rule of thumb:** Keys must be unique and unchangeable (like strings, numbers, or tuples). Values can repeat and can be anything — even lists, dictionaries, or sets.

---

## 5. The Safer Way to Access Values: `.get()`

Constantly worrying about `KeyError` is annoying. That's where `.get()` comes to the rescue.

🧠 **Analogy:** Using `[]` is like shouting a name into a crowd and getting upset if no one answers. Using `.get()` is like politely asking the receptionist, *"Is John here? If not, just tell me he's not, don't yell at me."*

```python
print(movies.get(1))
```
```
None
```

No error this time — just a calm "I couldn't find that" reply (`None`).

You can also customize that "I couldn't find that" message:

```python
movies.get("comedy", "not found")
```
```
2
```

```python
movies.get("horror", "not found")
```
```
'not found'
```

---

## 6. What happens with duplicate keys?

```python
movies = {"action": 1, "comedy": 2, "drama": 3, "action": 11, "comedy": 33}
movies
```
```
{'action': 11, 'comedy': 33, 'drama': 3}
```

🧠 **Analogy:** Imagine saving a contact named "John" twice with two different numbers. Your phone doesn't keep both — it just **overwrites** the old number with the latest one. Same with dictionaries: **the last value wins**, and duplicate keys are not allowed to coexist.

---

## 7. Building a Dictionary with `zip()`

Suppose you have two separate lists — one with names, one with marks — and you want to pair them up.

```python
names = {"neha", "ravi", "tom"}
marks = [85, 92, 78]

result = dict(zip(names, marks))
result
```
```
{'tom': 85, 'ravi': 92, 'neha': 78}
```

🧠 **Analogy:** `zip()` is like a stapler that joins two stacks of paper side by side — one name, one mark, one name, one mark — and `dict()` then turns each stapled pair into a key-value entry.

⚠️ **Careful:** Since `names` here is a **set**, and sets don't preserve order, the pairing with `marks` may not come out the way you expect. It's safer to use a list or tuple instead of a set when order matters:

```python
names = ["neha", "ravi", "tom"]
result = dict(zip(names, marks))
result
```
```
{'neha': 85, 'ravi': 92, 'tom': 78}
```

---

## 8. Removing Items with `.pop()`

```python
result.pop("neha")
```
```
85
```

```python
result
```
```
{'ravi': 92, 'tom': 78}
```

If the key doesn't exist:

```python
result.pop("neha")
```
```
Traceback (most recent call last):
  File "<pyshell#3>", line 1, in <module>
    result.pop("neha")
KeyError: 'neha'
```

🧠 **Analogy:** `.pop()` is like removing a contact from your phone *and* asking it to read out the number before deleting. If the contact never existed, your phone has nothing to read out, so it complains.

💡 **Tip:** Just like `.get()`, `.pop()` also accepts a default value to avoid errors:

```python
result.pop("neha", "already removed")
```
```
'already removed'
```

---

## 9. Other handy ways to remove things

```python
del result["tom"]
result
```
```
{'ravi': 92}
```

`del` simply deletes a key-value pair — no return value, unlike `.pop()`.

```python
result.clear()
result
```
```
{}
```

`.clear()` empties out the whole dictionary — like wiping your contact list clean.

---

## 10. Nested Dictionaries (Dictionaries inside Dictionaries)

Real-world data is rarely flat. Think of a **company directory**: each department has its own tools, and some departments have sub-teams with their own info.

```python
company = {
    "design": "figma",
    "backend": ["python", "django"],
    "frontend": {"library": "react", "version": "18"}
}

company
```
```
{'design': 'figma', 'backend': ['python', 'django'], 'frontend': {'library': 'react', 'version': '18'}}
```

```python
company["design"]
```
```
'figma'
```

```python
company["frontend"]
```
```
{'library': 'react', 'version': '18'}
```

To dig deeper, just chain the keys:

```python
company["frontend"]["version"]
```
```
'18'
```

🧠 **Analogy:** This is like a filing cabinet (`company`) with drawers (`frontend`), and inside one drawer there's a smaller labeled box (`version`) holding the value you want. You just keep opening one container at a time.

---

## 11. Checking if a key exists — the `in` keyword

Instead of risking a `KeyError`, you can simply ask Python first:

```python
"design" in company
```
```
True
```

```python
"marketing" in company
```
```
False
```

🧠 **Analogy:** This is like checking the index of a book before flipping to a chapter — "Is this topic even in here?" — instead of randomly flipping pages and being disappointed.

---

## 12. Updating a Dictionary

```python
company.update({"marketing": "canva", "design": "figma & photoshop"})
company
```
```
{'design': 'figma & photoshop', 'backend': ['python', 'django'], 'frontend': {'library': 'react', 'version': '18'}, 'marketing': 'canva'}
```

`.update()` adds new keys and **overwrites** existing ones — just like updating a contact's number while adding a brand-new contact at the same time.

---

## 13. Looping Through a Dictionary

```python
for department in company:
    print(department)
```
```
design
backend
frontend
marketing
```

By default, looping over a dictionary gives you the **keys**. To get keys and values together:

```python
for department, tool in company.items():
    print(department, "->", tool)
```
```
design -> figma & photoshop
backend -> ['python', 'django']
frontend -> {'library': 'react', 'version': '18'}
marketing -> canva
```

There are three useful "views" of a dictionary:

```python
company.keys()      # all the keys
company.values()    # all the values
company.items()     # all key-value pairs (as tuples)
```

---

## 14. Dictionary Comprehension (a quick, advanced shortcut)

Just like list comprehensions, you can build dictionaries in a single line.

```python
squares = {num: num ** 2 for num in range(1, 6)}
squares
```
```
{1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
```

🧠 **Analogy:** It's like an assembly line — for every item passing through, instantly create a labeled box (`key`) with its processed result inside (`value`).

---

## 15. Merging Two Dictionaries (Python 3.9+)

```python
defaults = {"theme": "light", "language": "english"}
user_prefs = {"language": "spanish"}

final_settings = defaults | user_prefs
final_settings
```
```
{'theme': 'light', 'language': 'spanish'}
```

🧠 **Analogy:** Think of `defaults` as the factory settings of your phone, and `user_prefs` as the changes you made. Merging them gives you the final settings — your custom choices **override** the defaults wherever they overlap.

---

## 16. Quick Recap

| Operation | What it does | Real-world analogy |
|---|---|---|
| `d[key]` | Access a value (errors if missing) | Calling a saved contact |
| `d.get(key, default)` | Access safely | Asking a receptionist politely |
| `d[key] = value` | Add/update a pair | Saving or editing a contact |
| `d.pop(key)` | Remove and return value | Deleting a contact and noting their number |
| `del d[key]` | Remove a pair | Deleting a contact, no questions asked |
| `d.update(other)` | Merge/overwrite pairs | Importing new contacts, overwriting duplicates |
| `key in d` | Check existence | Checking the index of a book |
| `d.keys() / .values() / .items()` | View parts of the dictionary | Viewing just names, just numbers, or both |
| `{k: v for ...}` | Build dynamically | An assembly line creating labeled boxes |
| `d1 \| d2` | Merge two dictionaries | Combining factory settings with your preferences |

---

### 🔑 Key Takeaways
- A dictionary stores data as **key-value pairs**, like a contact list.
- Keys must be **unique**; if you reuse a key, the latest value wins.
- Use `.get()` instead of `[]` when you're not sure a key exists.
- Dictionaries can be **nested** to represent complex, real-world data.
- Choose a dictionary over a list/tuple/set whenever you want your data to be **labeled and instantly searchable** rather than just ordered or unique.

---

*Next up:*
