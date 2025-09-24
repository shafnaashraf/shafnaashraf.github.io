# The `this` Keyword in Java

## Overview
The `this` keyword is a reference variable that refers to the current object instance. It's commonly used to resolve naming conflicts and for constructor chaining.

## 1. Basic Usage of `this` Keyword

### Problem: Variable Name Conflict

When a method parameter has the same name as an instance variable, which one gets changed?

```java
public class Person {
    private int age;  // Instance variable
    
    public void setAge(int age) {  // Parameter has same name
        age = age;  // This doesn't work! Parameter assigns to itself
    }
}
```

### Solution: Using `this` Keyword

```java
public class Person {
    private int age;  // Instance variable
    
    public void setAge(int age) {  // Parameter
        this.age = age;  // this.age refers to instance variable
                        // age refers to parameter
    }
    
    public int getAge() {
        return this.age;  // this is optional here, but good practice
    }
}
```

### Complete Example

```java
public class Person {
    private String name;
    private int age;
    
    // Constructor using this keyword
    public Person(String name, int age) {
        this.name = name;  // this.name = instance variable
        this.age = age;    // this.age = instance variable
    }
    
    // Setter methods using this keyword
    public void setName(String name) {
        this.name = name;
    }
    
    public void setAge(int age) {
        this.age = age;
    }
    
    // Getter methods
    public String getName() {
        return this.name;
    }
    
    public int getAge() {
        return this.age;
    }
    
    public void displayInfo() {
        System.out.println("Name: " + this.name + ", Age: " + this.age);
    }
}
```

## 2. `this` and `super` in Inheritance

### Basic Inheritance Example

```java
// Parent class
class A {
    public A() {
        System.out.println("Constructor of A executed");
    }
}

// Child class
class B extends A {
    public B() {
        System.out.println("Constructor of B executed");
    }
}

// Test class
public class Test {
    public static void main(String[] args) {
        System.out.println("Creating object of B:");
        B obj = new B();
    }
}
```

**Output:**
```
Creating object of B:
Constructor of A executed
Constructor of B executed
```

**Key Point:** Even though we only created an object of `B`, the constructor of `A` executes first because every constructor implicitly calls `super()`.

### Parameterized Constructor Example

```java
// Parent class with parameterized constructor
class A {
    public A() {
        System.out.println("Default constructor of A executed");
    }
    
    public A(int value) {
        System.out.println("Parameterized constructor of A executed with value: " + value);
    }
}

// Child class
class B extends A {
    public B(int value) {
        // super(); is implicitly called here (calls A's default constructor)
        System.out.println("Parameterized constructor of B executed with value: " + value);
    }
}

// Test class
public class Test {
    public static void main(String[] args) {
        System.out.println("Creating object of B with parameter:");
        B obj = new B(10);
    }
}
```

**Output:**
```
Creating object of B with parameter:
Default constructor of A executed
Parameterized constructor of B executed with value: 10
```

### Explicit `super()` Call

```java
// Parent class
class A {
    public A() {
        System.out.println("Default constructor of A executed");
    }
    
    public A(int value) {
        System.out.println("Parameterized constructor of A executed with value: " + value);
    }
}

// Child class with explicit super call
class B extends A {
    public B(int value) {
        super(20);  // Explicitly calling A's parameterized constructor
        System.out.println("Parameterized constructor of B executed with value: " + value);
    }
}

// Test class
public class Test {
    public static void main(String[] args) {
        System.out.println("Creating object of B with explicit super:");
        B obj = new B(10);
    }
}
```

**Output:**
```
Creating object of B with explicit super:
Parameterized constructor of A executed with value: 20
Parameterized constructor of B executed with value: 10
```

## 3. Constructor Chaining with `this()`

### Using `this()` to Call Another Constructor

```java
class B {
    private int value;
    private String name;
    
    // Default constructor
    public B() {
        this(0);  // Calls the single parameter constructor
        System.out.println("Default constructor of B executed");
    }
    
    // Single parameter constructor
    public B(int value) {
        this(value, "Default");  // Calls the two parameter constructor
        System.out.println("Single parameter constructor of B executed with value: " + value);
    }
    
    // Two parameter constructor
    public B(int value, String name) {
        this.value = value;
        this.name = name;
        System.out.println("Two parameter constructor of B executed with value: " + value + " and name: " + name);
    }
}

// Test class
public class Test {
    public static void main(String[] args) {
        System.out.println("Creating object using default constructor:");
        B obj1 = new B();
        
        System.out.println("\nCreating object using single parameter constructor:");
        B obj2 = new B(5);
        
        System.out.println("\nCreating object using two parameter constructor:");
        B obj3 = new B(10, "Custom");
    }
}
```

**Output:**
```
Creating object using default constructor:
Two parameter constructor of B executed with value: 0 and name: Default
Single parameter constructor of B executed with value: 0
Default constructor of B executed

Creating object using single parameter constructor:
Two parameter constructor of B executed with value: 5 and name: Default
Single parameter constructor of B executed with value: 5

Creating object using two parameter constructor:
Two parameter constructor of B executed with value: 10 and name: Custom
```

## 4. Complete Example: Inheritance + Constructor Chaining

```java
// Parent class
class A {
    protected int parentValue;
    
    public A() {
        this(100);  // Calls A's parameterized constructor
        System.out.println("Default constructor of A executed");
    }
    
    public A(int value) {
        this.parentValue = value;
        System.out.println("Parameterized constructor of A executed with value: " + value);
    }
}

// Child class
class B extends A {
    private int childValue;
    private String name;
    
    // Default constructor
    public B() {
        this(0, "Default");  // Calls B's two parameter constructor
        System.out.println("Default constructor of B executed");
    }
    
    // Single parameter constructor
    public B(int value) {
        super(value + 50);  // Calls A's parameterized constructor
        this.childValue = value;
        System.out.println("Single parameter constructor of B executed with value: " + value);
    }
    
    // Two parameter constructor
    public B(int value, String name) {
        super();  // Calls A's default constructor
        this.childValue = value;
        this.name = name;
        System.out.println("Two parameter constructor of B executed with value: " + value + " and name: " + name);
    }
    
    public void displayValues() {
        System.out.println("Parent value: " + this.parentValue + 
                          ", Child value: " + this.childValue + 
                          ", Name: " + this.name);
    }
}

// Test class
public class Test {
    public static void main(String[] args) {
        System.out.println("=== Creating B() ===");
        B obj1 = new B();
        obj1.displayValues();
        
        System.out.println("\n=== Creating B(25) ===");
        B obj2 = new B(25);
        obj2.displayValues();
        
        System.out.println("\n=== Creating B(15, \"Custom\") ===");
        B obj3 = new B(15, "Custom");
        obj3.displayValues();
    }
}
```

**Output:**
```
=== Creating B() ===
Parameterized constructor of A executed with value: 100
Default constructor of A executed
Two parameter constructor of B executed with value: 0 and name: Default
Default constructor of B executed
Parent value: 100, Child value: 0, Name: Default

=== Creating B(25) ===
Parameterized constructor of A executed with value: 75
Single parameter constructor of B executed with value: 25
Parent value: 75, Child value: 25, Name: null

=== Creating B(15, "Custom") ===
Parameterized constructor of A executed with value: 100
Default constructor of A executed
Two parameter constructor of B executed with value: 15 and name: Custom
Parent value: 100, Child value: 15, Name: Custom
```

## 5. Important Rules and Concepts

### Object Class Inheritance
Every class in Java implicitly extends the `Object` class:

```java
class A {
    public A() {
        // Implicitly calls super(); which calls Object class constructor
        System.out.println("Constructor of A");
    }
}
```

### Constructor Chaining Rules

1. **`super()` or `super(parameters)` must be the first statement** in a constructor (if used)
2. **`this()` or `this(parameters)` must be the first statement** in a constructor (if used)
3. **You cannot use both `super()` and `this()`** in the same constructor
4. **If neither `super()` nor `this()` is explicitly called**, `super()` is automatically inserted

### Example Showing Rules

```java
class Parent {
    public Parent() {
        System.out.println("Parent default constructor");
    }
    
    public Parent(String msg) {
        System.out.println("Parent parameterized constructor: " + msg);
    }
}

class Child extends Parent {
    public Child() {
        // super(); is automatically inserted here
        System.out.println("Child default constructor");
    }
    
    public Child(int value) {
        this();  // Must be first statement - calls Child()
        System.out.println("Child parameterized constructor: " + value);
    }
    
    public Child(String msg) {
        super(msg);  // Must be first statement - calls Parent(String)
        System.out.println("Child string constructor: " + msg);
    }
}
```

## 6. Common Use Cases

### Method Chaining
```java
public class Person {
    private String name;
    private int age;
    
    public Person setName(String name) {
        this.name = name;
        return this;  // Returns current object for method chaining
    }
    
    public Person setAge(int age) {
        this.age = age;
        return this;
    }
    
    // Usage: Person p = new Person().setName("John").setAge(25);
}
```

### Passing Current Object
```java
public class Person {
    private String name;
    
    public void registerPerson() {
        Database.save(this);  // Passing current object
    }
}
```

## Key Takeaways

1. **`this` refers to the current object instance**
2. **Use `this.variableName` to resolve naming conflicts**
3. **`super()` is automatically called if not explicitly mentioned**
4. **`this()` enables constructor chaining within the same class**
5. **`super()` and `this()` must be the first statement in constructor**
6. **Every class implicitly extends Object class**
7. **Constructor execution follows inheritance hierarchy (parent first)**
