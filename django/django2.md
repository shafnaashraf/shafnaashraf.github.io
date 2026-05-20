# Django Basics: URLs, Views & Your First App

> **Series:** Django From Beginner to Advanced  
> **Topic:** How Django Handles Requests — URLs, Views & Apps Explained

---

## 🌐 Static Pages vs Dynamic Pages

Before diving into Django, let's understand what kind of pages exist on the web.

| Type | Description | Example |
|------|-------------|---------|
| **Static Page** | Same content for everyone, doesn't change | A company's "About Us" page |
| **Dynamic Page** | Content changes based on user, time, or data | Your Facebook feed, an e-commerce product page |

Django is built to power **dynamic pages** — content that changes based on what the user does or requests.

---

## 🏗️ Projects vs Apps — The City & Buildings Analogy

Think of a Django **project** as a **city**.

> A city has shared infrastructure — roads, electricity, a city hall — that everything depends on.

An **app** inside that project is like a **building** in that city — a bank, a hospital, a school. Each building has its own purpose, but they all share the city's infrastructure.

- The **project** holds global settings (`settings.py`, `urls.py`) — like the city's central plan.
- Each **app** is a self-contained module — login system, calculator, blog, store.

This makes your code **modular and reusable**. You can even take the "login" building from one city and drop it into another!

---

## 🛠️ Creating Your First App

```bash
python manage.py startapp calc
```

This creates a folder called `calc` with these files:

```
calc/
├── admin.py       # Register models for the admin panel
├── apps.py        # App configuration
├── models.py      # Your data structure (database tables)
├── tests.py       # Write tests for your app
├── views.py       # What the user sees (the logic layer)
└── migrations/    # Database change history
```

> 💡 Notice: **no `settings.py`** inside the app! That lives in the project folder because settings apply to the *whole city*, not just one building.

---

## 📦 Key Files at a Glance

### `models.py` — The Blueprint (Data)

Django uses something called **ORM (Object-Relational Mapper)**. Instead of writing raw SQL, you define your data as Python classes.

> Think of `models.py` as the **architect's blueprint** — it describes what data your app stores (users, products, orders, etc.)

### `views.py` — The Waiter (Logic)

The view is the middleman between data and the user.

> Think of a **restaurant**: the customer (browser) places an order (URL request) → the waiter (`views.py`) takes the order to the kitchen → brings back the food (response/page).

### `tests.py` — Quality Control

Write tests to make sure your code behaves as expected. Especially useful as your project grows.

---

## 🔄 How Django Handles a Request — Step by Step

When a user types a URL like `http://mysite.com/` in their browser:

```
Browser hits URL
      ↓
Django checks urls.py (project level)
      ↓
Matches a pattern → routes to app's urls.py
      ↓
App's urls.py maps to a view function
      ↓
View function runs and returns a response
      ↓
User sees the page
```

> Real-world analogy: It's like calling a company's main number → the receptionist transfers you to the right department → the department answers your question.

---

## 👋 Your First View: Hello World

### Step 1 — Write the view in `calc/views.py`

```python
from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.
def home(request):
    return HttpResponse("Hello World")
```

**What's happening here?**

- `request` — Django passes this automatically. It contains everything about the incoming browser request (who sent it, what they asked for, etc.)
- `HttpResponse("Hello World")` — We're sending back a simple text response. Think of it as handing the customer their order.

---

### Step 2 — Create `calc/urls.py` (the app's receptionist)

Your app doesn't have a `urls.py` by default — you create it:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home')
]
```

**Breaking it down:**

- `path('')` — An empty string means the **root URL** (`/`). If someone visits the homepage, match this.
- `views.home` — Run the `home` function from `views.py`.
- `name='home'` — A nickname for this URL, useful for linking to it elsewhere in your code.

---

### Step 3 — Include your app's URLs in the project's `urls.py`

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path("admin/", admin.site.urls),
    path('', include('calc.urls'))   # 👈 Add this line
]
```

**What does `include` do?**

> Think of the project's `urls.py` as the **city directory**. It doesn't handle every single address itself — it says: *"For anything starting with '' (root), go ask the `calc` app's directory."*

`include('calc.urls')` delegates URL matching to the `calc` app.

---

## 🗺️ The Full Picture

```
User visits: http://mysite.com/

Project urls.py:
  path('', include('calc.urls'))  ← matches!
        ↓
calc/urls.py:
  path('', views.home, name='home')  ← matches!
        ↓
calc/views.py:
  def home(request):
      return HttpResponse("Hello World")  ← runs!
        ↓
Browser shows: Hello World ✅
```

---

## ✅ Summary

| Concept | What It Does | Real-World Analogy |
|--------|-------------|-------------------|
| **Project** | The big container holding everything | A city |
| **App** | A self-contained module | A building in the city |
| **`views.py`** | Handles logic, returns a response | A waiter in a restaurant |
| **`urls.py` (project)** | Main routing — delegates to apps | City directory / receptionist |
| **`urls.py` (app)** | App-level routing | Department extensions |
| **`models.py`** | Defines data structure | Architect's blueprint |
| **ORM** | Python interface to the database | A translator between Python and SQL |

---

## 🔜 What's Next?

In the next part, we'll:
- Connect our view to an **HTML template** (instead of plain text)
- Learn how Django's **template engine** works
- Build a proper homepage with real HTML

---

*Found this helpful? ⭐ Star the repo and share it with fellow Django learners!*
