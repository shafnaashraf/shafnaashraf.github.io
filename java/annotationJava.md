# Java Annotations Tutorial

## What are Annotations?

Annotations in Java are a form of **metadata** that provide supplemental information to the compiler or runtime environment. They don't directly affect the execution of your code but can influence how your code is processed by tools, frameworks, or the Java runtime.

## Basic Example

```java
class ClassA {
    public void show() {
        System.out.println("Method in ClassA");
    }
}

class ClassB extends ClassA {
    @Override
    public void show() {
        System.out.println("Method in ClassB");
    }
}

public class Main {
    public static void main(String[] args) {
        ClassB obj = new ClassB();
        obj.show(); // Output: Method in ClassB
    }
}
```

## Common Built-in Annotations

### @Override
- **Purpose**: Indicates that a method is overriding a method from its superclass
- **Benefits**: 
  - Helps catch errors at compile time
  - Improves code readability
  - Ensures method signature matches parent class

```java
class Parent {
    public void display() { }
}

class Child extends Parent {
    @Override
    public void display() {
        // This method overrides the parent's display() method
    }
}
```

### @Deprecated
- **Purpose**: Marks a class, method, or field as deprecated
- **Usage**: Indicates that the element should not be used as it may be removed in future versions
- **Effect**: Compiler shows warnings when deprecated elements are used

```java
public class Example {
    @Deprecated
    public void oldMethod() {
        System.out.println("This method is deprecated");
    }
    
    public void newMethod() {
        System.out.println("Use this method instead");
    }
}
```

### @SuppressWarnings
- **Purpose**: Suppresses specific compiler warnings
- **Common values**:
  - `"unchecked"` - suppresses unchecked warnings
  - `"deprecation"` - suppresses deprecation warnings
  - `"unused"` - suppresses unused variable warnings

```java
public class WarningExample {
    @SuppressWarnings("deprecation")
    public void useDeprecatedMethod() {
        // This won't show deprecation warnings
        oldMethod();
    }
    
    @SuppressWarnings({"unchecked", "unused"})
    public void multipleSuppressions() {
        // Suppresses multiple types of warnings
    }
}
```

## Annotation Retention Levels

Annotations can exist at different levels depending on when they're needed:

### 1. SOURCE Level
- Available only in source code
- Discarded by compiler
- Example: `@SuppressWarnings`

### 2. CLASS Level
- Available in source code and bytecode
- Not available at runtime
- Default retention level

### 3. RUNTIME Level
- Available in source code, bytecode, and at runtime
- Can be accessed using reflection
- Example: `@Deprecated`

```java
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation {
    String value();
}
```

## Framework-Specific Annotations

### Spring Framework
- `@Component` - Marks a class as a Spring component
- `@Service` - Specialization of @Component for service layer
- `@Repository` - Specialization of @Component for data access layer
- `@Controller` - Specialization of @Component for presentation layer
- `@Autowired` - Automatic dependency injection

### JPA/Hibernate
- `@Entity` - Marks a class as a JPA entity
- `@Table` - Specifies database table details
- `@Id` - Marks primary key field
- `@Column` - Maps field to database column
- `@Transient` - Excludes field from database mapping

### Lombok
- `@Data` - Generates getters, setters, toString, equals, and hashCode
- `@Getter/@Setter` - Generates getter/setter methods
- `@NoArgsConstructor/@AllArgsConstructor` - Generates constructors

## Custom Annotations Example

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface LogExecutionTime {
    String value() default "";
}

// Usage
public class ServiceClass {
    @LogExecutionTime("Database operation")
    public void performDatabaseOperation() {
        // Method implementation
    }
}
```

## Key Points to Remember

1. **Annotations are metadata** - They don't change program behavior directly
2. **Multiple retention levels** - SOURCE, CLASS, and RUNTIME
3. **Built-in annotations** help with code quality and compiler checks
4. **Framework annotations** enable powerful features like dependency injection
5. **Custom annotations** can be created for specific needs
6. **Reflection** can be used to process runtime annotations

## Best Practices

- Use `@Override` consistently when overriding methods
- Be cautious with `@SuppressWarnings` - don't hide important warnings
- Understand retention policies when creating custom annotations
- Use framework annotations appropriately (Spring, JPA, etc.)
- Document custom annotations well for team understanding

---

*This tutorial covers the fundamental concepts of Java annotations. For more advanced topics, explore custom annotation processing and specific framework documentation.*
