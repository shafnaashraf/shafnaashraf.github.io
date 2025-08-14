# Java Access Modifiers Tutorial

## Overview

Access modifiers in Java control the visibility and accessibility of classes, methods, and variables. They are essential for implementing encapsulation, one of the core principles of object-oriented programming.

## Types of Access Modifiers

### 1. **Public**
- **Accessibility**: Can be accessed from anywhere in the program
- **Usage**: Use when something needs to be accessed outside the package
- **Scope**: No restrictions - accessible from any class, package, or subclass

```java
public class MyClass {
    public int publicVariable = 10;
    
    public void publicMethod() {
        System.out.println("This method can be called from anywhere");
    }
}
```

### 2. **Private**
- **Accessibility**: Can only be used within the same class
- **Usage**: Use for internal implementation details that should not be exposed
- **Scope**: Most restrictive - only accessible within the declaring class

```java
public class MyClass {
    private int privateVariable = 20;
    
    private void privateMethod() {
        System.out.println("This method can only be called within MyClass");
    }
    
    // Getter method to access private variable
    public int getPrivateVariable() {
        return privateVariable;
    }
}
```

### 3. **Default (Package-Private)**
- **Accessibility**: Can be accessed within the same package
- **Usage**: When no access modifier is specified, it defaults to package-private
- **Scope**: Accessible to all classes within the same package

```java
class DefaultClass {  // No access modifier = default
    int defaultVariable = 30;
    
    void defaultMethod() {
        System.out.println("This method is accessible within the same package");
    }
}
```

### 4. **Protected**
- **Accessibility**: Same package + subclasses from different packages
- **Usage**: Use when you want subclasses to have access, even from different packages
- **Scope**: Package access + inheritance access

```java
public class ParentClass {
    protected int protectedVariable = 40;
    
    protected void protectedMethod() {
        System.out.println("This method is accessible to subclasses");
    }
}

// In a different package
public class ChildClass extends ParentClass {
    public void accessProtected() {
        protectedVariable = 50;  // Accessible because of inheritance
        protectedMethod();       // Accessible because of inheritance
    }
}
```

## Access Modifier Comparison Table

| Access Level | Private | Default | Protected | Public |
|-------------|---------|---------|-----------|--------|
| Same class | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes |
| Same package subclass | ❌ No | ✅ Yes | ✅ Yes | ✅ Yes |
| Same package non-subclass | ❌ No | ✅ Yes | ✅ Yes | ✅ Yes |
| Different package subclass | ❌ No | ❌ No | ✅ Yes | ✅ Yes |
| Different package non-subclass | ❌ No | ❌ No | ❌ No | ✅ Yes |

## Best Practices

### Basic Usage Guidelines:

1. **Classes**: Make classes `public` when they need to be used outside their package
2. **Instance Variables**: Keep instance variables `private` for proper encapsulation
3. **Methods**: Make methods `public` when they need to be accessed externally
4. **Subclass Methods**: Use `protected` when a method needs to be accessed by subclasses

### Example Following Best Practices:

```java
public class BankAccount {
    // Private instance variables (encapsulation)
    private String accountNumber;
    private double balance;
    private String ownerName;
    
    // Public constructor
    public BankAccount(String accountNumber, String ownerName) {
        this.accountNumber = accountNumber;
        this.ownerName = ownerName;
        this.balance = 0.0;
    }
    
    // Public methods for external access
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }
    
    public boolean withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            return true;
        }
        return false;
    }
    
    // Public getter methods
    public double getBalance() {
        return balance;
    }
    
    public String getAccountNumber() {
        return accountNumber;
    }
    
    // Protected method for subclasses
    protected void updateBalance(double newBalance) {
        this.balance = newBalance;
    }
    
    // Private helper method
    private boolean isValidAmount(double amount) {
        return amount > 0;
    }
}
```

## Key Takeaways

- **Encapsulation**: Use `private` for internal data and implementation details
- **Interface**: Use `public` for methods and classes that form your public API
- **Package Organization**: Use default access for package-internal utilities
- **Inheritance**: Use `protected` when you want to allow subclass access
- **Security**: Always use the most restrictive access modifier that still allows your code to function properly

## Common Mistakes to Avoid

1. Making everything `public` - this breaks encapsulation
2. Forgetting that default access is package-private, not private
3. Using `protected` when `private` would suffice
4. Not providing public getter/setter methods for private fields when external access is needed

Remember: **Start with the most restrictive access modifier and only make it more permissive when necessary!**
