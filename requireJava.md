# Getting Started with Java Development

To begin developing Java applications, you need two essential components:

## 1. IDE/Editor
**IDE (Integrated Development Environment)** provides comprehensive tools for software development including the ability to:
- Write and edit code
- Compile programs
- Debug applications
- Run and test code

**Popular Java IDEs and Editors:**
- **IntelliJ IDEA** - Professional Java IDE with advanced features
- **Visual Studio Code** - Lightweight editor with Java extensions
- **Atom** - Hackable text editor (discontinued but still usable)
- **Eclipse** - Free, open-source IDE
- **NetBeans** - Official Oracle IDE for Java

## 2. Compiler - JDK (Java Development Kit)
The **JDK (Java Development Kit)** is essential for compiling Java code. It includes:
- Java compiler (javac)
- Java Runtime Environment (JRE)
- Development tools and libraries
- Documentation and examples

## Installation Verification

After installing the JDK, verify your installation:

### Check Java Version
Open Command Prompt (Windows) or Terminal (Mac/Linux) and run:
```bash
java --version
```

### Setting Up Path Variables (Windows)

If the command doesn't work, you need to add Java to your system PATH:
#### 1. Open System Properties → Advanced → Environment Variables
#### 2. Add to System Variables or User Variables:
      Variable Name: JAVA_HOME
      Variable Value: Path to your JDK installation (e.g., C:\Program Files\Java\jdk-17)
#### 3. Edit PATH variable and add: %JAVA_HOME%\bin
#### 4. Restart Command Prompt and test with java --version

###Typical JDK Installation Paths:
#### * Windows: C:\Program Files\Java\jdk-[version]\bin
#### * Mac: /Library/Java/JavaVirtualMachines/jdk-[version].jdk/Contents/Home/bin
#### * Linux: /usr/lib/jvm/java-[version]-openjdk/bin

