---
layout: default
title:  Java Sever Pages
permalink: /jsp/
---
# Complete JSP Tutorial

## Table of Contents
1. [What is JSP?](#what-is-jsp)
2. [Why do we need JSP?](#why-do-we-need-jsp)
3. [Relationship between Servlets and JSP](#relationship-between-servlets-and-jsp)
4. [JSP to Servlet Conversion](#jsp-to-servlet-conversion)
5. [JSP Syntax Elements](#jsp-syntax-elements)
6. [JSP Example](#jsp-example)
7. [JSP Directives](#jsp-directives)
8. [JSP Implicit Objects](#jsp-implicit-objects)
9. [Exception Handling in JSP](#exception-handling-in-jsp)
10. [Database Connection using JDBC](#database-connection-using-jdbc)
11. [MVC Architecture with JSP](#mvc-architecture-with-jsp)

---

## What is JSP?

**JSP (JavaServer Pages)** is a server-side technology that allows developers to create dynamic web pages by embedding Java code directly into HTML. JSP files have a `.jsp` extension and are processed by the web server to generate HTML content dynamically.

Key characteristics:
- Server-side scripting technology
- Combines HTML with Java code
- Platform independent
- Part of Java EE specification

---

## Why do we need JSP?

### Problems with Servlets for UI Generation

When using servlets to generate HTML responses, we had to write:

```java
PrintWriter out = response.getWriter();
out.println("<html>");
out.println("<head><title>Welcome</title></head>");
out.println("<body>");
out.println("<h1>Hello User!</h1>");
out.println("</body>");
out.println("</html>");
```

**Issues with this approach:**
- Writing HTML in Java is cumbersome
- Difficult to maintain and design
- Mixing presentation logic with business logic
- Web designers can't work directly with the code

### JSP Solutions:
- Write Java code inside HTML (reverse approach)
- Simplified object creation and management
- Better separation of concerns
- Designer-friendly syntax

---

## Relationship between Servlets and JSP

### Why do we still need Servlets?

Even though JSP seems easier, we still need servlets because:

1. **JSP cannot run directly** - Tomcat is a servlet container that only understands servlets
2. **JSP is converted to servlet** behind the scenes
3. **Servlets provide the underlying infrastructure** for JSP execution

### JSP to Servlet Conversion Process

```
JSP File → Translation Phase → Servlet Java File → Compilation → Servlet Class → Execution
```

**How the conversion works:**
- JSP filename becomes the servlet class name
- Automatically extends `HttpServlet`
- Auto-generates `service()` method
- HTML content becomes `out.println()` statements
- Java code between delimiters goes into the `service()` method

---

## JSP to Servlet Conversion

### Example: JSP to Servlet Conversion

**Original JSP (welcome.jsp):**
```jsp
<%@ page language="java" contentType="text/html" %>
<html>
<head>
    <title>Welcome Page</title>
</head>
<body>
    <h1>Welcome!</h1>
    <%
        String name = request.getParameter("name");
        int count = 5;
    %>
    <p>Hello <%= name %>, you have <%= count %> messages.</p>
    <%!
        private int visitorCount = 0;
        public String getWelcomeMessage() {
            return "Welcome to our site!";
        }
    %>
</body>
</html>
```

**Generated Servlet (welcome_jsp.java):**
```java
public class welcome_jsp extends HttpServlet {
    // Declaration tag content becomes instance variables/methods
    private int visitorCount = 0;
    public String getWelcomeMessage() {
        return "Welcome to our site!";
    }
    
    public void service(HttpServletRequest request, HttpServletResponse response) {
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        
        // HTML content becomes println statements
        out.println("<html>");
        out.println("<head>");
        out.println("<title>Welcome Page</title>");
        out.println("</head>");
        out.println("<body>");
        out.println("<h1>Welcome!</h1>");
        
        // Scriptlet content goes into service method
        String name = request.getParameter("name");
        int count = 5;
        
        out.println("<p>Hello " + name + ", you have " + count + " messages.</p>");
        out.println("</body>");
        out.println("</html>");
    }
}
```

---

## JSP Syntax Elements

### 1. Scriptlet Tag `<% %>`
Contains Java code that gets executed

```jsp
<%
    int i = Integer.parseInt(request.getParameter("num1"));
    int j = Integer.parseInt(request.getParameter("num2"));
    int k = i + j;
%>
```

### 2. Declaration Tag `<%! %>`
Declares instance variables and methods

```jsp
<%!
    private int counter = 0;
    public void incrementCounter() {
        counter++;
    }
%>
```

### 3. Expression Tag `<%= %>`
Outputs values directly to the HTML

```jsp
<p>Result: <%= k %></p>
<p>Current time: <%= new Date() %></p>
```

### 4. Directive Tag `<%@ %>`
Provides page-level instructions

```jsp
<%@ page import="java.util.*" %>
<%@ page language="java" contentType="text/html" %>
```

---

## JSP Example

**add.jsp - Simple Calculator:**
```jsp
<%@ page language="java" contentType="text/html" %>
<html>
<head>
    <title>Addition Result</title>
</head>
<body bgcolor="cyan">
    <h2>Calculator Result</h2>
    <%
        int i = Integer.parseInt(request.getParameter("num1"));
        int j = Integer.parseInt(request.getParameter("num2"));
        int k = i + j;
        out.println("Result: " + k);
    %>
    
    <p>Sum of <%= i %> and <%= j %> is: <%= k %></p>
</body>
</html>
```

**HTML Form to call JSP:**
```html
<form action="add.jsp" method="get">
    Number 1: <input type="text" name="num1"><br>
    Number 2: <input type="text" name="num2"><br>
    <input type="submit" value="Add">
</form>
```

---

## JSP Directives

### 1. Page Directive `<%@ page %>`
Defines page-level attributes:

```jsp
<%@ page 
    language="java" 
    extends="com.example.CustomServlet"
    import="java.util.*, java.sql.*"
    contentType="text/html; charset=UTF-8"
    session="true"
    autoFlush="true"
    errorPage="error.jsp"
    isErrorPage="false"
    info="This is a sample page"
    isELIgnored="false"
    isThreadSafe="true"
%>
```

### 2. Include Directive `<%@ include %>`
Includes content from other files at translation time:

```jsp
<%@ include file="header.jsp" %>
<%@ include file="navigation.jsp" %>
```

### 3. Taglib Directive `<%@ taglib %>`
Declares custom tag libraries:

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ taglib uri="/WEB-INF/custom.tld" prefix="custom" %>
```

---

## JSP Implicit Objects

JSP provides built-in objects without explicit creation:

### 1. `request` - HttpServletRequest
```jsp
<%= request.getParameter("username") %>
<%= request.getRemoteAddr() %>
```

### 2. `response` - HttpServletResponse
```jsp
<%
    response.setContentType("text/html");
    response.addCookie(new Cookie("user", "john"));
%>
```

### 3. `out` - PrintWriter
```jsp
<%
    out.println("Hello World");
    out.flush();
%>
```

### 4. `session` - HttpSession
```jsp
<%
    session.setAttribute("user", "John Doe");
    String user = (String) session.getAttribute("user");
%>
```

### 5. `pageContext` - PageContext
Sets attributes accessible within the same page:
```jsp
<%
    pageContext.setAttribute("message", "Hello", PageContext.PAGE_SCOPE);
%>
```

### 6. `servletContext` - ServletContext
Application-wide context:
```jsp
<%
    servletContext.setAttribute("appName", "MyApp");
%>
```

### 7. `servletConfig` - ServletConfig
Servlet configuration information:
```jsp
<%= servletConfig.getServletName() %>
```

---

## Exception Handling in JSP

### Basic Exception Handling

**Problem:**
```jsp
<%
    int k = 9/0;  // This will cause ArithmeticException
%>
```

### Solution 1: Try-Catch (Not Recommended for Web)
```jsp
<%
    try {
        int k = 9/0;
        out.println("Result: " + k);
    } catch (Exception e) {
        out.println("Error: " + e.getMessage());
    }
%>
```

### Solution 2: Error Pages (Recommended)

**main.jsp:**
```jsp
<%@ page errorPage="error.jsp" %>
<html>
<body>
    <%
        int k = 9/0;  // Exception will be handled by error.jsp
    %>
</body>
</html>
```

**error.jsp:**
```jsp
<%@ page isErrorPage="true" %>
<html>
<body>
    <h2>An error occurred!</h2>
    <p>Error: <%= exception.getMessage() %></p>
    <p>Exception Type: <%= exception.getClass().getName() %></p>
</body>
</html>
```

---

## Database Connection using JDBC

### Complete JDBC Example in JSP

```jsp
<%@ page import="java.sql.*" %>
<html>
<head>
    <title>Database Example</title>
</head>
<body>
    <h2>User List</h2>
    <%
        String url = "jdbc:postgresql://localhost:5432/mydb";
        String username = "postgres";
        String password = "password";
        
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        
        try {
            // 1. Load the driver
            Class.forName("org.postgresql.Driver");
            
            // 2. Create connection
            conn = DriverManager.getConnection(url, username, password);
            
            // 3. Create statement
            stmt = conn.createStatement();
            
            // 4. Execute query
            String sql = "SELECT id, name, email FROM users";
            rs = stmt.executeQuery(sql);
            
            // 5. Process results
            out.println("<table border='1'>");
            out.println("<tr><th>ID</th><th>Name</th><th>Email</th></tr>");
            
            while (rs.next()) {
                int id = rs.getInt("id");
                String name = rs.getString("name");
                String email = rs.getString("email");
                
                out.println("<tr>");
                out.println("<td>" + id + "</td>");
                out.println("<td>" + name + "</td>");
                out.println("<td>" + email + "</td>");
                out.println("</tr>");
            }
            out.println("</table>");
            
        } catch (Exception e) {
            out.println("Error: " + e.getMessage());
        } finally {
            // 6. Close connections
            if (rs != null) rs.close();
            if (stmt != null) stmt.close();
            if (conn != null) conn.close();
        }
    %>
</body>
</html>
```

---

## MVC Architecture with JSP

### MVC Pattern Overview
```
Client → Controller (Servlet) → Model (POJO) → View (JSP)
```

**Why MVC?**
- **Separation of concerns:** Business logic separate from presentation
- **Don't use JSP for business logic** - use only for view
- **JSP creates dynamic content** better than static HTML

### MVC Components:

#### 1. Controller (Servlet)
```java
@WebServlet("/user")
public class UserController extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) {
        // Business logic
        UserService service = new UserService();
        List<User> users = service.getAllUsers();
        
        // Set model data
        request.setAttribute("users", users);
        
        // Forward to view
        RequestDispatcher rd = request.getRequestDispatcher("userList.jsp");
        rd.forward(request, response);
    }
}
```

#### 2. Model (POJO)
```java
public class User {
    private int id;
    private String name;
    private String email;
    
    // Constructors, getters, setters
    public User(int id, String name, String email) {
        this.id = id;
        this.name = name;
        this.email = email;
    }
    
    // Getters and Setters
    public int getId() { return id; }
    public String getName() { return name; }
    public String getEmail() { return email; }
}
```

#### 3. View (JSP)
```jsp
<%@ page import="java.util.*, com.example.User" %>
<html>
<head>
    <title>User List</title>
</head>
<body>
    <h2>All Users</h2>
    <table border="1">
        <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Email</th>
        </tr>
        <%
            List<User> users = (List<User>) request.getAttribute("users");
            for (User user : users) {
        %>
        <tr>
            <td><%= user.getId() %></td>
            <td><%= user.getName() %></td>
            <td><%= user.getEmail() %></td>
        </tr>
        <%
            }
        %>
    </table>
</body>
</html>
```

### MVC Layer Architecture:
```
Controller (Servlet) → Service Layer → DAO Layer → Database
                    ↓
                View (JSP)
```

**Benefits:**
- **Controller:** Handles HTTP requests, coordinates between layers
- **Service:** Contains business logic
- **DAO:** Data Access Object for database operations
- **View:** JSP for presentation only

---

## Best Practices

1. **Keep JSP simple** - Use only for presentation
2. **Use MVC pattern** - Separate concerns properly
3. **Handle exceptions gracefully** - Use error pages
4. **Close database connections** - Always use try-finally
5. **Minimize scriptlets** - Use JSTL and EL when possible
6. **Use implicit objects wisely** - Understand their scope
7. **Configure error pages** - Handle exceptions at application level

---

## Summary

JSP simplifies web development by allowing Java code within HTML, but it's important to:
- Understand that JSP converts to servlets
- Use JSP primarily for view layer in MVC
- Handle exceptions properly
- Follow best practices for maintainable code
- Leverage implicit objects effectively

JSP bridges the gap between static HTML and dynamic server-side programming, making it easier to create interactive web applications while maintaining the power of Java on the server side.
