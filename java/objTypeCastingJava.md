# Java Typecasting Tutorial

## What is Typecasting?

Typecasting is the process of converting the type of a value from one data type to another.

## Primitive Typecasting

### Example: Double to Integer
```java
double d = 4.5;
int i = (int) d;  // Explicit casting required
// Result: i = 4 (decimal part is truncated)
```

## Object Typecasting with Inheritance

### Basic Setup
```java
class A {
    public void methodA() {
        System.out.println("Method in class A");
    }
}

class B extends A {
    public void methodA() {
        System.out.println("Overridden method in class B");
    }
    
    public void methodB() {
        System.out.println("Method specific to class B");
    }
}
```

## Upcasting (Implicit)

Upcasting happens automatically when you assign a subclass object to a superclass reference.

```java
// Both statements are equivalent:
A obj = (A) new B();  // Explicit upcasting (unnecessary)
A obj = new B();      // Implicit upcasting (preferred)
```

**Key Points about Upcasting:**
- Happens automatically
- Always safe
- No `ClassCastException` risk
- Reference can only access methods defined in the parent class

### Problem with Upcasting
```java
A obj = new B();
obj.methodA();  // ✅ Works - method exists in A
// obj.methodB();  // ❌ Compilation error - methodB() not visible through A reference
```

## Downcasting (Explicit)

When you need to access methods specific to the subclass, you must downcast explicitly.

```java
A obj = new B();  // Upcasting

// To call methodB(), we need to downcast:
B castedObj = (B) obj;  // Explicit downcasting
castedObj.methodB();    // ✅ Now we can call B's specific method

// Or do it in one line:
((B) obj).methodB();    // ✅ Direct downcasting and method call
```

### Safe Downcasting with instanceof

Always check before downcasting to avoid `ClassCastException`:

```java
A obj = new B();

if (obj instanceof B) {
    B castedObj = (B) obj;
    castedObj.methodB();  // Safe to call
} else {
    System.out.println("Object is not an instance of B");
}
```

## Complete Example

```java
public class TypecastingExample {
    public static void main(String[] args) {
        // Upcasting
        A obj = new B();
        obj.methodA();  // Calls B's overridden version
        
        // Downcasting to access B's specific method
        if (obj instanceof B) {
            B bObj = (B) obj;
            bObj.methodB();  // Now we can call B's method
        }
        
        // Direct downcasting (risky without instanceof check)
        ((B) obj).methodB();
    }
}
```

**Output:**
```
Overridden method in class B
Method specific to class B
Method specific to class B
```

## Key Takeaways

1. **Upcasting**: Safe and implicit, but limits access to parent class methods only
2. **Downcasting**: Explicit casting required, enables access to subclass-specific methods
3. **Safety**: Always use `instanceof` before downcasting to prevent `ClassCastException`
4. **Use Case**: Downcast when you need to access methods that exist only in the subclass

## Best Practices

- Prefer composition over inheritance when possible
- Use `instanceof` checks before downcasting
- Keep inheritance hierarchies simple and logical
- Document when downcasting is necessary in your code
