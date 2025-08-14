# Java Abstract Keyword Tutorial

## What is the Abstract Keyword?

The `abstract` keyword in Java is used to declare methods without their implementation, which can be implemented somewhere else. It's a way to define a contract that must be followed by subclasses.

## Key Concepts

### 1. Abstract Methods
- Methods declared with `abstract` keyword have no implementation (no method body)
- Can only be declared inside abstract classes
- Must be implemented by any concrete subclass

### 2. Abstract Classes
- Classes that contain at least one abstract method must be declared as `abstract`
- Cannot be instantiated (no object creation)
- Can have both abstract and concrete methods
- Can have constructors, instance variables, and regular methods

## Important Rules

✅ **What you CAN do:**
- Create reference variables of abstract class type
- Have abstract classes without abstract methods
- Have concrete methods in abstract classes
- Have constructors in abstract classes

❌ **What you CANNOT do:**
- Create objects of abstract classes
- Declare abstract methods in concrete classes
- Have abstract methods in non-abstract classes

## Examples

### Example 1: Basic Abstract Class and Method

```java
// Abstract class
abstract class Animal {
    String name;
    
    // Constructor in abstract class
    public Animal(String name) {
        this.name = name;
    }
    
    // Abstract method - no implementation
    abstract void makeSound();
    
    // Concrete method - has implementation
    void sleep() {
        System.out.println(name + " is sleeping");
    }
}

// Concrete class extending abstract class
class Dog extends Animal {
    public Dog(String name) {
        super(name);
    }
    
    // Must implement abstract method
    @Override
    void makeSound() {
        System.out.println(name + " says: Woof!");
    }
}

class Cat extends Animal {
    public Cat(String name) {
        super(name);
    }
    
    // Must implement abstract method
    @Override
    void makeSound() {
        System.out.println(name + " says: Meow!");
    }
}
```

### Example 2: Using Abstract Class References

```java
public class AbstractExample {
    public static void main(String[] args) {
        // ❌ Cannot create object of abstract class
        // Animal animal = new Animal("Generic"); // Compilation error
        
        // ✅ Can create reference of abstract class
        Animal dog = new Dog("Buddy");
        Animal cat = new Cat("Whiskers");
        
        dog.makeSound(); // Output: Buddy says: Woof!
        cat.makeSound(); // Output: Whiskers says: Meow!
        
        dog.sleep(); // Output: Buddy is sleeping
        cat.sleep(); // Output: Whiskers is sleeping
    }
}
```

### Example 3: Abstract Class Without Abstract Methods

```java
// Abstract class with no abstract methods
abstract class Vehicle {
    String brand;
    
    public Vehicle(String brand) {
        this.brand = brand;
    }
    
    // Only concrete methods
    void startEngine() {
        System.out.println(brand + " engine started");
    }
    
    void stopEngine() {
        System.out.println(brand + " engine stopped");
    }
}

class Car extends Vehicle {
    public Car(String brand) {
        super(brand);
    }
    
    // No abstract methods to implement
    void drive() {
        System.out.println("Driving the " + brand + " car");
    }
}
```

### Example 4: Incomplete Implementation Results in Abstract Class

```java
abstract class Shape {
    abstract double calculateArea();
    abstract double calculatePerimeter();
    abstract void draw();
}

// This class doesn't implement all abstract methods
// So it must be declared as abstract
abstract class PartialShape extends Shape {
    @Override
    double calculateArea() {
        return 0; // Some implementation
    }
    
    // calculatePerimeter() and draw() are not implemented
    // So this class remains abstract
}

// Complete implementation - concrete class
class Circle extends Shape {
    double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    @Override
    double calculateArea() {
        return Math.PI * radius * radius;
    }
    
    @Override
    double calculatePerimeter() {
        return 2 * Math.PI * radius;
    }
    
    @Override
    void draw() {
        System.out.println("Drawing a circle with radius: " + radius);
    }
}
```

### Example 5: Real-world Example - Database Connection

```java
abstract class DatabaseConnection {
    String connectionString;
    
    public DatabaseConnection(String connectionString) {
        this.connectionString = connectionString;
    }
    
    // Abstract methods that must be implemented
    abstract void connect();
    abstract void disconnect();
    abstract void executeQuery(String query);
    
    // Concrete method available to all subclasses
    void logConnection() {
        System.out.println("Connecting to: " + connectionString);
    }
}

class MySQLConnection extends DatabaseConnection {
    public MySQLConnection(String connectionString) {
        super(connectionString);
    }
    
    @Override
    void connect() {
        System.out.println("Connecting to MySQL database");
        logConnection();
    }
    
    @Override
    void disconnect() {
        System.out.println("Disconnecting from MySQL database");
    }
    
    @Override
    void executeQuery(String query) {
        System.out.println("Executing MySQL query: " + query);
    }
}

class PostgreSQLConnection extends DatabaseConnection {
    public PostgreSQLConnection(String connectionString) {
        super(connectionString);
    }
    
    @Override
    void connect() {
        System.out.println("Connecting to PostgreSQL database");
        logConnection();
    }
    
    @Override
    void disconnect() {
        System.out.println("Disconnecting from PostgreSQL database");
    }
    
    @Override
    void executeQuery(String query) {
        System.out.println("Executing PostgreSQL query: " + query);
    }
}
```

## Summary

| Concept | Description | Example |
|---------|-------------|---------|
| **Abstract Method** | Method without implementation | `abstract void makeSound();` |
| **Abstract Class** | Class that cannot be instantiated | `abstract class Animal { }` |
| **Concrete Class** | Regular class that can be instantiated | `class Dog extends Animal { }` |
| **Reference Creation** | Can create reference variables | `Animal dog = new Dog();` |
| **Object Creation** | Cannot create objects of abstract class | `new Animal();` ❌ |

## Key Takeaways

1. **Abstract methods** provide a contract that subclasses must follow
2. **Abstract classes** cannot be instantiated but can have references
3. **All abstract methods** must be implemented in concrete subclasses
4. **Abstract classes** can have both abstract and concrete methods
5. **Incomplete implementation** of abstract methods results in another abstract class
6. **Concrete classes** are regular classes that can be instantiated

## When to Use Abstract Classes

- When you want to share code among several related classes
- When you want to force subclasses to implement certain methods
- When you have some common functionality but some methods should be implemented differently
- When you want to provide a common interface but prevent direct instantiation

---

*This tutorial covers the fundamental concepts of abstract classes and methods in Java. Practice these examples to strengthen your understanding!*
