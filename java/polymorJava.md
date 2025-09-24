# Java Polymorphism Tutorial

## What is Polymorphism?

Polymorphism is a fundamental concept in Object-Oriented Programming (OOP) that allows objects to **take different forms**. It enables the same interface to be used for different underlying data types or classes, allowing methods to behave differently based on different situations.

The word "polymorphism" comes from Greek, meaning "many forms."

## Types of Polymorphism

Java supports two main types of polymorphism:

### 1. Compile-Time Polymorphism (Early Binding)

- **Definition**: The behavior is decided at compile time
- **Implementation**: Achieved through **Method Overloading**
- **How it works**: The method that needs to be executed is determined based on the parameters passed to it

#### Example of Method Overloading:

```java
class Calculator {
    // Method with 2 integer parameters
    public int add(int a, int b) {
        return a + b;
    }
    
    // Method with 3 integer parameters
    public int add(int a, int b, int c) {
        return a + b + c;
    }
    
    // Method with 2 double parameters
    public double add(double a, double b) {
        return a + b;
    }
}
```

### 2. Runtime Polymorphism (Late Binding)

- **Definition**: The behavior is determined at runtime
- **Implementation**: Achieved through **Method Overriding** and **Dynamic Method Dispatch**
- **How it works**: The actual method to be called is determined by the object being referenced, not the reference type

## Runtime Polymorphism in Detail

### Basic Concept

Consider two classes with an inheritance relationship:

```java
class Computer {
    public void start() {
        System.out.println("Computer is starting...");
    }
    
    public void shutdown() {
        System.out.println("Computer is shutting down...");
    }
}

class Laptop extends Computer {
    @Override
    public void start() {
        System.out.println("Laptop is starting with battery check...");
    }
    
    @Override
    public void shutdown() {
        System.out.println("Laptop is shutting down and saving battery...");
    }
    
    public void closeLid() {
        System.out.println("Closing laptop lid...");
    }
}
```

### Object Creation Variations

With inheritance, we can create objects in different ways:

```java
public class PolymorphismDemo {
    public static void main(String[] args) {
        // Normal object creation
        Laptop laptop1 = new Laptop();
        
        // Polymorphic object creation
        Computer laptop2 = new Laptop(); // Reference type: Computer, Object type: Laptop
        
        // Method calls
        laptop1.start(); // Output: "Laptop is starting with battery check..."
        laptop2.start(); // Output: "Laptop is starting with battery check..."
    }
}
```

### Real-World Example: Computer and Laptop

Think of it this way:
- A **Laptop** is basically a **Computer**
- So we can have a reference of the parent class (`Computer`) pointing to an object of the child class (`Laptop`)
- This represents the real-world relationship where a laptop is a specialized type of computer

### Dynamic Method Dispatch

The key principle of runtime polymorphism:

> **The method execution depends on the OBJECT being created, NOT on the type of reference variable**

```java
class A {
    public void display() {
        System.out.println("Display from class A");
    }
}

class B extends A {
    @Override
    public void display() {
        System.out.println("Display from class B");
    }
}

public class Example {
    public static void main(String[] args) {
        A obj1 = new A(); // Reference: A, Object: A
        A obj2 = new B(); // Reference: A, Object: B
        
        obj1.display(); // Output: "Display from class A"
        obj2.display(); // Output: "Display from class B"
        
        // The method called depends on the object (A or B), 
        // not on the reference type (both are A)
    }
}
```

## Complete Example

```java
// Parent class
class Animal {
    public void makeSound() {
        System.out.println("Animal makes a sound");
    }
    
    public void sleep() {
        System.out.println("Animal is sleeping");
    }
}

// Child class 1
class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Dog barks: Woof! Woof!");
    }
}

// Child class 2  
class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Cat meows: Meow! Meow!");
    }
}

// Demo class
public class PolymorphismExample {
    public static void main(String[] args) {
        // Polymorphic references
        Animal animal1 = new Dog();
        Animal animal2 = new Cat();
        Animal animal3 = new Animal();
        
        // Same method call, different behaviors
        animal1.makeSound(); // Output: "Dog barks: Woof! Woof!"
        animal2.makeSound(); // Output: "Cat meows: Meow! Meow!"
        animal3.makeSound(); // Output: "Animal makes a sound"
        
        // All objects can call inherited methods
        animal1.sleep(); // Output: "Animal is sleeping"
        animal2.sleep(); // Output: "Animal is sleeping"
    }
}
```

## Key Points to Remember

1. **Polymorphism allows the same interface to work with different types**
2. **Compile-time polymorphism** = Method Overloading (Early Binding)
3. **Runtime polymorphism** = Method Overriding + Dynamic Method Dispatch (Late Binding)
4. **The method execution depends on the object type, not the reference type**
5. **Parent reference can point to child object, but not vice versa**
6. **Polymorphism promotes code reusability and flexibility**

## Benefits of Polymorphism

- **Code Reusability**: Same code can work with multiple types
- **Flexibility**: Easy to extend and maintain code
- **Loose Coupling**: Reduces dependencies between classes
- **Dynamic Behavior**: Behavior can be determined at runtime

## Limitations

- **Cannot access child-specific methods** through parent reference without casting
- **Slight performance overhead** due to runtime method resolution

---

*This tutorial covers the fundamental concepts of Java Polymorphism. Practice with different examples to strengthen your understanding!*
