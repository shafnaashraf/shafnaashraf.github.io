# Java Arrays Tutorial

## What is an Array?

An **array** is a data structure that allows a variable to store multiple values of the same type. Arrays are essential for handling collections of data efficiently in Java.

## 1-Dimensional Arrays

### Array Declaration and Initialization

Arrays in Java can be declared and initialized in several ways:

```java
// Method 1: Direct initialization
int num[] = {1, 2, 3, 4};

// Method 2: Declaration with size specification
int num[] = new int[4];

// Method 3: Alternative syntax
int[] num = new int[4];
```

### Key Points about 1D Arrays

- **Fixed Size**: Once the size is declared, it cannot be changed
- **Zero-based Indexing**: Array numbering starts with 0
  - `nums[0]` gives the first element
  - `nums[1]` gives the second element
- **Element Assignment**: You can assign values to specific positions

```java
num[2] = 3; // This sets the 3rd element (index 2) to value '3'
```

### Iterating Through Arrays

You can use loops to traverse through array elements:

```java
// Traditional for loop
for(int i = 0; i < num.length; i++) {
    System.out.println(num[i]);
}
```

## Multi-Dimensional Arrays

### 2D Array Declaration

Multi-dimensional arrays are essentially **arrays of arrays**:

```java
// Creates a 2D array with 3 rows and 4 columns
int nums[][] = new int[3][4];
```

### Nested Loop Iteration

```java
// Traditional nested for loops
for(int i = 0; i < 3; i++) {
    for(int j = 0; j < 4; j++) {
        System.out.print(nums[i][j] + " ");
    }
    System.out.println(); // New line after each row
}
```

### Enhanced For Loop for 2D Arrays

```java
// Enhanced for loop (for-each)
for(int n[] : nums) {           // Iterate through each row
    for(int number : n) {       // Iterate through each element in the row
        System.out.print(number + " ");
    }
    System.out.println();
}
```

## Jagged Arrays

Java supports **jagged arrays** where internal arrays can have different lengths:

```java
// Declare a 2D array with 3 rows but unspecified column sizes
int nums[][] = new int[3][];

// Assign different sizes to each row
nums[0] = new int[3];  // First row has 3 elements
nums[1] = new int[1];  // Second row has 1 element
nums[2] = new int[5];  // Third row has 5 elements
```

## Enhanced For Loops (For-Each)

### Syntax
```java
for(dataType variableName : collection/arrayName) {
    // Code to execute for each element
}
```

### Examples

```java
// 1D Array
int[] numbers = {1, 2, 3, 4, 5};
for(int num : numbers) {
    System.out.println(num);
}

// String array
String[] names = {"Alice", "Bob", "Charlie"};
for(String name : names) {
    System.out.println(name);
}
```

### Benefits of Enhanced For Loop
- **Cleaner syntax**: No need for index management
- **Error-proof**: Eliminates index out of bounds errors
- **Readable**: More intuitive for simple iterations

### Limitations
- **Read-only**: Cannot modify array elements directly
- **No index access**: Cannot get the current index during iteration

## Array of Objects

### Real-world Example: Student Array

```java
// Create Student objects
Student s1 = new Student("John", 20);
Student s2 = new Student("Jane", 19);
Student s3 = new Student("Bob", 21);

// Create array of Student objects
Student students[] = new Student[3];

// Assign objects to array
students[0] = s1;
students[1] = s2;
students[2] = s3;

// Alternative direct initialization
Student students[] = {s1, s2, s3};
```

### Iterating Through Object Arrays

```java
// Traditional for loop
for(int i = 0; i < students.length; i++) {
    System.out.println(students[i].getName());
}

// Enhanced for loop
for(Student student : students) {
    System.out.println(student.getName());
}
```

## Advantages of Arrays

1. **Fast Access**: Direct access to elements using index - O(1) time complexity
2. **Memory Efficient**: Elements are stored in contiguous memory locations
3. **Simple**: Easy to understand and implement
4. **Cache Performance**: Better cache locality due to contiguous memory

## Disadvantages of Arrays

1. **Fixed Size**: Once specified, size cannot be expanded or reduced
2. **Insertion/Deletion Difficulty**: Adding or removing elements requires shifting
3. **Searching**: Linear search is required for unsorted arrays - O(n) time complexity
4. **Same Data Type**: Can only store elements of the same type
5. **Memory Waste**: If array is not fully utilized, memory is wasted

## Related Concepts

### Array vs ArrayList
- **Array**: Fixed size, faster access, primitive type support
- **ArrayList**: Dynamic size, more methods, only objects

### Memory Layout
Arrays store elements in contiguous memory locations, which provides:
- Better cache performance
- Predictable memory usage
- Direct memory access through pointer arithmetic

### Common Array Operations
```java
// Length of array
int size = arr.length;

// Copy array
int[] copy = Arrays.copyOf(original, original.length);

// Sort array
Arrays.sort(arr);

// Search in sorted array
int index = Arrays.binarySearch(arr, target);

// Fill array with value
Arrays.fill(arr, value);
```

## Best Practices

1. **Use meaningful names**: `studentGrades` instead of `arr`
2. **Check bounds**: Always validate array indices
3. **Use enhanced for loops**: When you don't need the index
4. **Consider ArrayList**: For dynamic sizing requirements
5. **Initialize properly**: Avoid null pointer exceptions with object arrays

## Conclusion

Arrays are fundamental data structures in Java that provide efficient storage and access to collections of elements. While they have limitations like fixed size, they remain essential for performance-critical applications and serve as building blocks for more complex data structures.
