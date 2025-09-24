# Java Method Overloading and Overriding Tutorial

## Table of Contents
- [Method Overloading](#method-overloading)
- [Method Overriding](#method-overriding)
- [Key Differences](#key-differences)
- [Examples](#examples)

## Method Overloading

Method overloading occurs when there are **multiple methods with the same name in the same class** but with **differences in their parameters**.

### Key Points:
- Multiple methods share the same name
- Methods must differ in their parameter list (number, type, or order of parameters)
- **Return type alone cannot differentiate overloaded methods**
- Occurs within the same class
- Resolved at compile time (compile-time polymorphism)

### Example:
```java
class A {
    // Method with no parameters
    void show() {
        System.out.println("Method with no parameters");
    }
    
    // Overloaded method with two int parameters
    void show(int a, int b) {
        System.out.println("Method with two parameters: " + a + ", " + b);
    }
    
    // Another overloaded method with different parameter type
    void show(String message) {
        System.out.println("Method with String parameter: " + message);
    }
    
    // Overloaded method with different number of parameters
    void show(int a, int b, int c) {
        System.out.println("Method with three parameters: " + a + ", " + b + ", " + c);
    }
}
```

### Usage:
```java
public class Main {
    public static void main(String[] args) {
        A obj = new A();
        obj.show();           // Calls show()
        obj.show(5, 10);      // Calls show(int a, int b)
        obj.show("Hello");    // Calls show(String message)
        obj.show(1, 2, 3);    // Calls show(int a, int b, int c)
    }
}
```

## Method Overriding

Method overriding occurs when there are **two classes with an inheritance relationship**, and both classes have the **same method signature**. The child class overrides the method of the parent class.

### Key Points:
- Requires inheritance relationship between classes
- Child class provides a specific implementation of a method already defined in parent class
- Method signature must be exactly the same (name, parameters, return type)
- Resolved at runtime (runtime polymorphism)
- **Inheritance is mandatory for method overriding**

### Example:
```java
// Parent class
class A {
    void display() {
        System.out.println("Display method in class A");
    }
    
    void show() {
        System.out.println("Show method in parent class A");
    }
}

// Child class inheriting from A
class B extends A {
    // Overriding the display method from parent class
    @Override
    void display() {
        System.out.println("Display method overridden in class B");
    }
    
    // Overriding the show method from parent class
    @Override
    void show() {
        System.out.println("Show method overridden in child class B");
    }
}
```

### Usage:
```java
public class Main {
    public static void main(String[] args) {
        A parentObj = new A();
        B childObj = new B();
        A polymorphicObj = new B(); // Parent reference, child object
        
        parentObj.display();     // Output: Display method in class A
        childObj.display();      // Output: Display method overridden in class B
        polymorphicObj.display(); // Output: Display method overridden in class B
    }
}
```

## Key Differences

| Aspect | Method Overloading | Method Overriding |
|--------|-------------------|-------------------|
| **Relationship** | Same class | Parent-child classes (inheritance) |
| **Method Signature** | Different parameters | Same signature |
| **Return Type** | Can be different | Must be same (or covariant) |
| **Binding** | Compile-time | Runtime |
| **Polymorphism Type** | Compile-time polymorphism | Runtime polymorphism |
| **Inheritance Required** | No | Yes |
| **Purpose** | Increase readability | Provide specific implementation |

## Complete Example

```java
// Demonstrating both concepts together
class Animal {
    // Method to be overridden
    void makeSound() {
        System.out.println("Animal makes a sound");
    }
    
    // Overloaded methods
    void eat() {
        System.out.println("Animal eats");
    }
    
    void eat(String food) {
        System.out.println("Animal eats " + food);
    }
}

class Dog extends Animal {
    // Method overriding
    @Override
    void makeSound() {
        System.out.println("Dog barks: Woof!");
    }
    
    // Method overloading in child class
    void eat(String food, int quantity) {
        System.out.println("Dog eats " + quantity + " portions of " + food);
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        
        // Method overriding in action
        dog.makeSound(); // Output: Dog barks: Woof!
        
        // Method overloading in action
        dog.eat();                    // Inherited from Animal
        dog.eat("bones");             // Inherited from Animal
        dog.eat("kibble", 2);         // Defined in Dog class
    }
}
```

## Best Practices

### For Method Overloading:
- Use meaningful parameter names
- Avoid overloading with too many variations
- Consider using different method names if the functionality is significantly different

### For Method Overriding:
- Always use `@Override` annotation
- Ensure the overridden method provides meaningful implementation
- Call `super.methodName()` if you need to invoke parent method
- Maintain the contract of the parent method

---

*This tutorial covers the fundamental concepts of method overloading and method overriding in Java. Practice with different examples to strengthen your understanding!*
