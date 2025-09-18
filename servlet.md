---
layout: default
title: Core Java Mastery
permalink: /servlet/
---
# Java Servlets Tutorial

## Overview

Servlets in Java provide a powerful way to create dynamic web applications. This tutorial covers the fundamentals of servlet development, configuration, and deployment.

## What are Servlets?

In a typical web application architecture:
- **Client** sends a static request to the **Server**
- **Server** returns an existing page for static requests
- For **dynamic content**, the server needs to create pages on-demand

### Web Container (Servlet Container)

When dynamic content is required, requests go to a **helper application** called a **Web Container** (e.g., Apache Tomcat). The web container contains **Servlets** that:
- Take incoming requests
- Process them
- Convert them to HTML pages

### Example Flow with Tomcat
1. Request comes to Tomcat for `abc.html`
2. If `abc.html` doesn't exist, Tomcat searches for the corresponding servlet
3. Servlet mapping is defined in the **Deployment Descriptor** (`web.xml`)

## Deployment Descriptor (web.xml)

The `web.xml` file contains two important tags:

### 1. Servlet Tag
```xml
<servlet>
    <servlet-name>MyServlet</servlet-name>
    <servlet-class>com.example.MyServletClass</servlet-class>
</servlet>
```

### 2. Servlet Mapping Tag
```xml
<servlet-mapping>
    <servlet-name>MyServlet</servlet-name>
    <url-pattern>/myservlet</url-pattern>
</servlet-mapping>
```

## Flow Chart

### Static Request Flow
```
Client → Server → Existing HTML Page → Client
```

### Dynamic Request Flow
```
Client → Server → Web Container → Servlet → Generated HTML → Client
```

## Annotation-Based Configuration

You can avoid XML configuration by using annotations:
```java
@WebServlet("/myservlet")
public class MyServlet extends HttpServlet {
    // servlet code
}
```

## Creating a Servlet

To create a servlet, extend the `HttpServlet` class:

```java
public class MyServlet extends HttpServlet {
    public void service(HttpServletRequest request, HttpServletResponse response) 
            throws ServletException, IOException {
        // servlet logic here
    }
}
```

## Prerequisites for Servlet Development

### Setting up Tomcat in IntelliJ IDEA

1. Create a new project
2. Understand the project structure:
   - `WEB-INF/` directory
   - `web.xml` file
   - `classes/` directory for compiled servlets

## Basic Examples

### 1. Returning a Static File
Simple example of serving static content through a servlet.

### 2. Dynamic Servlet for Adding Two Numbers

```java
@WebServlet("/add")
public class AddServlet extends HttpServlet {
    public void service(HttpServletRequest req, HttpServletResponse res) 
            throws ServletException, IOException {
        
        int num1 = Integer.parseInt(req.getParameter("num1"));
        int num2 = Integer.parseInt(req.getParameter("num2"));
        int result = num1 + num2;
        
        res.setContentType("text/html");
        PrintWriter out = res.getWriter();
        out.println("<h1>Result: " + result + "</h1>");
    }
}
```

**Note**: `getParameter()` always returns a `String`, so type conversion is needed.

## HTTP Methods: doGet() vs doPost()

Instead of using the generic `service()` method, you can specify HTTP methods:

```java
public void doGet(HttpServletRequest req, HttpServletResponse res) {
    // Handle GET requests
}

public void doPost(HttpServletRequest req, HttpServletResponse res) {
    // Handle POST requests
}
```

The `service()` method runs behind the scenes and redirects to the appropriate method based on the HTTP request type.

## Key Objects

- **HttpServletRequest**: Contains all data the servlet needs from the client
- **HttpServletResponse**: Contains all data the client needs from the servlet
- These objects are created and managed by Tomcat

## Servlet Communication

### Calling One Servlet from Another

There are two main approaches:

#### 1. RequestDispatcher (Forward)
```java
// In Servlet1
RequestDispatcher rd = request.getRequestDispatcher("/servlet2");
rd.forward(req, res);
```
- Same request and response objects are shared
- Client doesn't know the request was forwarded
- URL doesn't change in browser

#### 2. Redirect
```java
// In Servlet1
response.sendRedirect("servlet2");
```
- Client makes a new request to servlet2
- URL changes in browser
- New request and response objects are created

## Data Sharing Between Servlets

### 1. Session Management
```java
// In Servlet1 - Setting data
HttpSession session = req.getSession();
session.setAttribute("number", 213);

// In Servlet2 - Getting data
HttpSession session = req.getSession();
int number = (Integer) session.getAttribute("number");

// Remove session data
session.removeAttribute("number");
```

### 2. Cookies
```java
// In Servlet1 - Setting cookie
Cookie c = new Cookie("key", "value");
res.addCookie(c);

// In Servlet2 - Reading cookie
Cookie[] cookies = req.getCookies();
for (Cookie cookie : cookies) {
    if ("key".equals(cookie.getName())) {
        String value = cookie.getValue();
        break;
    }
}
```

### 3. Request Attributes (with RequestDispatcher)
```java
// In Servlet1
req.setAttribute("number", 12);
RequestDispatcher rd = req.getRequestDispatcher("/servlet2");
rd.forward(req, res);

// In Servlet2
int number = (Integer) req.getAttribute("number");
```

### 4. URL Rewriting (with Redirect)
```java
response.sendRedirect("servlet2?key=" + value);
```

## ServletContext vs ServletConfig

### ServletContext
Shared among all servlets in the application:

```xml
<!-- In web.xml -->
<context-param>
    <param-name>dbUrl</param-name>
    <param-value>jdbc:mysql://localhost:3306/mydb</param-value>
</context-param>
```

```java
// In servlet
ServletContext ctx = getServletContext();
String dbUrl = ctx.getInitParameter("dbUrl");
```

### ServletConfig
Specific to individual servlets:

```xml
<!-- In web.xml -->
<servlet>
    <servlet-name>MyServlet</servlet-name>
    <servlet-class>com.example.MyServlet</servlet-class>
    <init-param>
        <param-name>maxConnections</param-name>
        <param-value>100</param-value>
    </init-param>
</servlet>
```

```java
// In servlet
ServletConfig config = getServletConfig();
String maxConn = config.getInitParameter("maxConnections");
```

## Servlet Annotations

Modern servlet development uses annotations instead of XML configuration:

### Basic Servlet Annotation
```java
@WebServlet("/myservlet")
public class MyServlet extends HttpServlet {
    // servlet implementation
}
```

### Advanced Servlet Annotation
```java
@WebServlet(
    name = "MyServlet",
    urlPatterns = {"/myservlet", "/servlet"},
    initParams = {
        @WebInitParam(name = "param1", value = "value1"),
        @WebInitParam(name = "param2", value = "value2")
    }
)
public class MyServlet extends HttpServlet {
    // servlet implementation
}
```

### Related Annotations
- `@WebFilter` - For servlet filters
- `@WebListener` - For event listeners
- `@WebInitParam` - For initialization parameters
- `@MultipartConfig` - For file upload handling

## Best Practices

1. Always handle exceptions properly
2. Use appropriate HTTP methods (GET for retrieval, POST for data submission)
3. Validate user input
4. Use session management judiciously
5. Consider using annotations for cleaner code
6. Follow MVC pattern for larger applications

## Conclusion

Servlets provide a robust foundation for Java web applications. Understanding the request-response cycle, servlet lifecycle, and data sharing mechanisms is crucial for building effective web applications.
