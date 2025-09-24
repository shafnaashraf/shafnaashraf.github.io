# Java Inheritance Tutorial

## What is Inheritance?

Inheritance in Java is a fundamental concept of Object-Oriented Programming (OOP) that allows one class to inherit properties and methods from another class. It establishes an **"is-a"** relationship between classes.

### Real-World Example
Think of a **Laptop** and a **Computer**:
- A laptop **is a** computer
- It has all the basic features of a computer
- Plus some additional features specific to laptops

This **"is-a"** relationship is the foundation of inheritance.

## Key Terminology

| Parent Class | Child Class |
|--------------|-------------|
| Super Class  | Sub Class   |
| Base Class   | Derived Class |

## Basic Example: Calculator Inheritance

Let's start with a simple calculator example:

### Basic Calculator Class
```java
// BasicCalculator.java
public class BasicCalculator {
    public int add(int a, int b) {
        return a + b;
    }
    
    public int subtract(int a, int b) {
        return a - b;
    }
}
```

### Advanced Calculator Class
Now, we need an advanced calculator with all basic features plus some new ones:

```java
// AdvancedCalculator.java
public class AdvancedCalculator extends BasicCalculator {
    // Inherits add() and subtract() from BasicCalculator
    
    public int multiply(int a, int b) {
        return a * b;
    }
    
    public int divide(int a, int b) {
        if (b != 0) {
            return a / b;
        }
        throw new IllegalArgumentException("Cannot divide by zero");
    }
}
```

### Key Points:
- `AdvancedCalculator` **extends** `BasicCalculator`
- It inherits all properties and methods from the parent class
- It can add its own additional methods
- Even if `BasicCalculator.java` source file is not available, inheritance works as long as `BasicCalculator.class` file exists

## Inheritance Chain (Multilevel Inheritance)

Java supports multilevel inheritance:

```
BasicCalculator <- AdvancedCalculator <- VeryAdvancedCalculator
```

### Example:
```java
// VeryAdvancedCalculator.java
public class VeryAdvancedCalculator extends AdvancedCalculator {
    // Inherits from AdvancedCalculator (which inherits from BasicCalculator)
    
    public double power(int base, int exponent) {
        return Math.pow(base, exponent);
    }
    
    public double sqrt(int number) {
        return Math.sqrt(number);
    }
}
```

### Method Resolution
When calling a method on `VeryAdvancedCalculator` object:

1. **First**: Checks if method exists in `VeryAdvancedCalculator`
2. **Then**: If not found, checks in `AdvancedCalculator`
3. **Finally**: If still not found, checks in `BasicCalculator`

```java
public class TestInheritance {
    public static void main(String[] args) {
        VeryAdvancedCalculator calc = new VeryAdvancedCalculator();
        
        // Method from BasicCalculator
        System.out.println(calc.add(5, 3));        // Output: 8
        
        // Method from AdvancedCalculator  
        System.out.println(calc.multiply(4, 2));   // Output: 8
        
        // Method from VeryAdvancedCalculator
        System.out.println(calc.power(2, 3));      // Output: 8.0
    }
}
```

## Multiple Inheritance Problem

### What is Multiple Inheritance?
Multiple inheritance would allow a class to extend multiple classes simultaneously:

```java
// This is NOT allowed in Java!
public class VeryAdvancedCalculator extends AdvancedCalculator, ScientificCalculator {
    // This causes compilation error
}
```

### The Diamond Problem (Ambiguity Problem)

Consider this scenario:
- `AdvancedCalculator` has a method `calculate()`
- `ScientificCalculator` also has a method `calculate()`
- `VeryAdvancedCalculator` extends both

**Question**: When we call `calculate()` on `VeryAdvancedCalculator` object, which method should be executed?

This creates **ambiguity** and confusion.

### Java's Solution

❌ **Java does NOT support multiple inheritance directly**

✅ **Java provides Interfaces as an alternative solution**

## Why Java Doesn't Support Multiple Inheritance

1. **Ambiguity Problem**: Confusion about which method to call
2. **Complexity**: Makes the language more complex
3. **Diamond Problem**: Creates circular dependencies

## Alternative: Interfaces (Preview)

While Java doesn't support multiple inheritance with classes, it provides **interfaces** to achieve similar functionality without ambiguity:

```java
// This will be covered in detail in the next tutorial
public class VeryAdvancedCalculator extends AdvancedCalculator implements ScientificOperations {
    // Implementation details...
}
```

## Summary

### ✅ What Java Supports:
- **Single Inheritance**: One class extends one class
- **Multilevel Inheritance**: Chain of inheritance (A <- B <- C)
- **Hierarchical Inheritance**: Multiple classes inherit from one parent

### ❌ What Java Doesn't Support:
- **Multiple Inheritance**: One class extending multiple classes directly

### 🔑 Key Benefits of Inheritance:
- **Code Reusability**: Don't repeat code
- **Method Overriding**: Customize inherited behavior
- **Polymorphism**: Treat objects of different types uniformly
- **Maintainability**: Changes in parent reflect in children

---

## Next Steps
In the next tutorial, we'll explore:
- **Interfaces** - Java's solution to multiple inheritance
- **Method Overriding** - Customizing inherited methods  
- **Super keyword** - Accessing parent class members
- **Abstract Classes** - Creating templates for inheritance

Happy coding! 🚀
