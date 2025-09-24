# Packages in Java

## What are Packages?

When working on Java projects, we often have multiple files. To keep our code organized and manageable, we can organize these files into folders. In Java, these folders are called **packages**.

Packages serve as containers for related classes and interfaces, helping us:
- Organize our code logically
- Avoid naming conflicts
- Control access to classes
- Create a hierarchical structure

## Creating a Package

To create a package, we use the `package` keyword followed by the package name at the top of our Java file:

```java
package packagename;

public class MyClass {
    // class content
}
```

## Package Naming Conventions

- Package names are written in lowercase
- Use dots (.) to separate package levels
- Follow reverse domain naming convention for uniqueness

```java
package com.company.projectname;
package org.example.utilities;
```

## Importing Classes from Other Packages

When we need to use a class that exists outside its current package, we need to **import** it using the `import` keyword:

```java
import packagename.ClassName;

// Example
import java.util.Scanner;
import java.util.ArrayList;
```

## Important Facts About Packages

### Every Class Belongs to a Package
- Every class in Java belongs to a package
- If no package is specified, the class belongs to the **default package**

### Importing Multiple Classes
We can use the asterisk (*) wildcard to import all classes from a package:

```java
import java.util.*;  // imports all classes from java.util package
```

### Default Import
Every Java class automatically imports `java.lang.*` by default. This package contains fundamental classes like:
- `String`
- `System`
- `Object`
- `Integer`, `Double`, etc.

## Example Structure

```
src/
├── com/
│   └── mycompany/
│       └── myproject/
│           ├── Main.java
│           ├── models/
│           │   └── User.java
│           └── utils/
│               └── Helper.java
```

## Complete Example

**File: com/mycompany/myproject/models/User.java**
```java
package com.mycompany.myproject.models;

public class User {
    private String name;
    
    public User(String name) {
        this.name = name;
    }
    
    public String getName() {
        return name;
    }
}
```

**File: com/mycompany/myproject/Main.java**
```java
package com.mycompany.myproject;

import com.mycompany.myproject.models.User;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        User user = new User("John Doe");
        Scanner scanner = new Scanner(System.in);
        
        System.out.println("User: " + user.getName());
    }
}
```

## Key Takeaways

1. Packages help organize Java code into logical folders
2. Use `package packagename;` to declare a package
3. Use `import packagename.ClassName;` to use classes from other packages
4. Every class belongs to a package (default package if not specified)
5. Use `import packagename.*;` to import all classes from a package
6. `java.lang.*` is automatically imported in every Java class
