# Exceptions in Java

## Types of Errors

Java programs can encounter three main types of errors:

### 1. Compile-Time Errors
Errors that occur during compilation phase, preventing the code from being compiled.
- **Example**: Syntax errors, missing semicolons, undeclared variables

### 2. Runtime Errors (Exceptions)
Code compiles successfully but unexpectedly stops working during execution due to some errors. These will stop the execution of the program.
- **Example**: Dividing by zero, accessing null object

### 3. Logical Errors
The program runs without crashing but doesn't produce the expected result. These are bugs in the program logic.
- **Example**: Using wrong formula, incorrect condition in if statement

## Exception Handling

### Try-Catch Block
When dealing with statements in Java:
- **Normal statements**: Execute without issues
- **Critical statements**: May throw exceptions

Put critical statements inside a **try block**. If there's an exception, it will throw an exception object. After the catch block executes, other statements continue to execute.

```java
try {
    // Critical statements that might throw exceptions
    int result = 10 / 0;  // This will throw ArithmeticException
} catch (Exception e) {
    // Handle the exception
    System.out.println("An exception occurred: " + e.getMessage());
}
```

### Common Exception Types
- **ArithmeticException**: Division by zero
- **ArrayIndexOutOfBoundsException**: Accessing invalid array index

### Multiple Catch Blocks
You can create separate catch blocks for each exception type:

```java
try {
    // Critical code
} catch (ArithmeticException e) {
    System.out.println("Arithmetic error: " + e.getMessage());
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Array index error: " + e.getMessage());
} catch (Exception e) {
    System.out.println("General exception: " + e.getMessage());
}
```

## Exception Hierarchy

```
Object
  └── Throwable (interface)
      ├── Exception
      │   ├── SQLException (Checked)
      │   └── RuntimeException (Unchecked)
      └── Error (Cannot be handled)
```

### Exception Types:
- **Checked Exceptions**: Must be handled at compile time (e.g., SQLException)
- **Unchecked Exceptions**: Runtime exceptions that don't need to be declared (e.g., RuntimeException)
- **Error**: System-level errors that cannot be handled by application code

## Throw Keyword

Use `throw` when you want to manually generate an exception. For example, when someone tries to divide a number by 20, you want to trigger a specific action:

```java
public void checkDivision(int number, int divisor) {
    if (divisor == 20) {
        throw new Exception("Division by 20 is not allowed");
        // or
        throw new Exception(); // without message
    }
}
```

## Creating Custom Exceptions

To create your own exception class:

1. Create a class that extends Exception
2. Create a constructor
3. If you want to print a message, pass it to `super(message)` to call the Exception class method

```java
class CustomException extends Exception {
    public CustomException(String message) {
        super(message);
    }
}
```

## Throws Keyword

The `throws` keyword is used to duck (delegate) exception handling to the calling method.

### Scenario:
- Method `d()` has critical code
- Method `e()` has critical code  
- Method `c()` calls both `d()` and `e()`

Instead of adding try-catch in both `d()` and `e()`, you can use the `throws` keyword:

```java
public void methodD() throws Exception {
    // Critical code that might throw exception
    // No try-catch needed here
}

public void methodE() throws Exception {
    // Critical code that might throw exception
    // No try-catch needed here
}

public void methodC() {
    try {
        methodD();  // These calls are placed in try-catch
        methodE();
    } catch (Exception e) {
        System.out.println("Exception handled in calling method: " + e.getMessage());
    }
}
```

The called methods (`d` and `e`) just throw exceptions, and the parent method (`c`) handles them. This provides a cleaner separation of concerns and centralized exception handling.
