# Enumeration Type - Enum in Java

## Table of Contents
- [What is Enum?](#what-is-enum)
- [Basic Enum Declaration](#basic-enum-declaration)
- [Enum Methods](#enum-methods)
  - [ordinal() Method](#ordinal-method)
  - [values() Method](#values-method)
- [Accessing Enum Elements](#accessing-enum-elements)
  - [Enhanced For Loop](#enhanced-for-loop)
- [Control Structures with Enum](#control-structures-with-enum)
  - [Using if-else with Enum](#using-if-else-with-enum)
  - [Using switch-case with Enum](#using-switch-case-with-enum)
- [Enum Inheritance and Structure](#enum-inheritance-and-structure)
- [Enum Constructors and Methods](#enum-constructors-and-methods)

## What is Enum?

An **enum** (enumeration) in Java is a special class that represents a group of **named constants**. Enums provide a type-safe way to represent fixed sets of constants and are more powerful than traditional constant declarations.

## Basic Enum Declaration

```java
public enum Day {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
}

public enum Status {
    ACTIVE, INACTIVE, PENDING, CANCELLED
}
```

## Enum Methods

### ordinal() Method

The `ordinal()` method returns the **index** (position) of the enum constant, starting from 0.

```java
public enum Color {
    RED, GREEN, BLUE, YELLOW
}

public class EnumExample {
    public static void main(String[] args) {
        System.out.println(Color.RED.ordinal());    // Output: 0
        System.out.println(Color.GREEN.ordinal());  // Output: 1
        System.out.println(Color.BLUE.ordinal());   // Output: 2
        System.out.println(Color.YELLOW.ordinal()); // Output: 3
    }
}
```

### values() Method

The `values()` method returns an array containing **all the values** of the enum in the order they are declared.

```java
public class EnumExample {
    public static void main(String[] args) {
        Color[] colors = Color.values();
        
        System.out.println("All colors:");
        for (Color color : colors) {
            System.out.println(color + " - Index: " + color.ordinal());
        }
    }
}
```

**Output:**
```
All colors:
RED - Index: 0
GREEN - Index: 1
BLUE - Index: 2
YELLOW - Index: 3
```

## Accessing Enum Elements

### Enhanced For Loop

The **enhanced for loop** (for-each loop) is commonly used to iterate through enum values.

```java
public enum Priority {
    LOW, MEDIUM, HIGH, CRITICAL
}

public class EnumIteration {
    public static void main(String[] args) {
        // Using enhanced for loop
        System.out.println("Priority levels:");
        for (Priority p : Priority.values()) {
            System.out.println("Priority: " + p + ", Ordinal: " + p.ordinal());
        }
    }
}
```

## Control Structures with Enum

### Using if-else with Enum

```java
public enum TrafficLight {
    RED, YELLOW, GREEN
}

public class TrafficController {
    public static void checkTrafficLight(TrafficLight light) {
        if (light == TrafficLight.RED) {
            System.out.println("Stop!");
        } else if (light == TrafficLight.YELLOW) {
            System.out.println("Caution!");
        } else if (light == TrafficLight.GREEN) {
            System.out.println("Go!");
        }
    }
    
    public static void main(String[] args) {
        checkTrafficLight(TrafficLight.RED);    // Output: Stop!
        checkTrafficLight(TrafficLight.GREEN);  // Output: Go!
    }
}
```

### Using switch-case with Enum

Switch statements work very well with enums and are often preferred for cleaner code:

```java
public enum Season {
    SPRING, SUMMER, AUTUMN, WINTER
}

public class WeatherApp {
    public static void describeWeather(Season season) {
        switch (season) {
            case SPRING:
                System.out.println("Flowers are blooming!");
                break;
            case SUMMER:
                System.out.println("It's hot and sunny!");
                break;
            case AUTUMN:
                System.out.println("Leaves are falling!");
                break;
            case WINTER:
                System.out.println("It's cold and snowy!");
                break;
            default:
                System.out.println("Unknown season");
        }
    }
    
    public static void main(String[] args) {
        for (Season season : Season.values()) {
            describeWeather(season);
        }
    }
}
```

## Enum Inheritance and Structure

### Key Points about Enum Inheritance:

- **We cannot extend any class from enum** - Enums cannot inherit from other classes
- **Enum extends Enum class** - All enums implicitly extend the `java.lang.Enum` class
- Enums can implement interfaces
- Enums are implicitly `final`, so they cannot be subclassed

```java
// This is NOT allowed - enums cannot extend classes
// public enum Color extends SomeClass { ... } // COMPILE ERROR

// This IS allowed - enums can implement interfaces
public interface Describable {
    String getDescription();
}

public enum Color implements Describable {
    RED("Passionate color"),
    GREEN("Nature's color"),
    BLUE("Sky color");
    
    private String description;
    
    Color(String description) {
        this.description = description;
    }
    
    @Override
    public String getDescription() {
        return description;
    }
}
```

## Enum Constructors and Methods

### Enum can have methods and constructors

**Constructors are usually private** since we are creating objects within the enum class itself.

```java
public enum Planet {
    MERCURY(3.303e+23, 2.4397e6),
    VENUS(4.869e+24, 6.0518e6),
    EARTH(5.976e+24, 6.37814e6),
    MARS(6.421e+23, 3.3972e6);
    
    private final double mass;   // in kilograms
    private final double radius; // in meters
    
    // Constructor is private by default
    Planet(double mass, double radius) {
        this.mass = mass;
        this.radius = radius;
    }
    
    // Custom methods
    public double getMass() {
        return mass;
    }
    
    public double getRadius() {
        return radius;
    }
    
    // Calculate surface gravity
    public double surfaceGravity() {
        final double G = 6.67300E-11;
        return G * mass / (radius * radius);
    }
    
    public double surfaceWeight(double otherMass) {
        return otherMass * surfaceGravity();
    }
}

public class PlanetExample {
    public static void main(String[] args) {
        double earthWeight = 70; // kg
        double mass = earthWeight / Planet.EARTH.surfaceGravity();
        
        for (Planet p : Planet.values()) {
            System.out.printf("Your weight on %s is %f kg%n",
                            p, p.surfaceWeight(mass));
        }
    }
}
```

### Advanced Enum Example with Methods

```java
public enum Operation {
    PLUS("+") {
        public double apply(double x, double y) { return x + y; }
    },
    MINUS("-") {
        public double apply(double x, double y) { return x - y; }
    },
    TIMES("*") {
        public double apply(double x, double y) { return x * y; }
    },
    DIVIDE("/") {
        public double apply(double x, double y) { return x / y; }
    };
    
    private final String symbol;
    
    Operation(String symbol) {
        this.symbol = symbol;
    }
    
    public abstract double apply(double x, double y);
    
    @Override
    public String toString() {
        return symbol;
    }
    
    public static void main(String[] args) {
        double x = 10.0;
        double y = 5.0;
        
        for (Operation op : Operation.values()) {
            System.out.printf("%.1f %s %.1f = %.1f%n",
                            x, op, y, op.apply(x, y));
        }
    }
}
```

## Summary

- **Enum** represents named constants in a type-safe way
- Use `ordinal()` to get the index position of enum constants
- Use `values()` to get all enum values as an array
- Enhanced for loops work perfectly with enums
- Both if-else and switch-case statements can be used with enums
- Enums cannot extend classes but can implement interfaces
- Enums automatically extend the `java.lang.Enum` class
- Enum constructors are private since objects are created within the enum itself
- Enums can have custom methods and fields for complex behavior
