# Java Variables and Data Types - Complete Guide

## Introduction

When building Java applications, understanding how to work with data is fundamental. This guide covers everything you need to know about variables and data types in Java.

## Where Do We Store Data?

When building applications, we need to handle data in two main ways:

1. **Permanent Storage**: Database (persistent storage that survives program termination)
2. **Temporary Storage**: Variables (temporary storage during program execution)

## Variables: The Building Blocks of Data Storage

Think of a variable as a **labeled box** that has:
- **Type**: What kind of data it can hold
- **Name**: How we identify and access it  
- **Value**: The actual data stored inside

## Java's Strong Typing System

Java is **strongly typed**, meaning:
- Every variable must have a declared type
- You cannot store incompatible data types without explicit conversion
- Type checking happens at compile time

## Variable Declaration Syntax

```java
dataType variableName = value;
```

The `=` symbol is the **assignment operator** (not equality).

## Java Primitive Data Types

### What are Primitive Data Types?

Primitive data types are the basic building blocks of data in Java. They are built into the language and represent simple values. Java has **8 primitive data types**.

### Memory Size and Ranges

| Data Type | Size    | Range                                    | Default Value |
|-----------|---------|------------------------------------------|---------------|
| `byte`    | 1 byte  | -128 to 127                             | 0             |
| `short`   | 2 bytes | -32,768 to 32,767                       | 0             |
| `int`     | 4 bytes | -2,147,483,648 to 2,147,483,647         | 0             |
| `long`    | 8 bytes | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 | 0L |
| `float`   | 4 bytes | ~3.4e−038 to 3.4e+038                   | 0.0f          |
| `double`  | 8 bytes | ~1.7e−308 to 1.7e+308                   | 0.0d          |
| `char`    | 2 bytes | 0 to 65,535 (Unicode characters)       | '\u0000'      |
| `boolean` | 1 bit   | true or false                           | false         |

### 1. Integer Types with Examples

```java
class DataTypesDemo {
    public static void main(String args[]) {
        // Integer variables (4 bytes)
        int num1 = 25;
        int num2 = 15;
        
        System.out.println("num1 = " + num1);           // Output: num1 = 25
        System.out.println("num2 = " + num2);           // Output: num2 = 15
        System.out.println(num1 + num2);                // Output: 40
        System.out.println("Sum: " + (num1 + num2));    // Output: Sum: 40
        
        // Other integer types
        byte smallNum = 127;        // 1 byte: -128 to 127
        short mediumNum = 32000;    // 2 bytes: -32,768 to 32,767
        long bigNum = 1000000L;     // 8 bytes: very large numbers (L suffix required)
        
        System.out.println("Byte: " + smallNum);
        System.out.println("Short: " + mediumNum);
        System.out.println("Long: " + bigNum);
        
        // Special integer literals
        int binary = 0b101;         // Binary literal (equals 5 in decimal)
        int readable = 10_00_000;   // Underscore for readability (equals 1000000)
        int hex = 0xFF;             // Hexadecimal literal (equals 255 in decimal)
        int octal = 077;            // Octal literal (equals 63 in decimal)
        
        System.out.println("Binary 0b101 = " + binary);     // Output: Binary 0b101 = 5
        System.out.println("Readable 10_00_000 = " + readable); // Output: Readable 10_00_000 = 1000000
        System.out.println("Hex 0xFF = " + hex);            // Output: Hex 0xFF = 255
        System.out.println("Octal 077 = " + octal);         // Output: Octal 077 = 63
    }
}
```

### 2. Floating Point Types

```java
class FloatingPointDemo {
    public static void main(String args[]) {
        // Important: Java defaults to double for decimal literals
        double salary = 50000.75;    // Default for decimal numbers (8 bytes)
        float price = 19.99f;        // Requires 'f' suffix (4 bytes)
        
        // Without 'f' suffix, this would be an error:
        // float wrong = 19.99;      // ❌ Compile error - cannot convert double to float
        
        System.out.println("Price: $" + price);         // Output: Price: $19.99
        System.out.println("Salary: $" + salary);       // Output: Salary: $50000.75
        System.out.println("Total: " + (price + salary)); // Output: Total: 50020.74
        
        // Scientific notation
        double scientific = 1.23e4;  // 1.23 × 10^4 = 12300.0
        float scientificFloat = 5.67e-3f; // 5.67 × 10^-3 = 0.00567
        
        System.out.println("Scientific double: " + scientific);     // Output: 12300.0
        System.out.println("Scientific float: " + scientificFloat); // Output: 0.00567
    }
}
```

### 3. Character Type and Unicode Support

```java
class CharacterDemo {
    public static void main(String args[]) {
        // Java supports Unicode (2 bytes per character)
        char grade = 'A';           // Basic ASCII character
        char symbol = '$';          // Special symbol
        char unicode1 = '\u0041';   // Unicode for 'A'
        char unicode2 = '\u20AC';   // Unicode for Euro symbol '€'
        char chinese = '中';        // Chinese character
        char emoji = '😊';          // Emoji (requires surrogate pairs in some cases)
        
        System.out.println("Grade: " + grade);          // Output: Grade: A
        System.out.println("Symbol: " + symbol);        // Output: Symbol: $
        System.out.println("Unicode A: " + unicode1);   // Output: Unicode A: A
        System.out.println("Euro: " + unicode2);        // Output: Euro: €
        System.out.println("Chinese: " + chinese);      // Output: Chinese: 中
        System.out.println("Emoji: " + emoji);          // Output: Emoji: 😊
        
        // Character arithmetic
        char letter = 'A';
        System.out.println("ASCII value of A: " + (int)letter); // Output: ASCII value of A: 65
        System.out.println("Next letter: " + (char)(letter + 1)); // Output: Next letter: B
    }
}
```

### 4. Boolean and Literals

```java
class BooleanDemo {
    public static void main(String args[]) {
        boolean isActive = true;    // Boolean literals: true or false
        boolean isComplete = false;
        
        System.out.println("Is Active: " + isActive);   // Output: Is Active: true
        System.out.println("Is Complete: " + isComplete); // Output: Is Complete: false
        
        // Boolean expressions
        int age = 18;
        boolean canVote = age >= 18;
        System.out.println("Can vote: " + canVote);     // Output: Can vote: true
    }
}
```

## String Operations

```java
class StringDemo {
    public static void main(String args[]) {
        String firstName = "John";
        String lastName = "Doe";
        int age = 25;
        
        // String concatenation
        System.out.println(firstName + " " + lastName);           // Output: John Doe
        System.out.println("Name: " + firstName + " " + lastName); // Output: Name: John Doe
        System.out.println("Age: " + age);                       // Output: Age: 25
        System.out.println("Hello, " + firstName + "!");         // Output: Hello, John!
        
        // Be careful with concatenation order
        System.out.println("Sum: " + 5 + 3);        // Output: Sum: 53 (string concatenation)
        System.out.println("Sum: " + (5 + 3));      // Output: Sum: 8 (arithmetic first)
        System.out.println(5 + 3 + " Sum");         // Output: 8 Sum (arithmetic first)
    }
}
```

## Mixed Data Type Examples

```java
class MixedTypesDemo {
    public static void main(String args[]) {
        String name = "Alice";
        int score = 95;
        double percentage = 87.5;
        char grade = 'A';
        boolean passed = true;
        
        // Mixing different types in output
        System.out.println("Student: " + name);
        System.out.println("Score: " + score + "/100");
        System.out.println("Percentage: " + percentage + "%");
        System.out.println("Grade: " + grade);
        System.out.println("Passed: " + passed);
        
        // Complex concatenation
        System.out.println(name + " scored " + score + " points (" + percentage + "%) and received grade " + grade);
        // Output: Alice scored 95 points (87.5%) and received grade A
    }
}
```

## Understanding Literals

**Literals** are the actual values you assign to variables. Java supports various literal formats:

```java
class LiteralsDemo {
    public static void main(String args[]) {
        // Integer literals in different formats
        int decimal = 100;          // Decimal literal
        int binary = 0b1100100;     // Binary literal (equals 100)
        int octal = 0144;           // Octal literal (equals 100)
        int hex = 0x64;             // Hexadecimal literal (equals 100)
        int readable = 1_000_000;   // Underscore for readability (equals 1000000)
        
        System.out.println("All represent same value:");
        System.out.println("Decimal: " + decimal);     // Output: 100
        System.out.println("Binary: " + binary);       // Output: 100
        System.out.println("Octal: " + octal);         // Output: 100
        System.out.println("Hex: " + hex);             // Output: 100
        System.out.println("Readable: " + readable);   // Output: 1000000
        
        // Floating point literals
        double d1 = 123.456;        // Standard decimal
        double d2 = 1.234e2;        // Scientific notation (123.4)
        float f1 = 123.456f;        // Float requires 'f' suffix
        float f2 = 1.234e2f;        // Scientific notation float
        
        // Character literals
        char c1 = 'A';              // Single character
        char c2 = '\n';             // Escape sequence (newline)
        char c3 = '\u0041';         // Unicode literal
        
        // String literals
        String str = "Hello World"; // String literal (not primitive, but commonly used)
        
        // Boolean literals
        boolean t = true;           // Boolean literal
        boolean f = false;          // Boolean literal
    }
}
```

## Important Concepts

### 1. Type Safety
```java
int number = 10;        // ✅ Correct
// int number = "hello"; // ❌ Compile error - type mismatch
```

### 2. Assignment vs Equality
```java
int x = 5;              // Assignment operator
// if (x == 5)          // Equality operator (used in conditions)
```

### 3. String vs Numeric Addition
```java
System.out.println(2 + 3);        // Output: 5 (arithmetic)
System.out.println("2" + "3");    // Output: 23 (string concatenation)
System.out.println("2" + 3);      // Output: 23 (string concatenation)
System.out.println(2 + 3 + "!");  // Output: 5! (arithmetic first, then concatenation)
```

### 4. Why Float Needs 'f' Suffix
```java
double defaultDecimal = 4.3;    // Java defaults to double
float needsSuffix = 4.3f;       // Must specify 'f' for float
// float error = 4.3;           // ❌ Compile error - cannot convert double to float
```

## Variable Naming Conventions

```java
// Good variable names (camelCase)
int studentAge = 20;
String firstName = "John";
double accountBalance = 1500.50;
boolean isLoggedIn = true;

// Avoid these
// int 123name = 5;     // ❌ Cannot start with number
// int class = 10;      // ❌ Cannot use Java keywords
```

## Summary

Variables in Java serve as temporary storage containers during program execution, while databases provide permanent storage. Java's strong typing system ensures type safety by requiring explicit type declarations for all variables. 

Key points to remember:
- Java has 8 primitive data types with specific memory sizes
- `int` uses 4 bytes, `long` uses 8 bytes
- Java defaults to `double` for decimal literals - use `f` suffix for `float`
- Java supports Unicode characters (2 bytes each)
- Literals are the actual values, and Java supports various formats (binary, hex, etc.)
- Use underscores in numeric literals for better readability

Understanding variables and data types is fundamental to Java programming, as they form the foundation for all data manipulation and processing in your applications.
