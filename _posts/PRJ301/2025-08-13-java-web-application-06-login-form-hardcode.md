---
title: Epoch 6 - Hardcoded Login/Logout (Session or Cookie).
description: >-
  Implement a basic login/logout feature using hardcoded credentials in JSP Model 1 architecture, with both Session and Cookie handling options.
author: [shandy]
date: 2025-08-13
categories: [(Java) Web Application, (PBL) Shopping Web]
tags: [(Java - PBL) Shopping Web]
sort_index: 106
# pin: true
# media_subpath: '/posts/01'
---

## Session-based and Cookie-based

Structure it so each file has two clearly labeled sections:
- Option 1: Cookie Approach
- Option 2: Session Approach
> with comments and explanatory differences.

## Option 1. Cookie-based Login/Logout Approach

### 1.1 Theory - Cookie-based Authentication

A Cookie-based authentication approach stores user-related information (e.g., username, token) on the client-side in a small text file called a cookie.

The browser sends cookies automatically with each request to the same domain, enabling the server to identify the user.

**Advantages**:
- Persistent login across sessions if cookie expiry is set.
- No server-side memory usage for authentication state.

**Disadvantages**:
- Less secure if sensitive data is stored directly (must encrypt or hash).
- User can delete or modify cookies.

### 1.2 Commonly Used Cookie Methods

| Method                       | Description                                                |
| ---------------------------- | ---------------------------------------------------------- |
| `new Cookie(name, value)`    | Creates a new cookie object                                |
| `cookie.setMaxAge(seconds)`  | Sets lifetime (in seconds); `0` deletes cookie immediately |
| `cookie.getName()`           | Returns cookie name                                        |
| `cookie.getValue()`          | Returns cookie value                                       |
| `response.addCookie(cookie)` | Sends cookie to client                                     |
| `request.getCookies()`       | Returns an array of cookies sent by the client             |

### 1.3 Using by Cookie-Based

- Add cookie-based:

```java
    Cookie cookie = new Cookie("username", username);
    cookie.setMaxAge(60 * 60); // 1 hour
    response.addCookie(cookie);
```

- Get cookie-based:

```java
    // Get stoge
    String username = null;
    Cookie[] cookies = request.getCookies();
    if (cookies != null) {
        for (Cookie c : cookies) {
            if ("username".equals(c.getName())) {
                username = c.getValue();
                break;
            }
        }
    }
```

- Remove cookie-based:
  
```java
    Cookie[] cookies = request.getCookies();
    if (cookies != null) {
        for (Cookie c : cookies) {
            if ("username".equals(c.getName())) {
                c.setMaxAge(0);
                response.addCookie(c);
            }
        }
    }
```

## Option 2. Session-based Login/Logout Approach

### 2.1 Theory - Session-based Authentication

Session-based authentication is a method of maintaining user login state by storing session data on the server.
When a user successfully logs in, the server creates a HttpSession object containing the user’s information (e.g., username, role).

The server assigns a unique Session ID, sent to the client (usually via a cookie named JSESSIONID).

On each request, the browser sends this Session ID, allowing the server to retrieve the user’s stored data from memory.

| | |
| ------------------- | --------------------------------------------------------------------------------------------- |
| **Advantage**       |                                                                            |
| Server-side storage | Sensitive data is stored on the server, not the client, reducing exposure.                    |
| Easy to implement   | JSP and Servlet provide built-in `HttpSession` support.                                       |
| Supports large data | Can store more data than cookies without worrying about size limits (\~4KB limit in cookies). |
| Automatic timeout   | Sessions can be set to expire after a period of inactivity.                                   |
| **Disadvantage**                  |                                                                                                |
| Server memory usage               | Each active user session consumes server memory.                                                                   |
| Not stateless                     | Requires server to maintain state, reducing scalability in distributed systems unless session replication is used. |
| Dependent on cookies (by default) | If the user disables cookies, session tracking requires URL rewriting.                                             |
| Session fixation risk             | If not handled correctly, attackers could hijack a session ID.                                                     |

### 2.2 Commonly Used Cookie Methods in JSP

| **Method**                                        | **Description**                                                              |
| ------------------------------------------------- | ---------------------------------------------------------------------------- |
| `request.getSession()`                            | Returns the current session, creating one if it does not exist.              |
| `request.getSession(false)`                       | Returns the current session **only** if it exists, otherwise returns `null`. |
| `session.setAttribute(String name, Object value)` | Stores an attribute in the session.                                          |
| `session.getAttribute(String name)`               | Retrieves an attribute from the session.                                     |
| `session.removeAttribute(String name)`            | Removes an attribute from the session.                                       |
| `session.invalidate()`                            | Invalidates the session, removing all attributes.                            |
| `session.getId()`                                 | Returns the unique session ID.                                               |
| `session.getCreationTime()`                       | Returns the time when the session was created.                               |
| `session.getLastAccessedTime()`                   | Returns the last time the client sent a request with this session.           |
| `session.setMaxInactiveInterval(int seconds)`     | Sets the maximum inactive time before the session expires.                   |

### 2.3 Using Session-based

- Add Session-based

```java
    // HttpSession session = request.getSession();
    session.setAttribute("username", username);
```

- Get Session-based

```java
    String username = null;
    // HttpSession session = request.getSession(false);
    username = (String)session.getAttribute("username");
    request.setAttribute("username", username);
```

- Remove Session-based

```java
    // HttpSession session = request.getSession();
    session.invalidate();
```

## Architechture for project:

```
/views/
    ├── login.jsp
    ├── logout.jsp
    ├── home.jsp
```

- Update webcome file in web.xml:

```xml
<welcome-file-list>
    <welcome-file>views/home.jsp</welcome-file>
</welcome-file-list>
```

- Create **login.jsp**:

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page contentType="text/html;charset=UTF-8" %>
<%@ page import="jakarta.servlet.*" %>

<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Login - JSP Shop</title>
        <link href="${pageContext.request.contextPath}/assets/css/bootstrap.min.css" rel="stylesheet">
        <link rel="stylesheet" href="${pageContext.request.contextPath}/assets/css/style.css">
    </head>
    <body>
        <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
            <div class="container">
                <a class="navbar-brand" href="${pageContext.request.contextPath}/views/home.jsp">
                    JSP Shop
                </a>
            </div>
        </nav>
         
        <main class="container py-4">
            <%
                if ("POST".equalsIgnoreCase(request.getMethod())) {
                    String username = request.getParameter("username");
                    String password = request.getParameter("password");

                    if ("admin".equals(username) && "123".equals(password)) {
                        // SET Cookie-based (1.3) or Session-based (2.3)

                        response.sendRedirect(request.getContextPath() + "/views/home.jsp");
                        return;
                    } else {
                        request.setAttribute("loginError", "Username or password are failed!");
                    }
                }
            %>
            
            <h2 class="mb-4">Login Form</h2>
                <form action="" method="post" class="col-md-4">
                    <div class="mb-3">
                        <label for="username" class="form-label">Username</label>
                        <input type="text" id="username" name="username" class="form-control" required>
                    </div>

                    <div class="mb-3">
                        <label for="password" class="form-label">Password</label>
                        <input type="password" id="password" name="password" class="form-control" required>
                    </div>

                    <button type="submit" class="btn btn-primary">Login</button>
                </form>
            </h2>
        </main>

        <footer class="bg-light border-top mt-5">
            <div class="container py-3 text-center small text-muted">
                © 2025 Hieu Nguyen - AI lecturer - FPT University Can Tho
            </div>
        </footer>
        <script src="${pageContext.request.contextPath}/assets/js/bootstrap.bundle.min.js"></script>
        <script src="${pageContext.request.contextPath}/assets/js/main.js"></script>
    </body>
</html>
```

- Update content in **home.jsp**:

```jsp
...
<main class="container py-4">
    <div class="row justify-content-center">
        <div class="col-lg-8">
            <div class="card shadow-sm">
                <div class="card-body">
                    <%
                        String username = null;
                        
                        // Get Cookie-based (1.3) or Session-based (2.3)

                        if (username == null) {
                            response.sendRedirect(request.getContextPath() + "/views/login.jsp");
                            return;
                        }

                        request.setAttribute("username", username);
                    %>
                    
                    <h1 class="h3 mb-3">
                        Welcome ${username} to JSP Shop
                        <a class="m-5" href="${pageContext.request.contextPath}/views/logout.jsp">Logout</a>
                    </h1>
                    <p class="mb-0">
                        This is a simple Home page.
                        for consistent UI with Bootstrap.
                    </p>
                </div>
            </div>
        </div>
    </div>
</main>
...
```

- Create **logout.jsp:**

```jsp
<%
    // Remove Cookie-based (1.3) or Session-based (2.3)

    response.sendRedirect(request.getContextPath() + "/views/login.jsp");
%>
```



[Source demo](https://github.com/shandyprofile/java-jsp-shop-basic/tree/main/jsp-shop-06)
