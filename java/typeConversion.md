# Java Type Conversion and Casting

## Overview

In Java, we cannot change the actual type of a variable once it's declared, but we can assign values of different types through type conversion and casting mechanisms. This tutorial covers the fundamental concepts of type conversion, casting, and type promotion.

## Key Concepts

### 1. Variable Type Immutability
Once a variable is declared with a specific type, its type cannot be changed. However, we can assign values of compatible types through conversion mechanisms.

```java
int number = 10;        // 'number' is always an int
// number cannot become a double, but we can assign converted values
```

## Type Conversion Categories

### 2. Widening Conversion (Implicit/Automatic)

**Definition:** Converting from a smaller data type to a larger data type. This happens automatically because there's no risk of data loss.

**Memory Size Hierarchy:**
```
byte (1 byte) → short (2 bytes) → int (4 bytes) → long (8 bytes) → float (4 bytes) → double (8 bytes)
char (2 bytes) → int (4 bytes)
```

**Examples:**

```java
public class WideningConversion {
    public static void main(String[] args) {
        // Automatic widening conversions
        byte b = 32;
        int a = 233;
        
        // This works - widening (small to large)
        a = b;  // byte to int conversion (automatic)
        System.out.println("a = " + a); // Output: a = 32
        
        // More examples of widening
        short s = 100;
        long l = s;     // short to long
        float f = l;    // long to float
        double d = f;   // float to double
        
        char c = 'A';
        int charToInt = c;  // char to int (ASCII value)
        System.out.println("Character 'A' as int: " + charToInt); // Output: 65
    }
}
```

### 3. Narrowing Conversion (Explicit/Manual Casting)

**Definition:** Converting from a larger data type to a smaller data type. This requires explicit casting because data loss might occur.

**Syntax:** `(target_type) value`

**Examples:**

```java
public class NarrowingConversion {
    public static void main(String[] args) {
        byte b = 32;
        int a = 233;
        
        // This doesn't work - narrowing without casting
        // b = a;  // Compilation error!
        
        // This works - explicit casting
        b = (byte) a;  // int to byte conversion (explicit)
        System.out.println("b = " + b); // Output: b = -23 (data loss occurred)
        
        // More examples of narrowing
        double d = 123.456;
        float f = (float) d;    // double to float
        long l = (long) f;      // float to long (decimal part lost)
        int i = (int) l;        // long to int
        short s = (short) i;    // int to short
        byte by = (byte) s;     // short to byte
        
        System.out.println("Original double: " + d);   // 123.456
        System.out.println("After conversions: " + by); // 123
    }
}
```

### 4. Data Loss in Narrowing

When performing narrowing conversions, data loss can occur:

```java
public class DataLoss {
    public static void main(String[] args) {
        // Example 1: Value too large for target type
        int largeValue = 300;
        byte smallByte = (byte) largeValue;
        System.out.println("300 as byte: " + smallByte); // Output: 44 (overflow)
        
        // Example 2: Decimal precision loss
        double decimal = 123.789;
        int integer = (int) decimal;
        System.out.println("123.789 as int: " + integer); // Output: 123
        
        // Example 3: Understanding byte overflow
        // byte range: -128 to 127
        // 300 - 256 = 44 (wrapping occurs)
    }
}
```

## Type Promotion in Expressions

### 5. Arithmetic Type Promotion

When performing arithmetic operations, Java automatically promotes smaller types to prevent overflow:

**Rules:**
1. `byte`, `short`, and `char` are promoted to `int`
2. If one operand is `long`, the other is promoted to `long`
3. If one operand is `float`, the other is promoted to `float`  
4. If one operand is `double`, the other is promoted to `double`

```java
public class TypePromotion {
    public static void main(String[] args) {
        // byte * byte example
        byte b1 = 10;
        byte b2 = 20;
        
        // This doesn't work - result is promoted to int
        // byte result = b1 * b2;  // Compilation error!
        
        // This works - accepting the promoted type
        int result = b1 * b2;
        System.out.println("10 * 20 = " + result); // Output: 200
        
        // To store back in byte, explicit casting needed
        byte byteResult = (byte)(b1 * b2);
        System.out.println("As byte: " + byteResult); // Output: -56 (overflow)
        
        // More promotion examples
        short s1 = 100;
        short s2 = 200;
        int shortMultiply = s1 * s2;  // short * short = int
        
        char c1 = 'A';  // 65
        char c2 = 'B';  // 66  
        int charAdd = c1 + c2;  // char + char = int
        System.out.println("'A' + 'B' = " + charAdd); // Output: 131
    }
}
```

### 6. Mixed Type Expressions

```java
public class MixedExpressions {
    public static void main(String[] args) {
        byte b = 10;
        short s = 20;
        int i = 30;
        long l = 40L;
        float f = 50.5f;
        double d = 60.6;
        
        // Different combinations and their result types
        int result1 = b + s;        // byte + short = int
        long result2 = i + l;       // int + long = long
        float result3 = s + f;      // short + float = float
        double result4 = f + d;     // float + double = double
        
        // Complex expression
        double complexResult = b + s * i - l / f + d;
        // Breakdown: byte+int-long+double = double (highest precision wins)
        
        System.out.println("Complex result: " + complexResult);
    }
}
```

## Practical Examples and Common Pitfalls

### 7. String to Number Conversion

```java
public class StringConversion {
    public static void main(String[] args) {
        // String to primitive conversions
        String numberString = "123";
        
        int intValue = Integer.parseInt(numberString);
        double doubleValue = Double.parseDouble(numberString);
        byte byteValue = Byte.parseByte(numberString);
        
        // Number to String conversion
        int number = 456;
        String stringValue = String.valueOf(number);
        // or
        String stringValue2 = Integer.toString(number);
        
        System.out.println("String to int: " + intValue);
        System.out.println("Int to string: " + stringValue);
    }
}
```

### 8. Common Mistakes and Best Practices

```java
public class CommonMistakes {
    public static void main(String[] args) {
        // Mistake 1: Forgetting casting for narrowing
        int large = 1000;
        // byte small = large;  // ERROR!
        byte small = (byte) large;  // Correct, but data loss
        
        // Mistake 2: Not considering promotion in operations
        byte a = 100;
        byte b = 100;
        // byte sum = a + b;  // ERROR! Result is int
        byte sum = (byte)(a + b);  // Correct
        
        // Mistake 3: Precision loss in floating point
        float f = 123.456789f;  // Only ~7 digits precision
        double d = f;           // Widening, but original precision already lost
        
        // Best Practice: Use appropriate types from the start
        double preciseValue = 123.456789;  // Better precision
        
        System.out.println("Float precision: " + f);
        System.out.println("Double from float: " + d);
        System.out.println("Original double: " + preciseValue);
    }
}
```

## Summary

| Conversion Type | Direction | Syntax | Data Loss Risk | Example |
|----------------|-----------|--------|----------------|---------|
| **Widening** | Small → Large | Automatic | No | `int a = byteValue;` |
| **Narrowing** | Large → Small | `(type)value` | Yes | `byte b = (byte)intValue;` |
| **Promotion** | In expressions | Automatic | No | `int result = byte1 + byte2;` |

## Key Takeaways

1. **Variable types are immutable** - once declared, a variable's type cannot change
2. **Widening is automatic** - smaller types automatically convert to larger types
3. **Narrowing requires casting** - larger types need explicit casting to smaller types
4. **Type promotion occurs in expressions** - `byte`, `short`, and `char` are promoted to `int` in arithmetic operations
5. **Data loss is possible** - narrowing conversions and precision loss in floating-point operations
6. **Always consider the range** - ensure values fit within the target type's range to avoid overflow

## Practice Exercise

Try to predict the output and identify any compilation errors:

```java
public class Practice {
    public static void main(String[] args) {
        byte b1 = 50;
        byte b2 = 60;
        short s = 1000;
        int i = 70000;
        
        // What happens here?
        byte result1 = b1 + b2;          // ?
        short result2 = s + b1;          // ?
        byte result3 = (byte)(b1 + b2);  // ?
        byte result4 = (byte)i;          // ?
        
        char c = 'Z';
        int result5 = c + b1;            // ?
    }
}
```

**Answers:**
- Line 1: Compilation error (int result needs casting)
- Line 2: Compilation error (int result needs casting)  
- Line 3: 110 (successful casting)
- Line 4: Data loss/overflow (70000 doesn't fit in byte)
- Line 5: 140 ('Z' is 90, + 50 = 140)

---

*This tutorial covers the essential concepts of Java type conversion and casting. Practice with different data types and ranges to master these concepts!*
