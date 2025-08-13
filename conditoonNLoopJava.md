# Java Conditional Statements and Loops Tutorial

Welcome to this comprehensive guide on Java conditional statements and loops! These are fundamental building blocks that control the flow of your programs.

## Table of Contents
1. [Why Conditional Statements and Loops are Needed](#why-conditional-statements-and-loops-are-needed)
2. [Conditional Statements](#conditional-statements)
   - [if Statement](#if-statement)
   - [if-else Statement](#if-else-statement)
   - [else if Statement](#else-if-statement)
   - [Ternary Operator](#ternary-operator)
   - [switch Statement](#switch-statement)
3. [Loops](#loops)
   - [while Loop](#while-loop)
   - [do-while Loop](#do-while-loop)
   - [for Loop](#for-loop)
   - [Enhanced for Loop (for-each)](#enhanced-for-loop-for-each)
   - [Nested Loops](#nested-loops)
4. [Loop Control Statements](#loop-control-statements)
5. [Real-World Scenarios and Best Practices](#real-world-scenarios-and-best-practices)
6. [Performance Considerations](#performance-considerations)

---

## Why Conditional Statements and Loops are Needed

**Conditional Statements** are essential because:
- Programs need to make decisions based on different conditions
- Real-world scenarios require different actions for different situations
- They enable dynamic behavior rather than fixed sequential execution
- Essential for validation, error handling, and user interaction

**Loops** are crucial because:
- They eliminate code repetition (DRY principle - Don't Repeat Yourself)
- Enable processing of large datasets efficiently
- Allow programs to handle unknown quantities of data
- Essential for iterating through collections, arrays, and data structures
- Enable automation of repetitive tasks

**Example of why we need them:**
```java
// Without conditionals and loops - INEFFICIENT and LIMITED
System.out.println("Student 1 grade: A");
System.out.println("Student 2 grade: B");
System.out.println("Student 3 grade: C");
// ... What if we have 1000 students?

// With conditionals and loops - EFFICIENT and SCALABLE
for (int i = 0; i < students.length; i++) {
    if (students[i].getScore() >= 90) {
        System.out.println("Student " + (i+1) + " grade: A");
    } else if (students[i].getScore() >= 80) {
        System.out.println("Student " + (i+1) + " grade: B");
    } // ... and so on
}
```

---

## Conditional Statements

Conditional statements allow your program to execute different code blocks based on specific conditions.

### if Statement

The most basic conditional statement. Executes code only if the condition is true.

#### Syntax
```java
if (condition) {
    // Code to execute if condition is true
}
```

#### Examples
```java
int temperature = 25;

if (temperature > 30) {
    System.out.println("It's hot outside!");
}

// Multiple statements
int age = 20;
if (age >= 18) {
    System.out.println("You are eligible to vote");
    System.out.println("You can also get a driver's license");
}

// Single line (braces optional for single statement)
if (age >= 21) 
    System.out.println("You can drink alcohol in the US");
```

### if-else Statement

Provides an alternative path when the condition is false.

#### Syntax
```java
if (condition) {
    // Code if condition is true
} else {
    // Code if condition is false
}
```

#### Examples
```java
int score = 75;

if (score >= 60) {
    System.out.println("You passed!");
} else {
    System.out.println("You failed. Better luck next time.");
}

// Real-world example: Login validation
String username = "admin";
String password = "123456";

if (username.equals("admin") && password.equals("123456")) {
    System.out.println("Login successful!");
    System.out.println("Welcome to the dashboard");
} else {
    System.out.println("Invalid credentials!");
    System.out.println("Please try again");
}
```

### else if Statement

Handles multiple conditions in sequence.

#### Syntax
```java
if (condition1) {
    // Code if condition1 is true
} else if (condition2) {
    // Code if condition2 is true
} else if (condition3) {
    // Code if condition3 is true
} else {
    // Code if none of the conditions are true
}
```

#### Examples
```java
int marks = 85;

if (marks >= 90) {
    System.out.println("Grade: A+");
} else if (marks >= 80) {
    System.out.println("Grade: A");
} else if (marks >= 70) {
    System.out.println("Grade: B");
} else if (marks >= 60) {
    System.out.println("Grade: C");
} else {
    System.out.println("Grade: F");
}

// Traffic light example
String lightColor = "yellow";

if (lightColor.equals("red")) {
    System.out.println("Stop");
} else if (lightColor.equals("yellow")) {
    System.out.println("Caution - prepare to stop");
} else if (lightColor.equals("green")) {
    System.out.println("Go");
} else {
    System.out.println("Invalid traffic light color");
}
```

### Ternary Operator

A shorthand way to write simple if-else statements in one line.

#### Syntax
```java
variable = (condition) ? valueIfTrue : valueIfFalse;
```

#### Examples
```java
int age = 17;
String eligibility = (age >= 18) ? "Can vote" : "Cannot vote";
System.out.println(eligibility);

// Multiple ternary operators (use sparingly for readability)
int score = 85;
String grade = (score >= 90) ? "A" : 
               (score >= 80) ? "B" : 
               (score >= 70) ? "C" : "F";

// In method calls
int a = 10, b = 5;
System.out.println("Maximum: " + ((a > b) ? a : b));

// Real-world example: Discount calculation
double price = 100.0;
boolean isMember = true;
double finalPrice = isMember ? (price * 0.9) : price; // 10% discount for members
```

**When to use ternary instead of if-else:**
- Simple conditions with single assignments
- When you want concise code
- Avoid for complex conditions (reduces readability)

### switch Statement

Efficient way to handle multiple discrete values of a variable.

#### Syntax
```java
switch (variable) {
    case value1:
        // Code for value1
        break;
    case value2:
        // Code for value2
        break;
    case value3:
        // Code for value3
        break;
    default:
        // Code when no case matches
        break;
}
```

#### Why `break` is necessary:
Without `break`, execution "falls through" to the next case, which is usually not desired.

#### Examples
```java
int dayOfWeek = 3;

switch (dayOfWeek) {
    case 1:
        System.out.println("Monday - Start of work week");
        break;
    case 2:
        System.out.println("Tuesday - Getting into rhythm");
        break;
    case 3:
        System.out.println("Wednesday - Hump day!");
        break;
    case 4:
        System.out.println("Thursday - Almost there");
        break;
    case 5:
        System.out.println("Friday - TGIF!");
        break;
    case 6:
    case 7:
        System.out.println("Weekend - Time to relax!");
        break;
    default:
        System.out.println("Invalid day");
        break;
}

// Calculator example
char operator = '+';
double num1 = 10.0, num2 = 5.0;
double result = 0;

switch (operator) {
    case '+':
        result = num1 + num2;
        break;
    case '-':
        result = num1 - num2;
        break;
    case '*':
        result = num1 * num2;
        break;
    case '/':
        if (num2 != 0) {
            result = num1 / num2;
        } else {
            System.out.println("Cannot divide by zero!");
            return;
        }
        break;
    default:
        System.out.println("Invalid operator");
        return;
}

System.out.println("Result: " + result);
```

#### Fall-through example (intentional):
```java
int month = 2;
int year = 2024;
int daysInMonth;

switch (month) {
    case 1: case 3: case 5: case 7: case 8: case 10: case 12:
        daysInMonth = 31;
        break;
    case 4: case 6: case 9: case 11:
        daysInMonth = 30;
        break;
    case 2:
        daysInMonth = (year % 4 == 0) ? 29 : 28; // Leap year check
        break;
    default:
        daysInMonth = -1; // Invalid month
        break;
}
```

---

## Loops

Loops allow you to repeat certain blocks of code based on conditions, eliminating redundancy and enabling efficient processing.

### while Loop

Repeats code as long as the condition remains true. The condition is checked before each iteration.

#### Syntax
```java
while (condition) {
    // Code to repeat
    // Update statement (important to avoid infinite loop)
}
```

#### Examples
```java
// Basic counting
int count = 1;
while (count <= 5) {
    System.out.println("Count: " + count);
    count++; // Important: update the condition variable
}

// Input validation example
Scanner scanner = new Scanner(System.in);
int number;
while (true) {
    System.out.print("Enter a positive number: ");
    number = scanner.nextInt();
    if (number > 0) {
        break; // Exit the loop
    }
    System.out.println("Please enter a positive number!");
}

// Processing user input until quit
String input;
while (!(input = scanner.nextLine()).equals("quit")) {
    System.out.println("You entered: " + input);
    System.out.print("Enter another word (or 'quit' to exit): ");
}
```

### do-while Loop

Similar to while loop, but the condition is checked after each iteration, guaranteeing at least one execution.

#### Why is do-while needed?
- When you need to execute the loop body at least once
- Menu systems, input validation, game loops
- Situations where you need to perform an action before checking the condition

#### Syntax
```java
do {
    // Code to repeat (executes at least once)
    // Update statement
} while (condition);
```

#### Examples
```java
// Menu system - always show menu at least once
Scanner scanner = new Scanner(System.in);
int choice;

do {
    System.out.println("\n--- Menu ---");
    System.out.println("1. Add");
    System.out.println("2. Delete");
    System.out.println("3. View");
    System.out.println("4. Exit");
    System.out.print("Enter your choice: ");
    choice = scanner.nextInt();
    
    switch (choice) {
        case 1:
            System.out.println("Adding item...");
            break;
        case 2:
            System.out.println("Deleting item...");
            break;
        case 3:
            System.out.println("Viewing items...");
            break;
        case 4:
            System.out.println("Exiting...");
            break;
        default:
            System.out.println("Invalid choice!");
    }
} while (choice != 4);

// Password validation - ask at least once
String password;
do {
    System.out.print("Enter password: ");
    password = scanner.nextLine();
    if (password.length() < 8) {
        System.out.println("Password must be at least 8 characters long!");
    }
} while (password.length() < 8);
```

### for Loop

Most commonly used when you know the number of iterations in advance. Contains initialization, condition, and increment/decrement in one line.

#### Syntax
```java
for (initialization; condition; increment/decrement) {
    // Code to repeat
}
```

#### Components:
- **Initialization**: Executed once at the beginning
- **Condition**: Checked before each iteration
- **Increment/Decrement**: Executed after each iteration

#### Examples
```java
// Basic counting
for (int i = 1; i <= 10; i++) {
    System.out.println("Number: " + i);
}

// Countdown
for (int i = 10; i >= 1; i--) {
    System.out.println("Countdown: " + i);
}
System.out.println("Blast off!");

// Array processing
int[] numbers = {10, 20, 30, 40, 50};
for (int i = 0; i < numbers.length; i++) {
    System.out.println("Index " + i + ": " + numbers[i]);
}

// Sum of numbers
int sum = 0;
for (int i = 1; i <= 100; i++) {
    sum += i;
}
System.out.println("Sum of 1 to 100: " + sum);

// Multiplication table
int number = 5;
for (int i = 1; i <= 10; i++) {
    System.out.println(number + " x " + i + " = " + (number * i));
}
```

### Enhanced for Loop (for-each)

Simplified way to iterate through arrays and collections without dealing with indices.

#### Syntax
```java
for (dataType variable : array/collection) {
    // Code using variable
}
```

#### Examples
```java
// Array iteration
int[] numbers = {1, 2, 3, 4, 5};
for (int num : numbers) {
    System.out.println("Number: " + num);
}

// String array
String[] names = {"Alice", "Bob", "Charlie"};
for (String name : names) {
    System.out.println("Hello, " + name);
}

// Collection iteration
List<String> fruits = Arrays.asList("apple", "banana", "orange");
for (String fruit : fruits) {
    System.out.println("Fruit: " + fruit);
}
```

**When to use enhanced for loop:**
- When you don't need the index
- When iterating through entire collection
- Makes code cleaner and less error-prone
- Cannot modify the array/collection during iteration

### Nested Loops

Loops inside other loops, useful for multi-dimensional processing.

#### Examples
```java
// Multiplication table
for (int i = 1; i <= 5; i++) {
    for (int j = 1; j <= 5; j++) {
        System.out.print((i * j) + "\t");
    }
    System.out.println(); // New line after each row
}

// Pattern printing
for (int i = 1; i <= 5; i++) {
    for (int j = 1; j <= i; j++) {
        System.out.print("* ");
    }
    System.out.println();
}
// Output:
// *
// * *
// * * *
// * * * *
// * * * * *

// 2D array processing
int[][] matrix = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};
for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        System.out.print(matrix[i][j] + " ");
    }
    System.out.println();
}
```

---

## Loop Control Statements

### break Statement
Exits the loop immediately when encountered.

```java
// Find first even number
for (int i = 1; i <= 10; i++) {
    if (i % 2 == 0) {
        System.out.println("First even number: " + i);
        break; // Exit the loop
    }
}

// Search in array
int[] numbers = {3, 7, 2, 9, 5};
int target = 7;
boolean found = false;

for (int num : numbers) {
    if (num == target) {
        found = true;
        break;
    }
}
System.out.println("Number " + target + " found: " + found);
```

### continue Statement
Skips the current iteration and moves to the next one.

```java
// Print odd numbers only
for (int i = 1; i <= 10; i++) {
    if (i % 2 == 0) {
        continue; // Skip even numbers
    }
    System.out.println("Odd number: " + i);
}

// Process valid data only
String[] data = {"apple", "", "banana", null, "cherry"};
for (String item : data) {
    if (item == null || item.isEmpty()) {
        continue; // Skip invalid data
    }
    System.out.println("Processing: " + item);
}
```

### return Statement
Exits the entire method, not just the loop.

```java
public static int findIndex(int[] array, int target) {
    for (int i = 0; i < array.length; i++) {
        if (array[i] == target) {
            return i; // Exit method and return index
        }
    }
    return -1; // Not found
}
```

---

## Real-World Scenarios and Best Practices

### When to Use Which Loop

#### Use `for` loop when:
- You know the exact number of iterations
- Working with arrays/collections with indices
- Need a counter variable
- **Examples**: Processing arrays, generating sequences, countdown timers

```java
// File processing
String[] files = getFileList();
for (int i = 0; i < files.length; i++) {
    processFile(files[i]);
    updateProgressBar(i + 1, files.length);
}
```

#### Use `enhanced for` loop when:
- Iterating through collections without needing indices
- Code simplicity is important
- **Examples**: Data processing, validation, display

```java
// Validate all user inputs
List<String> userInputs = getUserInputs();
for (String input : userInputs) {
    if (!isValid(input)) {
        showError("Invalid input: " + input);
    }
}
```

#### Use `while` loop when:
- Number of iterations is unknown
- Loop depends on external conditions
- **Examples**: Reading files, user input, network operations

```java
// Read file until end
BufferedReader reader = new BufferedReader(new FileReader("data.txt"));
String line;
while ((line = reader.readLine()) != null) {
    processLine(line);
}

// Game loop
while (gameRunning && player.isAlive()) {
    updateGame();
    renderFrame();
    handleInput();
}
```

#### Use `do-while` loop when:
- Need to execute at least once
- Menu systems, validation loops
- **Examples**: User interfaces, input validation

```java
// ATM machine
do {
    displayMenu();
    choice = getUserChoice();
    processChoice(choice);
} while (choice != EXIT_OPTION);
```

### Performance Considerations

```java
// GOOD: Pre-calculate array length
int[] array = getLargeArray();
int length = array.length; // Calculate once
for (int i = 0; i < length; i++) {
    process(array[i]);
}

// AVOID: Recalculating in loop condition
for (int i = 0; i < array.length; i++) { // array.length calculated each iteration
    process(array[i]);
}

// GOOD: Enhanced for loop is often more efficient
for (int value : array) {
    process(value);
}
```

### Common Patterns and Best Practices

#### Input Validation Pattern
```java
Scanner scanner = new Scanner(System.in);
int age;

do {
    System.out.print("Enter your age (1-120): ");
    while (!scanner.hasNextInt()) {
        System.out.print("Please enter a valid number: ");
        scanner.next(); // Consume invalid input
    }
    age = scanner.nextInt();
    
    if (age < 1 || age > 120) {
        System.out.println("Age must be between 1 and 120!");
    }
} while (age < 1 || age > 120);
```

#### Search Pattern
```java
public static int linearSearch(int[] array, int target) {
    for (int i = 0; i < array.length; i++) {
        if (array[i] == target) {
            return i; // Found at index i
        }
    }
    return -1; // Not found
}
```

#### Accumulator Pattern
```java
// Calculate average
double[] scores = {85.5, 92.0, 78.5, 96.0, 88.5};
double sum = 0;
for (double score : scores) {
    sum += score;
}
double average = sum / scores.length;
```

#### Flag Pattern
```java
boolean hasNegative = false;
int[] numbers = {1, 5, -3, 8, 2};

for (int num : numbers) {
    if (num < 0) {
        hasNegative = true;
        break; // Found what we're looking for
    }
}

if (hasNegative) {
    System.out.println("Array contains negative numbers");
}
```

### Advanced Concepts

#### Labeled Loops (for breaking out of nested loops)
```java
outer: for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (i == 1 && j == 1) {
            break outer; // Break out of both loops
        }
        System.out.println("i=" + i + ", j=" + j);
    }
}
```

#### Infinite Loops (intentional)
```java
// Server application
while (true) {
    Request request = waitForRequest();
    if (request == null) continue;
    
    if (request.isShutdown()) {
        break; // Proper way to exit
    }
    
    processRequest(request);
}
```

---

## Common Mistakes and How to Avoid Them

### 1. Infinite Loops
```java
// WRONG: Forgetting to update loop variable
int i = 0;
while (i < 10) {
    System.out.println(i);
    // Missing: i++; This creates infinite loop!
}

// CORRECT:
int i = 0;
while (i < 10) {
    System.out.println(i);
    i++;
}
```

### 2. Off-by-One Errors
```java
// WRONG: Going beyond array bounds
int[] array = new int[10];
for (int i = 0; i <= array.length; i++) { // <= instead of <
    array[i] = i; // ArrayIndexOutOfBoundsException!
}

// CORRECT:
for (int i = 0; i < array.length; i++) {
    array[i] = i;
}
```

### 3. Missing Break in Switch
```java
// WRONG: Missing break causes fall-through
switch (grade) {
    case 'A':
        System.out.println("Excellent!");
        // Missing break - falls through to case 'B'
    case 'B':
        System.out.println("Good job!");
        break;
}

// CORRECT:
switch (grade) {
    case 'A':
        System.out.println("Excellent!");
        break; // Important!
    case 'B':
        System.out.println("Good job!");
        break;
}
```

---

## Summary

Understanding conditional statements and loops is crucial for writing effective Java programs:

- **Conditionals** enable decision-making in programs
- **if-else** for binary decisions, **else if** for multiple conditions
- **Ternary operator** for simple conditional assignments
- **switch** for multiple discrete values (remember the `break`!)
- **Loops** eliminate code repetition and enable data processing
- **while** for unknown iterations, **do-while** for at least-once execution
- **for** for known iterations, **enhanced for** for clean collection iteration
- Choose the right loop based on your specific needs
- Always consider performance and readability
- Be careful with infinite loops and boundary conditions

Practice these concepts with real-world examples to master program flow control! 🚀
