# Java Functional Interface and Lambda Expressions Tutorial

## Functional Interface

A **Functional Interface** is also known as **SAM (Single Abstract Method)** - an interface with exactly one abstract method.

### Defining a Functional Interface

```java
@FunctionalInterface // Annotation to ensure it's a functional interface
interface A {
    void show();
}
```

The `@FunctionalInterface` annotation is optional but recommended as it ensures compile-time checking.

## Problem: Cannot Instantiate Interfaces

We cannot directly create an object of an interface:

```java
A obj = new A(); // ❌ ERROR: Cannot instantiate interface
```

## Traditional Solutions

### Solution 1: Implement with a Class

```java
class B implements A {
    public void show() {
        System.out.println("Implementation in class B");
    }
}

// Usage
B obj = new B();
// OR
A obj = new B();
obj.show();
```

### Solution 2: Anonymous Class

```java
A obj = new A() {
    public void show() {
        System.out.println("Anonymous implementation");
    }
};
obj.show();
```

## Modern Solution: Lambda Expressions

Java introduced **lambda expressions** to simplify anonymous classes for functional interfaces.

### Basic Lambda Syntax

Instead of the anonymous class above, we can write:

```java
A obj = () -> {
    System.out.println("Lambda implementation");
};
obj.show();
```

## Lambda Expression Variations

### With Parameters

If the method accepts parameters:

```java
@FunctionalInterface
interface Calculator {
    void calculate(int i, int j);
}

// With explicit types
Calculator obj = (int i, int j) -> {
    System.out.println("Result: " + (i + j));
};
obj.calculate(5, 3);
```

### Type Inference

Java can infer parameter types:

```java
Calculator obj = (i, j) -> {
    System.out.println("Result: " + (i + j));
};
obj.calculate(5, 3);
```

### Simplified Syntax Rules

1. **Single parameter**: No parentheses needed
```java
@FunctionalInterface
interface Printer {
    void print(String message);
}

Printer obj = message -> System.out.println(message);
```

2. **Single statement**: No curly braces needed
```java
Calculator obj = (i, j) -> System.out.println("Result: " + (i + j));
```

## Return Values in Lambda

### Method with Return Type

```java
@FunctionalInterface
interface Adder {
    int add(int a, int b);
}
```

### Lambda with Return Statement

```java
Adder obj = (a, b) -> {
    return a + b;
};
```

### Simplified Return (Expression Lambda)

When there's only a return statement:

```java
Adder obj = (a, b) -> a + b;
```

## Complete Example

```java
public class LambdaExample {
    @FunctionalInterface
    interface MathOperation {
        int operate(int a, int b);
    }
    
    public static void main(String[] args) {
        // Addition
        MathOperation add = (a, b) -> a + b;
        
        // Multiplication  
        MathOperation multiply = (a, b) -> a * b;
        
        // Usage
        System.out.println("Addition: " + add.operate(5, 3));        // Output: 8
        System.out.println("Multiplication: " + multiply.operate(5, 3)); // Output: 15
    }
}
```

## Key Benefits of Lambda Expressions

- **Concise**: Reduces boilerplate code
- **Readable**: More expressive than anonymous classes  
- **Functional Programming**: Enables functional programming style in Java
- **Performance**: Better performance than anonymous classes in some cases

## Summary

Lambda expressions provide a clean, concise way to implement functional interfaces, making Java code more readable and maintainable. They are especially useful for:

- Event handling
- Collection processing (with Stream API)
- Callback functions
- Functional programming patterns
