# What is Django?

Welcome to the first chapter of this Django tutorial series! By the end of this page, you'll understand what Django is, why developers love it, and how to set up your very first Django project — even if you've never built a web app before.

---

## 🌐 What is a Framework?

Before we talk about Django, let's understand what a **framework** is.

Imagine you're building a house. You *could* start from absolute scratch — mixing cement, cutting timber, laying bricks one by one. Or, you could use a **pre-built structure**: a frame that already has walls, a roof, plumbing blueprints, and electrical wiring planned out. You just fill in the details.

A **web framework** is exactly that pre-built structure — but for building websites and web applications. Instead of writing every single piece of code yourself (handling logins, database connections, security, etc.), a framework gives you ready-made tools and a clear structure to work with.

> 💡 **Real-world analogy:** Think of a framework like a restaurant kitchen. The stoves, ovens, prep stations, and storage are already set up. You (the chef/developer) just bring the recipes and ingredients. Without the kitchen, you'd spend all your time building equipment instead of cooking food.

---

## 🐍 What is Django?

**Django is a free, open-source web framework written in Python.**

It was built to help developers create web applications *quickly* and *cleanly*, without reinventing the wheel every time. Django follows the philosophy:

> *"Don't repeat yourself."* (DRY)

This means Django comes with a lot of built-in tools — user authentication, database management, admin panels, form handling — so you don't have to build them from scratch.

Some well-known websites built with Django include **Instagram**, **Pinterest**, **Disqus**, and **Mozilla**.

---

## 🏗️ MVC vs MVT — How Django Organizes Your Code

When building any web application, things can get messy fast. Imagine dumping your HTML, database logic, and business rules all into a single file — it would be a nightmare to read, fix, or update.

This is why most frameworks use a design pattern called **MVC (Model-View-Controller)** to separate concerns and keep code organized.

| Layer | Responsibility |
|---|---|
| **Model** | Manages data and the database |
| **View** | Controls what the user sees (the UI) |
| **Controller** | Handles the logic between Model and View |

> 💡 **Real-world analogy:** Think of a restaurant again. The **kitchen** (Model) stores and prepares the food (data). The **waiter** (Controller) takes your order and coordinates between you and the kitchen. The **dining area** (View) is what you, the customer, actually see and interact with.

### Django's Version: MVT

Django uses a similar pattern called **MVT (Model-View-Template)**:

| Layer | Responsibility | Django Equivalent |
|---|---|---|
| **Model** | Defines the data structure | `models.py` |
| **View** | Contains the business logic | `views.py` |
| **Template** | Defines the HTML shown to the user | `.html` files |

Django itself acts as the "Controller" behind the scenes — routing requests to the right View automatically.

> 💡 **Think of it this way:** A user visits your website (sends a request) → Django routes it to the right **View** → the View fetches data from the **Model** → the data gets rendered into a **Template** → the user sees the final HTML page.

---

## ✅ Why Choose Django?

There are many web frameworks out there (Flask, FastAPI, Express, Rails…), so why pick Django? Here are the top reasons:

### ⚡ Fast to Build With
Django comes with so many built-in features that you can go from idea to working prototype in hours, not days. Features like an automatic admin panel, user authentication, and ORM (Object-Relational Mapping for databases) are ready out of the box.

### ⚙️ Easy Configuration
Django follows *convention over configuration* — meaning it makes sensible decisions for you by default. You don't need to configure dozens of settings just to get started.

### 🧩 Rich Ecosystem of Components
Need to add payments? There's a Django package for that. Need REST APIs? There's Django REST Framework. Need image uploads, social login, or search? The Django ecosystem has you covered with thousands of reusable components.

### 🔒 Security First
Django handles many common security vulnerabilities *automatically*, including:
- SQL Injection
- Cross-Site Scripting (XSS)
- Cross-Site Request Forgery (CSRF)
- Clickjacking

You get a secure foundation without having to be a security expert.

### 📈 Scalable
Django is used by **Instagram** — one of the most visited websites in the world. It's built to scale, whether your app has 10 users or 10 million.

---

## 🛠️ Setting Up Your First Django Project

Let's get hands-on! Here's how to create your first Django project from scratch.

### Step 1 — Install Django

First, make sure you have Python installed. Then install Django using pip:

```bash
pip install django
```

### Step 2 — Create a New Project

Django comes with a command-line tool called **`django-admin`**. Use it to scaffold a new project:

```bash
django-admin startproject project1
```

This creates a folder called `project1` with the following structure:

```
project1/
├── manage.py
└── project1/
    ├── __init__.py
    ├── settings.py
    ├── urls.py
    ├── asgi.py
    └── wsgi.py
```

Let's understand what each file does:

| File | Purpose |
|---|---|
| `manage.py` | A command-line utility to interact with your Django project (run server, run migrations, etc.) |
| `__init__.py` | Tells Python that this folder is a Python package. You usually don't touch this file. |
| `settings.py` | The main configuration file for your project (database, installed apps, timezone, etc.) |
| `urls.py` | The URL dispatcher — maps URLs to the right views |
| `asgi.py` / `wsgi.py` | Entry points for deploying your app to a web server |

> 💡 **Analogy for `manage.py`:** Think of it as the **control panel** of your project. Want to start the server? Use `manage.py`. Want to create database tables? Use `manage.py`. It's your go-to tool for almost everything.

### Step 3 — Run the Development Server

Django includes a **lightweight built-in web server** — perfect for development. You don't need to install Apache or Nginx just to test your app locally.

Navigate into your project folder and run:

```bash
cd project1
python manage.py runserver
```

You'll see output like this in your terminal:

```
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).
May 13, 2026 - 10:00:00
Django version 5.x, using settings 'project1.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

### Step 4 — See Your App in the Browser

Open your browser and go to the address shown in the terminal:

```
http://127.0.0.1:8000/
```

You'll be greeted by Django's default success page:

![Django success page — "The install worked successfully! Congratulations!"](https://static.djangoproject.com/img/install-success.png)

🎉 **Congratulations — Django is running!**

> 💡 **What is `127.0.0.1:8000`?** This is your **local machine's address**. `127.0.0.1` always refers to *your own computer* (called "localhost"), and `8000` is the port Django listens on. This server is only visible on your machine — not the internet.

---

## 🔁 Quick Recap

| Concept | Summary |
|---|---|
| **Framework** | A pre-built structure for building apps faster |
| **Django** | A Python web framework — batteries included |
| **MVT** | Model (data), View (logic), Template (HTML) — Django's way of organizing code |
| **Why Django** | Fast, secure, scalable, rich ecosystem |
| **`django-admin startproject`** | Scaffolds a new Django project with all necessary files |
| **`manage.py runserver`** | Starts a local development server to preview your app |

---

## ⏭️ What's Next?

Now that Django is up and running, in the next chapter we'll:
- Understand **Django Apps** (yes, a project can have multiple apps!)
- Create your **first web page** with a View and Template
- Learn how URLs connect to Views

Stay tuned — it gets more exciting from here! 🚀

---

*This tutorial is part of the **Django: Beginner to Advanced** series.*
