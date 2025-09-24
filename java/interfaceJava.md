# Java Interface Tutorial

## What is an Interface?

An **interface** in Java is a blueprint that defines what a class should do, but not how it should do it. When you have only abstract methods and no concrete implementations, you don't need an abstract class - an interface is the perfect solution.

### Key Characteristics of Interfaces:

- **Interface is NOT a class** - it's a separate construct
- **All methods are `public abstract` by default** - you don't need to explicitly write these keywords
- **Cannot be instantiated** - you cannot create objects directly from an interface
- **All variables are `public static final` by default** - they are constants

## Basic Interface Syntax

```java
// Define an interface
interface Animal {
    // All methods are public abstract by default
    void makeSound();
    void move();
    
    // All variables are public static final by default
    int LEGS = 4;
}
```

## Implementing an Interface

Since we cannot instantiate an interface, we create a class that **implements** this interface:

```java
class Dog implements Animal {
    @Override
    public void makeSound() {
        System.out.println("Woof! Woof!");
    }
    
    @Override
    public void move() {
        System.out.println("Running on four legs");
    }
}

// Usage
public class Main {
    public static void main(String[] args) {
        // We can create reference of interface type
        Animal myDog = new Dog();
        myDog.makeSound(); // Output: Woof! Woof!
        myDog.move();      // Output: Running on four legs
        
        // Accessing interface constants
        System.out.println("Default legs: " + Animal.LEGS); // Output: 4
    }
}
```

## Interface Inheritance

An interface can extend another interface, creating a hierarchy:

```java
interface Flyable {
    void fly();
}

interface Bird extends Animal, Flyable {
    void buildNest();
}

class Eagle implements Bird {
    @Override
    public void makeSound() {
        System.out.println("Screech!");
    }
    
    @Override
    public void move() {
        System.out.println("Soaring through the sky");
    }
    
    @Override
    public void fly() {
        System.out.println("Flying at high altitude");
    }
    
    @Override
    public void buildNest() {
        System.out.println("Building nest on cliff");
    }
}
```

## Why Do We Need Interfaces?

### 1. **Loose Coupling**
Interfaces help create loosely coupled code. Your code depends on abstractions, not concrete implementations:

```java
interface PaymentProcessor {
    void processPayment(double amount);
}

class CreditCardProcessor implements PaymentProcessor {
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing $" + amount + " via Credit Card");
    }
}

class PayPalProcessor implements PaymentProcessor {
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing $" + amount + " via PayPal");
    }
}

class ShoppingCart {
    private PaymentProcessor processor;
    
    public ShoppingCart(PaymentProcessor processor) {
        this.processor = processor; // Loose coupling
    }
    
    public void checkout(double amount) {
        processor.processPayment(amount);
    }
}
```

### 2. **Multiple Implementations**
Different classes can implement the same interface in their own way:

```java
// Usage example
public class ECommerceApp {
    public static void main(String[] args) {
        ShoppingCart cart1 = new ShoppingCart(new CreditCardProcessor());
        ShoppingCart cart2 = new ShoppingCart(new PayPalProcessor());
        
        cart1.checkout(100.0); // Uses Credit Card
        cart2.checkout(150.0); // Uses PayPal
    }
}
```

## Anonymous Classes with Interfaces

You can implement an interface using anonymous classes for quick, one-time implementations:

```java
interface Greeting {
    void sayHello(String name);
}

public class AnonymousExample {
    public static void main(String[] args) {
        // Anonymous class implementation
        Greeting greeting = new Greeting() {
            @Override
            public void sayHello(String name) {
                System.out.println("Hello, " + name + "!");
            }
        };
        
        greeting.sayHello("John"); // Output: Hello, John!
        
        // Using lambda expression (Java 8+)
        Greeting lambdaGreeting = (name) -> System.out.println("Hi, " + name + "!");
        lambdaGreeting.sayHello("Alice"); // Output: Hi, Alice!
    }
}
```

## Multiple Interface Implementation

**Yes, a class can implement multiple interfaces!** This is one of the key advantages over class inheritance (single inheritance):

```java
interface Swimmer {
    void swim();
}

interface Runner {
    void run();
}

interface Flyer {
    void fly();
}

// A class can implement multiple interfaces
class Duck implements Swimmer, Runner, Flyer {
    @Override
    public void swim() {
        System.out.println("Duck is swimming");
    }
    
    @Override
    public void run() {
        System.out.println("Duck is running");
    }
    
    @Override
    public void fly() {
        System.out.println("Duck is flying");
    }
}

// Usage
public class MultiInterfaceExample {
    public static void main(String[] args) {
        Duck duck = new Duck();
        duck.swim(); // Duck is swimming
        duck.run();  // Duck is running
        duck.fly();  // Duck is flying
        
        // Can be referenced by any of its interface types
        Swimmer swimmer = new Duck();
        swimmer.swim();
        
        Runner runner = new Duck();
        runner.run();
    }
}
```

## Best Practices

1. **Use interfaces for contracts** - Define what a class should do
2. **Favor composition over inheritance** - Implement multiple interfaces instead of deep inheritance
3. **Keep interfaces focused** - Follow Single Responsibility Principle
4. **Use meaningful names** - Interface names should clearly indicate their purpose

## Common Interface Examples in Java

- `Runnable` - For classes that can be executed by threads
- `Comparable` - For objects that can be compared
- `Serializable` - For objects that can be serialized
- `ActionListener` - For handling GUI events

```java
// Example: Making a class comparable
class Student implements Comparable<Student> {
    private String name;
    private int grade;
    
    public Student(String name, int grade) {
        this.name = name;
        this.grade = grade;
    }
    
    @Override
    public int compareTo(Student other) {
        return Integer.compare(this.grade, other.grade);
    }
    
    @Override
    public String toString() {
        return name + " (Grade: " + grade + ")";
    }
}
```

## Summary

- Interfaces define contracts that implementing classes must follow
- They enable loose coupling and multiple inheritance of type
- All methods are public abstract by default
- All variables are public static final by default
- Classes can implement multiple interfaces
- Interfaces can extend other interfaces
- Anonymous classes and lambda expressions can implement interfaces
- Interfaces are fundamental to many design patterns and frameworks in Java
