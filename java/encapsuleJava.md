# Encapsulation in Java

## Overview
Encapsulation is one of the four fundamental principles of Object-Oriented Programming (OOP), along with inheritance, polymorphism, and abstraction.

## What is Encapsulation?

Encapsulation comes from the concept of a **capsule** - keeping something closed so that no one from the outside can access it directly. In programming terms, it means:

- **Binding data and methods together** into a single unit (class)
- **Restricting direct access** to some of an object's components
- **Controlling access** through well-defined interfaces (getters and setters)

## Why Use Encapsulation?

Imagine you have a `Human` class with several features. Without encapsulation, anyone could access and modify the internal data directly, which can lead to:
- Data corruption
- Security vulnerabilities
- Unpredictable behavior
- Difficulty in maintaining code

## Implementation in Java

### Step 1: Make Instance Variables Private

```java
public class Human {
    private String name;        // Private - cannot be accessed directly
    private int age;           // Private - cannot be accessed directly
    private String email;      // Private - cannot be accessed directly
}
```

### Step 2: Create Getter and Setter Methods

```java
public class Human {
    private String name;
    private int age;
    private String email;
    
    // Getter for name
    public String getName() {
        return name;
    }
    
    // Setter for name
    public void setName(String name) {
        this.name = name;
    }
    
    // Getter for age
    public int getAge() {
        return age;
    }
    
    // Setter for age with validation
    public void setAge(int age) {
        if (age > 0 && age < 150) {
            this.age = age;
        } else {
            System.out.println("Invalid age!");
        }
    }
    
    // Getter for email
    public String getEmail() {
        return email;
    }
    
    // Setter for email
    public void setEmail(String email) {
        this.email = email;
    }
}
```

### Step 3: Using the Encapsulated Class

```java
public class Main {
    public static void main(String[] args) {
        Human person = new Human();
        
        // This won't work - compilation error
        // person.name = "John"; // Error: name has private access
        
        // This is the correct way
        person.setName("John");
        person.setAge(25);
        person.setEmail("john@example.com");
        
        // Accessing data through getters
        System.out.println("Name: " + person.getName());
        System.out.println("Age: " + person.getAge());
        System.out.println("Email: " + person.getEmail());
    }
}
```

## Benefits of Encapsulation

1. **Data Protection**: Private variables cannot be accessed directly from outside the class
2. **Validation**: Setters can include validation logic to ensure data integrity
3. **Flexibility**: Internal implementation can be changed without affecting external code
4. **Maintainability**: Code is easier to maintain and debug
5. **Security**: Sensitive data can be protected from unauthorized access

## IDE Support

Most modern IDEs (Integrated Development Environments) can automatically generate getters and setters:

- **Eclipse**: Right-click → Source → Generate Getters and Setters
- **IntelliJ IDEA**: Alt + Insert → Getter and Setter
- **NetBeans**: Right-click → Insert Code → Getter and Setter
- **VS Code**: Use Java extensions with similar functionality

## Advanced Example with Validation

```java
public class BankAccount {
    private String accountNumber;
    private double balance;
    private String ownerName;
    
    public BankAccount(String accountNumber, String ownerName) {
        this.accountNumber = accountNumber;
        this.ownerName = ownerName;
        this.balance = 0.0;
    }
    
    // Getter for balance (read-only access)
    public double getBalance() {
        return balance;
    }
    
    // No direct setter for balance - controlled through methods
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println("Deposited: $" + amount);
        } else {
            System.out.println("Invalid deposit amount!");
        }
    }
    
    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            System.out.println("Withdrawn: $" + amount);
        } else {
            System.out.println("Invalid withdrawal amount or insufficient funds!");
        }
    }
    
    public String getAccountNumber() {
        return accountNumber;
    }
    
    public String getOwnerName() {
        return ownerName;
    }
    
    public void setOwnerName(String ownerName) {
        if (ownerName != null && !ownerName.trim().isEmpty()) {
            this.ownerName = ownerName;
        } else {
            System.out.println("Invalid owner name!");
        }
    }
}
```

## Related Concepts

### 1. Access Modifiers
- **private**: Accessible only within the same class
- **protected**: Accessible within the same package and subclasses
- **public**: Accessible from anywhere
- **default** (no modifier): Accessible within the same package

### 2. Data Hiding
Encapsulation implements data hiding by making instance variables private and providing controlled access through methods.

### 3. Information Hiding
The internal implementation details are hidden from the outside world, exposing only what's necessary through public methods.

### 4. Abstraction vs Encapsulation
- **Abstraction**: Hiding complexity and showing only essential features
- **Encapsulation**: Hiding data and providing controlled access to it

## Best Practices

1. **Always make instance variables private**
2. **Provide public getters and setters only when necessary**
3. **Include validation logic in setters**
4. **Use meaningful method names**
5. **Consider making some data read-only** (only getter, no setter)
6. **Document your getters and setters** when they perform complex operations

## Conclusion

Encapsulation is a powerful feature that helps create robust, maintainable, and secure Java applications. By binding data and methods together and controlling access through well-defined interfaces, we can build better object-oriented programs that are easier to understand, maintain, and extend.
