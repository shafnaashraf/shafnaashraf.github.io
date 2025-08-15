# Java Interfaces Tutorial

## Types of Interfaces in Java

Java interfaces can be categorized into three main types based on the number and nature of methods they contain:

1. **Normal Interface** - Contains 2 or more abstract methods
2. **Functional Interface (SAM)** - Contains a single abstract method
3. **Marker Interface** - Contains no methods

---

## 1. Normal Interface

A normal interface contains **two or more abstract methods** that implementing classes must define.

### Example:

```java
// Normal Interface with multiple methods
public interface Vehicle {
    void start();
    void stop();
    void accelerate();
    int getMaxSpeed();
}

// Implementation
public class Car implements Vehicle {
    @Override
    public void start() {
        System.out.println("Car started");
    }
    
    @Override
    public void stop() {
        System.out.println("Car stopped");
    }
    
    @Override
    public void accelerate() {
        System.out.println("Car accelerating");
    }
    
    @Override
    public int getMaxSpeed() {
        return 200;
    }
}
```

---

## 2. Functional Interface (SAM - Single Abstract Method)

A functional interface contains **exactly one abstract method**. These interfaces are used extensively with lambda expressions and method references.

### Key Points:
- Can have default and static methods
- Annotated with `@FunctionalInterface` (optional but recommended)
- Used for lambda expressions and functional programming

### Example:

```java
@FunctionalInterface
public interface Calculator {
    int calculate(int a, int b);  // Single abstract method
    
    // Default method allowed
    default void printResult(int result) {
        System.out.println("Result: " + result);
    }
    
    // Static method allowed
    static void info() {
        System.out.println("This is a calculator interface");
    }
}

// Usage with lambda expression
public class FunctionalExample {
    public static void main(String[] args) {
        // Lambda expression
        Calculator add = (a, b) -> a + b;
        Calculator multiply = (a, b) -> a * b;
        
        System.out.println(add.calculate(5, 3));      // Output: 8
        System.out.println(multiply.calculate(5, 3)); // Output: 15
    }
}
```

### Built-in Functional Interfaces:
- `Runnable`
- `Predicate<T>`
- `Function<T,R>`
- `Consumer<T>`
- `Supplier<T>`

---

## 3. Marker Interface

A marker interface is an interface that **contains no methods**. It serves as a "marker" or "tag" to indicate that a class has certain properties or capabilities.

### Why Marker Interfaces?

Marker interfaces are used to provide **metadata** about classes at runtime. They help the JVM and other components identify classes with specific characteristics without requiring method implementations.

### Common Use Cases:

1. **Serialization** - `Serializable` interface
2. **Cloning** - `Cloneable` interface  
3. **Remote Method Invocation** - `Remote` interface
4. **Event Handling** - `EventListener` interface

---

## Detailed Explanation: Marker Interfaces

### 1. Serializable Interface

The most common example of a marker interface. It indicates that objects of the class can be serialized (converted to byte stream).

```java
import java.io.*;

// Marker interface - no methods
public class Student implements Serializable {
    private String name;
    private int age;
    
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // getters and setters
    public String getName() { return name; }
    public int getAge() { return age; }
}

// Serialization Example
public class SerializationExample {
    public static void main(String[] args) {
        try {
            Student student = new Student("John", 20);
            
            // Serialize
            FileOutputStream fileOut = new FileOutputStream("student.ser");
            ObjectOutputStream out = new ObjectOutputStream(fileOut);
            out.writeObject(student);
            out.close();
            fileOut.close();
            
            // Deserialize
            FileInputStream fileIn = new FileInputStream("student.ser");
            ObjectInputStream in = new ObjectInputStream(fileIn);
            Student deserializedStudent = (Student) in.readObject();
            in.close();
            fileIn.close();
            
            System.out.println("Name: " + deserializedStudent.getName());
            System.out.println("Age: " + deserializedStudent.getAge());
            
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

### 2. Cloneable Interface

Indicates that a class allows cloning of its objects using `Object.clone()` method.

```java
public class Person implements Cloneable {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
    
    // getters and setters
    public String getName() { return name; }
    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }
}

// Usage
public class CloneExample {
    public static void main(String[] args) {
        try {
            Person original = new Person("Alice", 25);
            Person cloned = (Person) original.clone();
            
            System.out.println("Original: " + original.getName() + ", " + original.getAge());
            System.out.println("Cloned: " + cloned.getName() + ", " + cloned.getAge());
            
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
    }
}
```

### 3. Custom Marker Interface

You can create your own marker interfaces for specific purposes:

```java
// Custom marker interface
public interface Auditable {
    // No methods - just marks classes as auditable
}

public class BankAccount implements Auditable {
    private double balance;
    private String accountNumber;
    
    // class implementation
}

// Usage in framework code
public class AuditService {
    public void auditObject(Object obj) {
        if (obj instanceof Auditable) {
            System.out.println("Auditing: " + obj.getClass().getSimpleName());
            // Perform audit operations
        } else {
            System.out.println("Object is not auditable");
        }
    }
}
```

---

## How Marker Interfaces Work

### Runtime Type Checking

```java
public class InterfaceChecker {
    public static void checkInterfaces(Object obj) {
        Class<?> clazz = obj.getClass();
        
        if (obj instanceof Serializable) {
            System.out.println(clazz.getSimpleName() + " is Serializable");
        }
        
        if (obj instanceof Cloneable) {
            System.out.println(clazz.getSimpleName() + " is Cloneable");
        }
        
        if (obj instanceof Auditable) {
            System.out.println(clazz.getSimpleName() + " is Auditable");
        }
    }
}
```

---

## Advantages of Marker Interfaces

1. **Type Safety** - Compile-time checking
2. **Performance** - No method calls, just type checking
3. **Clear Intent** - Explicitly shows class capabilities
4. **Framework Integration** - Easy to integrate with frameworks and libraries

## Disadvantages

1. **Pollution** - Can lead to interface pollution
2. **Limited Information** - Cannot carry additional metadata
3. **Tight Coupling** - Creates dependency on specific interfaces

---

## Alternative: Annotations

Modern Java often uses annotations instead of marker interfaces for similar purposes:

```java
// Using annotation instead of marker interface
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface Auditable {
    String value() default "";
}

@Auditable("Financial data")
public class BankAccount {
    // class implementation
}
```

---

## Summary

| Interface Type | Methods | Purpose | Example |
|---------------|---------|---------|---------|
| Normal | 2+ abstract methods | Define contract with multiple operations | `List`, `Map` |
| Functional (SAM) | 1 abstract method | Lambda expressions, functional programming | `Runnable`, `Predicate` |
| Marker | 0 methods | Tag/mark classes with specific properties | `Serializable`, `Cloneable` |

Understanding these interface types is crucial for effective Java programming and helps in choosing the right approach for different scenarios.
