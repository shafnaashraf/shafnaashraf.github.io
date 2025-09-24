# Java Strings Tutorial

## Table of Contents
1. [String Basics](#string-basics)
2. [String Object Creation](#string-object-creation)
3. [Memory Management - Stack and Heap](#memory-management---stack-and-heap)
4. [String Concatenation](#string-concatenation)
5. [String Methods](#string-methods)
6. [String Literal vs String Object](#string-literal-vs-string-object)
7. [String Constant Pool](#string-constant-pool)
8. [String Immutability](#string-immutability)
9. [StringBuffer](#stringbuffer)
10. [StringBuilder](#stringbuilder)
11. [Performance Comparison](#performance-comparison)

---

## String Basics

In Java, strings use **double quotes** and String is a **class**, not a primitive data type.

```java
// Correct way to declare strings
String message = "Hello World";
String name = "John";
```

---

## String Object Creation

### Method 1: Using `new` keyword
```java
String variableName = new String();
String variableName = new String("Hello");
```

### Method 2: String Literal (Recommended)
```java
String name = "Hello World"; // No need to use new String()
```

---

## Memory Management - Stack and Heap

When you create a string object, here's what happens behind the scenes in JVM:

```
STACK                    HEAP
┌─────────────┐         ┌─────────────────┐
│ variableName│ ──────► │ "Hello World"   │
│ (reference) │         │ (actual value)  │
└─────────────┘         └─────────────────┘
```

- **Stack**: Contains the variable name (reference)
- **Heap**: Contains the actual string value
- The variable in stack points to the corresponding heap address

### Example:
```java
String name = new String("Hello");
```

```
STACK           HEAP
┌──────┐       ┌─────────┐
│ name │ ────► │ "Hello" │
└──────┘       └─────────┘
```

---

## String Concatenation

The `+` operator can be used to concatenate strings:

```java
String firstName = "John";
String lastName = "Doe";
String fullName = firstName + " " + lastName; // "John Doe"
```

### Important Note about Concatenation:
```java
String name = "Hello";
name = name + " World";
```

**Behind the scenes**: We are NOT actually changing the existing string. Instead:
1. JVM checks the String Constant Pool
2. Since "Hello World" is not present, a new string is created
3. The new address is assigned to the stack reference
4. The original "Hello" string remains unchanged in memory

---

## String Methods

### charAt() Method
Returns the character at a specified index:

```java
String name = "Hello";
char firstChar = name.charAt(0); // 'H'
char secondChar = name.charAt(1); // 'e'
```

### Other Useful String Methods:
```java
String text = "Hello World";

// Length
int length = text.length(); // 11

// Uppercase/Lowercase
String upper = text.toUpperCase(); // "HELLO WORLD"
String lower = text.toLowerCase(); // "hello world"

// Substring
String sub = text.substring(0, 5); // "Hello"

// Contains
boolean contains = text.contains("World"); // true
```

---

## String Literal vs String Object

### String Literals
```java
String s1 = "Hello"; // Stored in String Constant Pool
```

### String Objects
```java
String s2 = new String("Hello"); // Creates object in Heap
```

---

## String Constant Pool

The String Constant Pool is a special area in JVM memory where string literals are stored.

### Example:
```java
String s1 = "shafna";
String s2 = "shafna";
```

**What happens:**
```
STRING CONSTANT POOL
┌─────────────┐
│   "shafna"  │ ◄─── Both s1 and s2 point here
└─────────────┘

STACK
┌────┐    ┌────┐
│ s1 │────│ s2 │
└────┘    └────┘
    \      /
     \    /
      \  /
       \/
    Same reference!
```

**Key Points:**
- Only **ONE** object is created in the String Constant Pool
- Both variables `s1` and `s2` refer to the same memory location
- When a new string literal is created, JVM first checks if it already exists in the pool
- If exists: assigns the existing address to stack reference
- If not exists: creates new entry in pool

### Verification:
```java
String s1 = "shafna";
String s2 = "shafna";
System.out.println(s1 == s2); // true (same reference)

String s3 = new String("shafna");
System.out.println(s1 == s3); // false (different references)
System.out.println(s1.equals(s3)); // true (same content)
```

---

## String Immutability

**Strings are IMMUTABLE** in Java. This means once a string object is created, it cannot be changed.

### Why Strings are Immutable:

1. **Security**: Strings are used in network connections, file paths, etc.
2. **Thread Safety**: Multiple threads can access the same string without synchronization
3. **Caching**: String pool optimization is possible
4. **Performance**: Hash codes can be cached

### Example:
```java
String original = "Hello";
String modified = original.concat(" World");

System.out.println(original); // Still "Hello"
System.out.println(modified); // "Hello World"
```

**Memory representation:**
```
HEAP
┌─────────┐    ┌─────────────┐
│ "Hello" │    │ "Hello World" │
└─────────┘    └─────────────┘
     ▲               ▲
     │               │
  original      modified
```

---

## StringBuffer

**StringBuffer** is a mutable alternative to String, designed for scenarios where you need to modify strings frequently.

### Key Features:
- **Mutable**: Can be modified after creation
- **Thread-Safe**: Synchronized methods
- **Initial Capacity**: 16 characters by default

### Creation and Usage:
```java
StringBuffer sb = new StringBuffer(); // Creates buffer with 16 char capacity
StringBuffer sb2 = new StringBuffer("Hello"); // Creates with initial content
```

### Common Methods:
```java
StringBuffer sb = new StringBuffer();

// Append
sb.append("Hello");
sb.append(" ");
sb.append("World");
System.out.println(sb.toString()); // "Hello World"

// Insert
sb.insert(5, " Beautiful");
System.out.println(sb.toString()); // "Hello Beautiful World"

// Delete
sb.delete(5, 15); // Removes " Beautiful"
System.out.println(sb.toString()); // "Hello World"

// Reverse
sb.reverse();
System.out.println(sb.toString()); // "dlroW olleH"
```

### Capacity Management:
```java
StringBuffer sb = new StringBuffer();
System.out.println(sb.capacity()); // 16 (default)
System.out.println(sb.length());   // 0 (no content yet)

sb.append("Hello World");
System.out.println(sb.capacity()); // 16 (still fits)
System.out.println(sb.length());   // 11 (content length)
```

---

## StringBuilder

**StringBuilder** is similar to StringBuffer but **NOT thread-safe**, making it faster for single-threaded applications.

### Key Features:
- **Mutable**: Can be modified after creation
- **Not Thread-Safe**: No synchronization overhead
- **Better Performance**: Faster than StringBuffer in single-threaded scenarios

### Usage:
```java
StringBuilder sb = new StringBuilder();
sb.append("Hello");
sb.append(" World");
System.out.println(sb.toString()); // "Hello World"
```

### Thread Safety Comparison:
```java
// Thread-safe (slower)
StringBuffer buffer = new StringBuffer();

// Not thread-safe (faster)
StringBuilder builder = new StringBuilder();
```

---

## Performance Comparison

### String Concatenation Performance:

```java
// Inefficient - Creates multiple objects
String result = "";
for (int i = 0; i < 1000; i++) {
    result += "Hello "; // Creates new object each time
}

// Efficient - Uses mutable buffer
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    sb.append("Hello "); // Modifies existing buffer
}
String result = sb.toString();
```

### When to Use What:

| Scenario | Use |
|----------|-----|
| Immutable strings, occasional concatenation | **String** |
| Frequent string modifications, single-threaded | **StringBuilder** |
| Frequent string modifications, multi-threaded | **StringBuffer** |

### Performance Benchmark (Approximate):
- **String**: Slowest (creates multiple objects)
- **StringBuffer**: Medium (thread-safe overhead)
- **StringBuilder**: Fastest (no synchronization)

---

## Summary

1. **String**: Immutable, stored in String Constant Pool for literals
2. **StringBuffer**: Mutable, thread-safe, 16-char default capacity
3. **StringBuilder**: Mutable, not thread-safe, best performance for single-threaded apps
4. Use **String** for simple operations, **StringBuilder/StringBuffer** for frequent modifications
5. String Constant Pool optimizes memory usage for string literals
6. String immutability provides security and thread safety but can impact performance in concatenation-heavy scenarios

---

## Best Practices

1. Use string literals (`"text"`) instead of `new String("text")` when possible
2. Use `StringBuilder` for frequent string concatenations
3. Use `StringBuffer` only when thread safety is required
4. Always use `.equals()` for string content comparison
5. Use `==` only when you want to compare references

---

*Happy Coding! 🚀*
