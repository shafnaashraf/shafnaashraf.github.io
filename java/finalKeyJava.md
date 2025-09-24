# Java `final` Keyword Tutorial

The `final` keyword in Java is a modifier that can be applied to variables, methods, and classes to impose certain restrictions. It's a powerful feature that helps enforce immutability and prevents unwanted modifications.

## Final Variable

A **final variable** becomes a constant once it's assigned a value. After initialization, its value cannot be changed.

### Key Characteristics:
- Must be initialized either at declaration or in the constructor
- Cannot be reassigned after initialization
- Creates a constant value
- Convention: Use UPPER_CASE naming for final variables

### Example:
```java
public class FinalVariableExample {
    // Final variable initialized at declaration
    final int MAX_SIZE = 100;
    
    // Final variable initialized in constructor
    final String name;
    
    public FinalVariableExample(String name) {
        this.name = name; // Initialization in constructor
        // this.name = "Another name"; // This would cause compilation error
    }
    
    public void someMethod() {
        final int localConstant = 50;
        // localConstant = 60; // This would cause compilation error
    }
}
```

## Final Method

A **final method** cannot be overridden by subclasses. This ensures that the method's implementation remains unchanged in the inheritance hierarchy.

### Key Characteristics:
- Cannot be overridden in subclasses
- Can still be inherited and called
- Useful for methods that should maintain consistent behavior

### Example:
```java
public class Parent {
    // Final method - cannot be overridden
    public final void display() {
        System.out.println("This is a final method");
    }
    
    // Regular method - can be overridden
    public void regularMethod() {
        System.out.println("This can be overridden");
    }
}

public class Child extends Parent {
    // This would cause compilation error
    // public void display() { }
    
    // This is allowed
    @Override
    public void regularMethod() {
        System.out.println("Overridden method");
    }
}
```

## Final Class

A **final class** cannot be extended or subclassed. No other class can inherit from a final class.

### Key Characteristics:
- Cannot be extended by other classes
- All methods in a final class are implicitly final
- Examples in Java API: `String`, `Integer`, `Double`

### Example:
```java
// Final class - cannot be extended
public final class FinalClass {
    public void someMethod() {
        System.out.println("Method in final class");
    }
}

// This would cause compilation error
// public class SubClass extends FinalClass { }
```

## Common Use Cases

### 1. Constants
```java
public class Constants {
    public static final double PI = 3.14159;
    public static final String APP_NAME = "MyApplication";
}
```

### 2. Immutable Objects
```java
public final class ImmutablePerson {
    private final String name;
    private final int age;
    
    public ImmutablePerson(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public String getName() { return name; }
    public int getAge() { return age; }
}
```

### 3. Security and Design Integrity
```java
public class SecurityManager {
    // Prevent overriding of critical security method
    public final boolean hasPermission(String permission) {
        // Critical security logic
        return checkPermission(permission);
    }
}
```

## Best Practices

1. **Use final for constants**: Always declare constants as `final`
2. **Immutable design**: Use `final` fields to create immutable objects
3. **API stability**: Use `final` methods/classes when you don't want them to be modified
4. **Performance**: `final` can help with compiler optimizations
5. **Thread safety**: `final` variables are thread-safe after construction

## Summary

The `final` keyword is essential for:
- **Variables**: Creating constants and immutable fields
- **Methods**: Preventing method overriding
- **Classes**: Preventing class extension

Understanding and properly using `final` leads to more robust, secure, and maintainable Java code.
