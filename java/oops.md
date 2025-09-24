# Object-Oriented Programming in Java

## Table of Contents
- [Introduction](#introduction)
- [What is an Object?](#what-is-an-object)
- [Classes: The Blueprint](#classes-the-blueprint)
- [Creating Objects](#creating-objects)
- [Example: Calculator Class](#example-calculator-class)
- [Key OOP Concepts](#key-oop-concepts)
- [Best Practices](#best-practices)
- [Summary](#summary)

## Introduction

In the real world, everything around us is an object. Your phone, your car, even you as a person - everything can be considered an object. Object-Oriented Programming (OOP) in Java follows this same principle. We model real-world entities as objects in our programs.

## What is an Object?

An object has two main characteristics:
- **Properties (Attributes)**: What the object has
- **Behavior (Methods)**: What the object can do

### Real-World Example
Think of a **Car**:
- **Properties**: Color, model, engine capacity, fuel level
- **Behavior**: Start engine, accelerate, brake, turn

### Programming Example
Even a **Human** can be modeled as an object:
- **Properties**: Name, age, height, weight
- **Behavior**: Walk, talk, eat, sleep

> **Key Concept**: In programming, we need to think of everything as objects with properties and behaviors.

## Classes: The Blueprint

To create objects in Java, we first need to create a **class**. A class is like a blueprint or template for creating objects.

### Real-World Analogy
When you need a table, you go to a carpenter with specific dimensions and design requirements. The carpenter uses these specifications (blueprint) to create the actual table. Similarly:
- **Class** = Blueprint/Specification
- **Object** = Actual table created from the blueprint

### How Java Creates Objects
```
Source Code (.java) → Compiler (javac) → Bytecode (.class) → JVM → Object in Memory
```

The JVM (Java Virtual Machine) creates objects from the blueprint (class) via class files. These class files contain bytecode that the JVM can execute.

## Creating Objects

There are two parts to creating an object in Java:

### 1. Declaration (Creating Reference)
```java
ClassName variableName;  // This creates only a reference, NOT the object
```

### 2. Instantiation (Creating Actual Object)
```java
variableName = new ClassName();  // This creates the actual object
```

### Combined Syntax
```java
ClassName variableName = new ClassName();  // Declaration + Instantiation
```

**Important**: 
- `new ClassName()` is what actually creates the object in memory
- The variable just holds a reference (address) to that object

## Example: Calculator Class

Let's create a practical example with a Calculator class that adds two numbers:

### Calculator.java (The Blueprint)
```java
public class Calculator {
    // Properties (if needed)
    private String calculatorName;
    
    // Constructor
    public Calculator() {
        this.calculatorName = "Basic Calculator";
    }
    
    public Calculator(String name) {
        this.calculatorName = name;
    }
    
    // Behavior (Method)
    public int add(int num1, int num2) {
        int result = num1 + num2;
        System.out.println("Result: " + result);
        return result;
    }
    
    public void printCalculatorName() {
        System.out.println("Calculator: " + calculatorName);
    }
}
```

### Main.java (Using the Calculator)
```java
public class Main {
    public static void main(String[] args) {
        // Step 1: Create reference (declaration)
        Calculator calc;
        
        // Step 2: Create actual object (instantiation)
        calc = new Calculator();
        
        // Or combine both steps
        Calculator myCalculator = new Calculator("Advanced Calculator");
        
        // Step 3: Use the object (call methods)
        calc.add(5, 3);                    // Output: Result: 8
        myCalculator.add(10, 20);          // Output: Result: 30
        myCalculator.printCalculatorName(); // Output: Calculator: Advanced Calculator
    }
}
```

### Output
```
Result: 8
Result: 30
Calculator: Advanced Calculator
```

## Key OOP Concepts

### 1. Encapsulation
Bundling data (properties) and methods that work on that data within a single unit (class).

```java
public class BankAccount {
    private double balance;  // Private property (encapsulated)
    
    public void deposit(double amount) {  // Public method to access private data
        if (amount > 0) {
            balance += amount;
        }
    }
    
    public double getBalance() {
        return balance;
    }
}
```

### 2. Inheritance
Creating new classes based on existing classes.

```java
// Parent class
public class Vehicle {
    protected String brand;
    
    public void start() {
        System.out.println("Vehicle started");
    }
}

// Child class
public class Car extends Vehicle {
    private int doors;
    
    public void accelerate() {
        System.out.println("Car is accelerating");
    }
}
```

### 3. Polymorphism
Objects of different types can be treated as objects of a common base type.

```java
Vehicle vehicle1 = new Car();
Vehicle vehicle2 = new Motorcycle();
```

### 4. Abstraction
Hiding complex implementation details while showing only essential features.

```java
public abstract class Shape {
    abstract double calculateArea();  // Abstract method
}

public class Circle extends Shape {
    private double radius;
    
    @Override
    double calculateArea() {
        return Math.PI * radius * radius;
    }
}
```

## Best Practices

1. **Naming Conventions**:
   - Class names: PascalCase (`Calculator`, `BankAccount`)
   - Variable/Method names: camelCase (`calculatorName`, `addNumbers`)

2. **Encapsulation**:
   - Make fields private and provide public getter/setter methods
   ```java
   private int age;
   
   public int getAge() { return age; }
   public void setAge(int age) { this.age = age; }
   ```

3. **Constructor Usage**:
   - Always provide constructors to initialize objects properly
   - Consider providing both default and parameterized constructors

4. **Method Design**:
   - Keep methods focused on a single responsibility
   - Use meaningful names that describe what the method does

## Summary

- **Everything is an object** in OOP - model real-world entities with properties and behaviors
- **Classes are blueprints** for creating objects
- **Objects are instances** of classes created in memory
- Use `new ClassName()` to actually create objects
- Variable declarations only create references, not objects
- Master the four pillars of OOP: Encapsulation, Inheritance, Polymorphism, and Abstraction

### Remember
```java
// This creates a reference
Calculator calc;

// This creates the actual object
calc = new Calculator();

// Now you can use the object
calc.add(5, 3);
```

Happy coding! 🚀
