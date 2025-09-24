# Java Static Keyword - Complete Tutorial

## Table of Contents
- [Introduction](#introduction)
- [Static Variables](#static-variables)
- [Static Methods](#static-methods)
- [Static Blocks](#static-blocks)
- [Static Classes (Nested)](#static-classes-nested)
- [Class Loading and Static Context](#class-loading-and-static-context)
- [Best Practices](#best-practices)
- [Common Interview Questions](#common-interview-questions)

## Introduction

The `static` keyword in Java is a powerful modifier that makes a member (variable, method, or nested class) belong to the class rather than to any specific instance of the class. This means static members are shared among all instances and can be accessed without creating an object.

## Static Variables

### What are Static Variables?

Static variables, also known as class variables, are shared among all instances of a class. They are initialized only once when the class is first loaded.

```java
class Mobile {
    String brand;           // Instance variable
    static String name;     // Static variable
    
    public Mobile(String brand) {
        this.brand = brand;
    }
}
```

### Example: Understanding Static Variables

```java
public class StaticVariableExample {
    public static void main(String[] args) {
        Mobile obj1 = new Mobile("Samsung");
        Mobile obj2 = new Mobile("iPhone");
        
        // Setting static variable through object (not recommended)
        obj1.name = "Smartphone";
        
        // This will print "Smartphone" for both objects
        System.out.println("obj1 name: " + obj1.name); // Smartphone
        System.out.println("obj2 name: " + obj2.name); // Smartphone
        
        // Recommended way: Access static variable using class name
        Mobile.name = "Mobile Device";
        System.out.println("Updated name: " + Mobile.name); // Mobile Device
    }
}
```

### Key Points about Static Variables:
- Shared among all instances of the class
- Memory allocated only once when class is loaded
- Should be accessed using class name, not object reference
- Initialized when class is first loaded
- Also called class variables

## Static Methods

### Definition and Usage

Static methods belong to the class rather than any instance. They can be called without creating an object of the class.

```java
class MobileUtils {
    static String companyName = "TechCorp";
    
    // Static method
    public static void show(String deviceName) {
        System.out.println("Device: " + deviceName);
        System.out.println("Company: " + companyName); // Can access static variables
        
        // System.out.println(brand); // ERROR: Cannot access non-static variable
    }
    
    // Static method with object parameter
    public static void displayMobileInfo(Mobile mobile) {
        System.out.println("Brand: " + mobile.brand); // Can access through object parameter
    }
}
```

### Restrictions of Static Methods:
1. **Cannot access non-static variables directly**
2. **Cannot access non-static methods directly**
3. **Cannot use `this` or `super` keywords**
4. **Cannot be overridden** (but can be hidden)

```java
public class StaticMethodExample {
    static int staticVar = 100;
    int instanceVar = 200;
    
    public static void staticMethod() {
        System.out.println("Static variable: " + staticVar); // ✓ Valid
        // System.out.println("Instance variable: " + instanceVar); // ✗ Error
        
        staticMethod2(); // ✓ Valid - calling another static method
        // instanceMethod(); // ✗ Error - cannot call non-static method
    }
    
    public static void staticMethod2() {
        System.out.println("Another static method");
    }
    
    public void instanceMethod() {
        System.out.println("Instance method");
        System.out.println("Static variable: " + staticVar); // ✓ Valid
        System.out.println("Instance variable: " + instanceVar); // ✓ Valid
        staticMethod(); // ✓ Valid - can call static method from instance method
    }
}
```

### Why is the main() method static?

The `main()` method is static because:
- JVM needs to call it without creating an object
- It serves as the entry point of program execution
- No object exists when the program starts

```java
public class MainMethodExample {
    public static void main(String[] args) {
        System.out.println("Program starts here!");
        // No object creation needed to execute this method
    }
}
```

## Static Blocks

### What are Static Blocks?

Static blocks are used to initialize static variables and are executed when the class is first loaded, before any object is created or any static method is called.

```java
class Mobile {
    static String name;
    static String manufacturer;
    
    // Static block
    static {
        name = "Smartphone";
        manufacturer = "Unknown";
        System.out.println("Static block executed!");
    }
    
    // Constructor
    public Mobile() {
        System.out.println("Constructor called!");
    }
}
```

### Example: Static Block vs Constructor

```java
public class StaticBlockExample {
    public static void main(String[] args) {
        System.out.println("Creating first object:");
        Mobile mobile1 = new Mobile(); // Static block executes first
        
        System.out.println("\nCreating second object:");
        Mobile mobile2 = new Mobile(); // Only constructor executes
    }
}
```

**Output:**
```
Creating first object:
Static block executed!
Constructor called!

Creating second object:
Constructor called!
```

### Multiple Static Blocks

```java
class Example {
    static int a;
    static int b;
    
    // First static block
    static {
        a = 10;
        System.out.println("First static block");
    }
    
    // Second static block
    static {
        b = 20;
        System.out.println("Second static block");
    }
    
    // Static blocks execute in the order they appear
}
```

## Class Loading and Static Context

### When is a Class Loaded?

A class is loaded by the ClassLoader when:
1. First object of the class is created
2. First static method is called
3. First static variable is accessed
4. Class is explicitly loaded using `Class.forName()`

### Force Class Loading

```java
public class ClassLoadingExample {
    public static void main(String[] args) {
        try {
            // Force class loading without creating object
            Class.forName("Mobile");
            System.out.println("Class loaded successfully!");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

### Class Loading Sequence

1. **Loading**: Class bytecode is loaded into memory
2. **Linking**: Verification, preparation, and resolution
3. **Initialization**: Static variables are initialized and static blocks are executed

```java
class LoadingDemo {
    static int value = initializeValue();
    
    static {
        System.out.println("Static block executed");
    }
    
    static int initializeValue() {
        System.out.println("Static variable initialized");
        return 42;
    }
    
    public LoadingDemo() {
        System.out.println("Constructor called");
    }
}
```

## Static Classes (Nested)

Only nested classes can be static in Java. A static nested class doesn't need a reference to the outer class.

```java
public class OuterClass {
    static int outerStaticVar = 10;
    int outerInstanceVar = 20;
    
    // Static nested class
    static class StaticNestedClass {
        void display() {
            System.out.println("Outer static variable: " + outerStaticVar); // ✓ Valid
            // System.out.println("Outer instance variable: " + outerInstanceVar); // ✗ Error
        }
    }
    
    // Non-static nested class (Inner class)
    class InnerClass {
        void display() {
            System.out.println("Outer static variable: " + outerStaticVar); // ✓ Valid
            System.out.println("Outer instance variable: " + outerInstanceVar); // ✓ Valid
        }
    }
}
```

**Usage:**
```java
public class NestedClassExample {
    public static void main(String[] args) {
        // Static nested class - no outer class instance needed
        OuterClass.StaticNestedClass staticNested = new OuterClass.StaticNestedClass();
        staticNested.display();
        
        // Inner class - outer class instance needed
        OuterClass outer = new OuterClass();
        OuterClass.InnerClass inner = outer.new InnerClass();
        inner.display();
    }
}
```

## Best Practices

### 1. Use Static for Utility Methods

```java
public class MathUtils {
    // Utility methods should be static
    public static int add(int a, int b) {
        return a + b;
    }
    
    public static double calculateCircleArea(double radius) {
        return Math.PI * radius * radius;
    }
}
```

### 2. Use Static for Constants

```java
public class Constants {
    public static final String APPLICATION_NAME = "MyApp";
    public static final int MAX_USERS = 1000;
    public static final double PI = 3.14159;
}
```

### 3. Singleton Pattern with Static

```java
public class Singleton {
    private static Singleton instance;
    
    private Singleton() {
        // Private constructor
    }
    
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

### 4. Static Import

```java
import static java.lang.Math.*;
import static java.lang.System.out;

public class StaticImportExample {
    public static void main(String[] args) {
        out.println("Square root of 16: " + sqrt(16)); // No need for Math.sqrt()
        out.println("Value of PI: " + PI); // No need for Math.PI
    }
}
```

## Common Interview Questions

### Q1: Can we override static methods?
**Answer:** No, static methods cannot be overridden. They can be hidden by defining a method with the same signature in a subclass.

```java
class Parent {
    static void display() {
        System.out.println("Parent static method");
    }
}

class Child extends Parent {
    static void display() { // This hides parent's method, doesn't override
        System.out.println("Child static method");
    }
}
```

### Q2: Can we have static and non-static methods with the same name?
**Answer:** Yes, but they must have different parameters (method overloading).

```java
class Example {
    static void method(int x) {
        System.out.println("Static method: " + x);
    }
    
    void method(String s) {
        System.out.println("Non-static method: " + s);
    }
}
```

### Q3: What happens if we don't initialize a static variable?
**Answer:** Static variables get default values (0 for numbers, null for objects, false for boolean).

### Q4: Can constructors be static?
**Answer:** No, constructors cannot be static because they are meant to initialize instance variables.

### Q5: Memory allocation for static variables?
**Answer:** Static variables are stored in the Method Area (Metaspace in Java 8+) and are allocated memory when the class is first loaded.

## Summary

- **Static variables** belong to the class, not instances
- **Static methods** can be called without creating objects
- **Static blocks** execute once when class is loaded
- **Static members** are initialized during class loading
- **Static methods** cannot access non-static members directly
- **main() method** is static to serve as program entry point
- Use static for utility methods, constants, and shared data
- Static nested classes don't need outer class reference

Understanding the static keyword is crucial for Java programming and helps in writing efficient, memory-conscious code.
