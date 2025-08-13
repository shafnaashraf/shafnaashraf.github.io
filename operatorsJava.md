# Java Operators Tutorial

Welcome to this comprehensive guide on Java operators! This tutorial covers the essential operators you'll use frequently in Java programming.

## Table of Contents
1. [Arithmetic Operators](#arithmetic-operators)
2. [Relational Operators](#relational-operators)
3. [Logical Operators](#logical-operators)
4. [Bitwise Operators](#bitwise-operators)
5. [Ternary Operator](#ternary-operator)
6. [Assignment Operators](#assignment-operators)
7. [Unary Operators](#unary-operators)
8. [Type Comparison Operator (instanceof)](#type-comparison-operator-instanceof)
9. [Bit Shift Operators](#bit-shift-operators)

---

## Arithmetic Operators

Arithmetic operators are used to perform mathematical operations on variables and values.

### Basic Arithmetic Operations

```java
int num1 = 10;
int num2 = 3;

// Addition
int sum = num1 + num2;        // Result: 13

// Subtraction
int difference = num1 - num2; // Result: 7

// Multiplication
int product = num1 * num2;    // Result: 30

// Division
int quotient = num1 / num2;   // Result: 3 (integer division)

// Modulus (remainder)
int remainder = num1 % num2;  // Result: 1
```

### Assignment and Compound Assignment Operators

```java
int num1 = 5;

// Traditional assignment
num1 = num1 + 1;  // num1 becomes 6

// Compound assignment (shorthand)
num1 += 1;        // Equivalent to num1 = num1 + 1
num1 -= 2;        // Equivalent to num1 = num1 - 2
num1 *= 3;        // Equivalent to num1 = num1 * 3
num1 /= 2;        // Equivalent to num1 = num1 / 2
num1 %= 3;        // Equivalent to num1 = num1 % 3
```

### Increment and Decrement Operators

The increment (`++`) and decrement (`--`) operators are special operators that add or subtract 1 from a variable.

#### Post-increment vs Pre-increment

**Post-increment (`num++`)**: Returns the current value first, then increments.

```java
int num = 5;
int result = num++;  // result = 5, num = 6
System.out.println("Result: " + result); // Output: Result: 5
System.out.println("Num: " + num);       // Output: Num: 6
```

**Pre-increment (`++num`)**: Increments first, then returns the new value.

```java
int num = 5;
int result = ++num;  // num = 6, result = 6
System.out.println("Result: " + result); // Output: Result: 6
System.out.println("Num: " + num);       // Output: Num: 6
```

#### Complete Example: Pre vs Post Increment

```java
public class IncrementExample {
    public static void main(String[] args) {
        // Post-increment example
        int a = 10;
        int postResult = a++;
        System.out.println("Post-increment:");
        System.out.println("Original value used: " + postResult); // 10
        System.out.println("New value of a: " + a);              // 11
        
        // Pre-increment example
        int b = 10;
        int preResult = ++b;
        System.out.println("\nPre-increment:");
        System.out.println("New value used: " + preResult);      // 11
        System.out.println("New value of b: " + b);              // 11
    }
}
```

#### Decrement Operators

The same principle applies to decrement operators:

```java
int num = 10;

// Post-decrement
int postDec = num--;  // postDec = 10, num = 9

// Pre-decrement  
int preDec = --num;   // num = 8, preDec = 8
```

---

## Relational Operators

Relational operators are used to compare two values. The result is always a boolean value (`true` or `false`).

### List of Relational Operators

| Operator | Description              | Example     |
|----------|--------------------------|-------------|
| `<`      | Less than               | `5 < 10`    |
| `>`      | Greater than            | `10 > 5`    |
| `<=`     | Less than or equal to   | `5 <= 10`   |
| `>=`     | Greater than or equal to| `10 >= 5`   |
| `==`     | Equal to                | `5 == 5`    |
| `!=`     | Not equal to            | `5 != 10`   |

### Examples

```java
int a = 15;
int b = 20;
int c = 15;

// Less than
boolean result1 = a < b;   // true (15 < 20)
boolean result2 = b < a;   // false (20 < 15)

// Greater than
boolean result3 = b > a;   // true (20 > 15)
boolean result4 = a > b;   // false (15 > 20)

// Less than or equal to
boolean result5 = a <= c;  // true (15 <= 15)
boolean result6 = a <= b;  // true (15 <= 20)

// Greater than or equal to
boolean result7 = b >= a;  // true (20 >= 15)
boolean result8 = a >= c;  // true (15 >= 15)

// Equal to
boolean result9 = a == c;  // true (15 == 15)
boolean result10 = a == b; // false (15 == 20)

// Not equal to
boolean result11 = a != b; // true (15 != 20)
boolean result12 = a != c; // false (15 != 15)
```

### Practical Example

```java
public class RelationalExample {
    public static void main(String[] args) {
        int score = 85;
        int passingGrade = 60;
        
        boolean passed = score >= passingGrade;
        boolean failed = score < passingGrade;
        boolean perfectScore = score == 100;
        
        System.out.println("Score: " + score);
        System.out.println("Passed: " + passed);        // true
        System.out.println("Failed: " + failed);        // false
        System.out.println("Perfect: " + perfectScore); // false
    }
}
```

---

## Logical Operators

Logical operators are used to combine or modify boolean expressions.

### Types of Logical Operators

| Operator | Name        | Description                                    |
|----------|-------------|------------------------------------------------|
| `&&`     | Logical AND | Returns true if both operands are true        |
| `\|\|`   | Logical OR  | Returns true if at least one operand is true  |
| `!`      | Logical NOT | Returns the opposite boolean value             |

### Logical AND (`&&`)

Returns `true` only when both conditions are `true`.

```java
boolean a = true;
boolean b = false;
boolean c = true;

boolean result1 = a && c;  // true (both are true)
boolean result2 = a && b;  // false (one is false)
boolean result3 = b && c;  // false (one is false)
```

### Logical OR (`||`)

Returns `true` when at least one condition is `true`.

```java
boolean a = true;
boolean b = false;
boolean c = false;

boolean result1 = a || b;  // true (at least one is true)
boolean result2 = b || c;  // false (both are false)
boolean result3 = a || c;  // true (at least one is true)
```

### Logical NOT (`!`)

Returns the opposite boolean value.

```java
boolean a = true;
boolean b = false;

boolean result1 = !a;  // false (opposite of true)
boolean result2 = !b;  // true (opposite of false)
```

### Practical Example

```java
public class LogicalExample {
    public static void main(String[] args) {
        int age = 25;
        boolean hasLicense = true;
        boolean hasInsurance = false;
        
        // Can drive: must have license AND be 18 or older
        boolean canDrive = hasLicense && (age >= 18);
        
        // Can rent car: can drive AND has insurance
        boolean canRentCar = canDrive && hasInsurance;
        
        // Needs attention: doesn't have license OR doesn't have insurance
        boolean needsAttention = !hasLicense || !hasInsurance;
        
        System.out.println("Age: " + age);
        System.out.println("Has License: " + hasLicense);
        System.out.println("Has Insurance: " + hasInsurance);
        System.out.println("Can Drive: " + canDrive);        // true
        System.out.println("Can Rent Car: " + canRentCar);   // false
        System.out.println("Needs Attention: " + needsAttention); // true
    }
}
```

### Short-Circuit Evaluation

Java uses short-circuit evaluation for logical operators:

- **AND (`&&`)**: If the first operand is `false`, the second operand is not evaluated.
- **OR (`||`)**: If the first operand is `true`, the second operand is not evaluated.

```java
int x = 5;
boolean result1 = (x > 10) && (++x > 0); // ++x is not executed
System.out.println("x = " + x); // x is still 5

boolean result2 = (x < 10) || (++x > 0); // ++x is not executed
System.out.println("x = " + x); // x is still 5
```

---

## Bitwise Operators

Bitwise operators work on individual bits of integer types. They perform operations at the bit level.

### List of Bitwise Operators

| Operator | Name        | Description                                    |
|----------|-------------|------------------------------------------------|
| `&`      | Bitwise AND | Sets bit to 1 if both bits are 1              |
| `\|`     | Bitwise OR  | Sets bit to 1 if at least one bit is 1        |
| `^`      | Bitwise XOR | Sets bit to 1 if bits are different           |
| `~`      | Bitwise NOT | Inverts all bits (complement)                 |

### Examples

```java
int a = 5;   // Binary: 0101
int b = 3;   // Binary: 0011

// Bitwise AND
int andResult = a & b;  // 0101 & 0011 = 0001 (1)

// Bitwise OR  
int orResult = a | b;   // 0101 | 0011 = 0111 (7)

// Bitwise XOR
int xorResult = a ^ b;  // 0101 ^ 0011 = 0110 (6)

// Bitwise NOT
int notResult = ~a;     // ~0101 = 1010... (depends on int size)

System.out.println("a & b = " + andResult); // 1
System.out.println("a | b = " + orResult);  // 7
System.out.println("a ^ b = " + xorResult); // 6
```

---

## Ternary Operator

The ternary operator (`? :`) is a shorthand for simple if-else statements. It's also called the conditional operator.

### Syntax
```java
condition ? valueIfTrue : valueIfFalse
```

### Examples

```java
int age = 20;
String status = (age >= 18) ? "Adult" : "Minor";
System.out.println(status); // Output: Adult

// Traditional if-else equivalent:
String status2;
if (age >= 18) {
    status2 = "Adult";
} else {
    status2 = "Minor";
}
```

### More Examples

```java
int a = 10, b = 20;

// Find maximum
int max = (a > b) ? a : b;
System.out.println("Max: " + max); // 20

// Check even or odd
int num = 15;
String evenOdd = (num % 2 == 0) ? "Even" : "Odd";
System.out.println(num + " is " + evenOdd); // 15 is Odd

// Nested ternary (use sparingly for readability)
int score = 85;
String grade = (score >= 90) ? "A" : 
               (score >= 80) ? "B" : 
               (score >= 70) ? "C" : "F";
System.out.println("Grade: " + grade); // Grade: B
```

---

## Assignment Operators

Assignment operators are used to assign values to variables.

### List of Assignment Operators

| Operator | Name                    | Example  | Equivalent To    |
|----------|-------------------------|----------|------------------|
| `=`      | Simple assignment       | `a = 5`  | `a = 5`          |
| `+=`     | Addition assignment     | `a += 3` | `a = a + 3`      |
| `-=`     | Subtraction assignment  | `a -= 3` | `a = a - 3`      |
| `*=`     | Multiplication assignment| `a *= 3` | `a = a * 3`      |
| `/=`     | Division assignment     | `a /= 3` | `a = a / 3`      |
| `%=`     | Modulus assignment      | `a %= 3` | `a = a % 3`      |
| `&=`     | Bitwise AND assignment  | `a &= 3` | `a = a & 3`      |
| `\|=`    | Bitwise OR assignment   | `a \|= 3`| `a = a \| 3`     |
| `^=`     | Bitwise XOR assignment  | `a ^= 3` | `a = a ^ 3`      |

### Examples

```java
int num = 10;

num += 5;  // num = 15
num -= 3;  // num = 12  
num *= 2;  // num = 24
num /= 4;  // num = 6
num %= 4;  // num = 2

System.out.println("Final value: " + num); // 2
```

---

## Unary Operators

Unary operators operate on a single operand.

### List of Unary Operators

| Operator | Name           | Description                              |
|----------|----------------|------------------------------------------|
| `+`      | Unary plus     | Indicates positive value (rarely used)  |
| `-`      | Unary minus    | Negates the value                       |
| `++`     | Increment      | Increases value by 1                    |
| `--`     | Decrement      | Decreases value by 1                    |
| `!`      | Logical NOT    | Inverts boolean value                   |
| `~`      | Bitwise NOT    | Inverts all bits                        |

### Examples

```java
int a = 5;
int b = -10;
boolean flag = true;

// Unary plus (rarely used)
int positive = +a;     // 5

// Unary minus
int negative = -a;     // -5
int makePositive = -b; // 10

// Increment/Decrement (covered earlier)
a++;  // a becomes 6
--a;  // a becomes 5

// Logical NOT
boolean notFlag = !flag; // false

// Bitwise NOT
int complement = ~a;     // Inverts all bits of 5

System.out.println("Original: " + a);        // 5
System.out.println("Negative: " + negative); // -5
System.out.println("Not flag: " + notFlag);  // false
```

---

## Type Comparison Operator (instanceof)

The `instanceof` operator is used to test whether an object is an instance of a specific class or interface.

### Syntax
```java
object instanceof ClassName
```

### Examples

```java
String str = "Hello";
Integer num = 42;
Object obj = "World";

// Check if object is instance of specific class
boolean isString = str instanceof String;    // true
boolean isInteger = num instanceof Integer;  // true
boolean isObject = str instanceof Object;    // true (String extends Object)

// Check with null
String nullStr = null;
boolean isNull = nullStr instanceof String;  // false (null is not instance of anything)

System.out.println("str is String: " + isString);     // true
System.out.println("num is Integer: " + isInteger);   // true
System.out.println("obj is String: " + (obj instanceof String)); // true
```

### Practical Example

```java
public class InstanceofExample {
    public static void main(String[] args) {
        Object[] objects = {"Hello", 123, 45.67, true};
        
        for (Object obj : objects) {
            if (obj instanceof String) {
                System.out.println(obj + " is a String");
            } else if (obj instanceof Integer) {
                System.out.println(obj + " is an Integer");
            } else if (obj instanceof Double) {
                System.out.println(obj + " is a Double");
            } else if (obj instanceof Boolean) {
                System.out.println(obj + " is a Boolean");
            }
        }
    }
}
```

---

## Bit Shift Operators

Bit shift operators move the bits of a number left or right.

### List of Bit Shift Operators

| Operator | Name                        | Description                           |
|----------|-----------------------------|---------------------------------------|
| `<<`     | Left shift                  | Shifts bits left, fills with zeros   |
| `>>`     | Right shift (signed)        | Shifts bits right, preserves sign    |
| `>>>`    | Right shift (unsigned)      | Shifts bits right, fills with zeros  |

### Examples

```java
int num = 8;  // Binary: 1000

// Left shift (multiply by 2^n)
int leftShift1 = num << 1;  // 1000 << 1 = 10000 (16)
int leftShift2 = num << 2;  // 1000 << 2 = 100000 (32)

// Right shift signed (divide by 2^n, preserves sign)
int rightShift1 = num >> 1;  // 1000 >> 1 = 0100 (4)
int rightShift2 = num >> 2;  // 1000 >> 2 = 0010 (2)

// Right shift unsigned
int negativeNum = -8;
int unsignedRightShift = negativeNum >>> 1;  // Fills with zeros

System.out.println("Original: " + num);              // 8
System.out.println("Left shift 1: " + leftShift1);   // 16
System.out.println("Left shift 2: " + leftShift2);   // 32
System.out.println("Right shift 1: " + rightShift1); // 4
System.out.println("Right shift 2: " + rightShift2); // 2
```

### Practical Use Cases

```java
// Quick multiplication/division by powers of 2
int multiply4 = 5 << 2;    // 5 * 4 = 20 (faster than 5 * 4)
int divide8 = 40 >> 3;     // 40 / 8 = 5 (faster than 40 / 8)

// Check if number is even (last bit is 0)
boolean isEven = (num & 1) == 0;

// Set specific bit
int setBit = num | (1 << 2);  // Set bit at position 2

// Clear specific bit  
int clearBit = num & ~(1 << 2);  // Clear bit at position 2
```

---

## Summary

Java provides a comprehensive set of operators for different purposes:

- **Arithmetic Operators**: Mathematical operations (`+`, `-`, `*`, `/`, `%`, `++`, `--`)
- **Relational Operators**: Compare values and return boolean results (`<`, `>`, `<=`, `>=`, `==`, `!=`)
- **Logical Operators**: Combine boolean expressions (`&&`, `||`, `!`)
- **Bitwise Operators**: Operate on individual bits (`&`, `|`, `^`, `~`)
- **Ternary Operator**: Shorthand conditional (`? :`)
- **Assignment Operators**: Assign values with optional operations (`=`, `+=`, `-=`, etc.)
- **Unary Operators**: Operate on single operands (`+`, `-`, `++`, `--`, `!`, `~`)
- **Type Comparison**: Check object types (`instanceof`)
- **Bit Shift Operators**: Move bits left or right (`<<`, `>>`, `>>>`)

### Operator Precedence (Highest to Lowest)

1. Postfix: `expr++`, `expr--`
2. Unary: `++expr`, `--expr`, `+expr`, `-expr`, `~`, `!`
3. Multiplicative: `*`, `/`, `%`
4. Additive: `+`, `-`
5. Shift: `<<`, `>>`, `>>>`
6. Relational: `<`, `>`, `<=`, `>=`, `instanceof`
7. Equality: `==`, `!=`
8. Bitwise AND: `&`
9. Bitwise XOR: `^`
10. Bitwise OR: `|`
11. Logical AND: `&&`
12. Logical OR: `||`
13. Ternary: `? :`
14. Assignment: `=`, `+=`, `-=`, etc.

Understanding these operators is fundamental to Java programming as they form the building blocks for conditions, loops, and mathematical calculations in your programs.

## Practice Exercises

Try implementing these concepts in your own Java programs:

1. Create a program that demonstrates the difference between pre and post increment
2. Write a condition checker using relational operators
3. Combine multiple conditions using logical operators
4. Use bitwise operators to perform bit manipulation
5. Replace if-else statements with ternary operators
6. Create a type checker using `instanceof`
7. Demonstrate bit shifting for fast multiplication/division
8. Build a calculator using various assignment operators

### Challenge: Complete Operator Demo

```java
public class OperatorDemo {
    public static void main(String[] args) {
        // Try combining multiple operator types in meaningful ways
        int a = 10, b = 3;
        boolean flag = true;
        
        // Arithmetic + Ternary
        String result = (a % b == 0) ? "Divisible" : "Not divisible";
        
        // Bitwise + Assignment
        int bitwiseResult = a;
        bitwiseResult &= b;  // Combine assignment with bitwise
        
        // Logical + Relational
        boolean complexCondition = (a > b) && (flag || (a << 1) < 25);
        
        System.out.println("Experiment with these combinations!");
    }
}
```

Happy coding! 🚀
