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

## Architechture for project:

```
src/webapp/views/
├── layout.jsp
├── login_cookie.jsp
├── home_cookie.jsp
├── logout_cookie.jsp
|
├── login_session.jsp
├── home_session.jsp
├── logout_session.jsp
```

- Update webcome file in web.xml:

```xml
<welcome-file-list>
    <welcome-file>views/login.jsp</welcome-file>
</welcome-file-list>
```

- Create **login.jsp**:

```jsp
<jsp:include page="layout.jsp">
    <jsp:param name="pageTitle" value="Login" />
    <jsp:param name="bodyPage" value="login_form_content.jsp" />
</jsp:include>
```

- Create **login_form_content.jsp**

```jsp
<%@ page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
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
```

- Create **home.jsp**:

```jsp
<jsp:include page="layout.jsp">
    <jsp:param name="pageTitle" value="Login" />
    <jsp:param name="bodyPage" value="home_body.jsp" />
</jsp:include>
```

> **home_body** replaces: home_cookie_body or home_session_body

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

### 1.3 Login/logout by Cookie-based

- **views/login_cookie.jsp**

```jsp
<%@ page contentType="text/html;charset=UTF-8" %>
<%@ page import="jakarta.servlet.http.*" %>

<jsp:include page="login.jsp" />

<%
    if ("POST".equalsIgnoreCase(request.getMethod())) {
        String username = request.getParameter("username");
        String password = request.getParameter("password");

        if ("admin".equals(username) && "123".equals(password)) {
            Cookie userCookie = new Cookie("username", username);
            userCookie.setMaxAge(60*60); // 1 giờ
            response.addCookie(userCookie);
            response.sendRedirect("home.jsp");
            return;
        } else {
            out.println("<p style='color:red;'>Invalid credentials!</p>");
        }
    }
%>
```

- **views/home_cookie_body.jsp**

```jsp
<%@ page contentType="text/html;charset=UTF-8" %>
<%@ page import="jakarta.servlet.http.*" %>

<%
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

    if (username == null) {
        response.sendRedirect("login_cookie.jsp");
        return;
    }
%>

<h2>Welcome, <%= username %> (Cookie Based)</h2>
<a href="logout_cookie.jsp">Logout</a>
```

- **views/logout_cookie.jsp**

```jsp
<%@ page contentType="text/html;charset=UTF-8" %>
<%@ page import="jakarta.servlet.http.*" %>

<%
    Cookie userCookie = new Cookie("username", "");
    userCookie.setMaxAge(0); // Xóa cookie
    response.addCookie(userCookie);
    response.sendRedirect("login_cookie.jsp");
%>
```

## Option 2. Session-based Login/Logout Approach

### 1.1 Theory - Session-based Authentication

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

### 1.2 Commonly Used Cookie Methods in JSP

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

### 1.3 Login/logout by Cookie-based

- **views/login_session.jsp**

```jsp
<%@ page contentType="text/html;charset=UTF-8" %>
<%@ page import="jakarta.servlet.http.*" %>

<jsp:include page="login.jsp" />

<%
    if ("POST".equalsIgnoreCase(request.getMethod())) {
        String username = request.getParameter("username");
        String password = request.getParameter("password");

        if ("admin".equals(username) && "123".equals(password)) {
            HttpSession session = request.getSession();
            session.setAttribute("username", username);
            response.sendRedirect("home.jsp");
            return;
        } else {
            out.println("<p style='color:red;'>Invalid credentials!</p>");
        }
    }
%>
```

- **views/home_session_body.jsp**

```jsp
<%@ page contentType="text/html;charset=UTF-8" %>
<%@ page import="jakarta.servlet.http.*" %>

<%
    HttpSession session = request.getSession(false);
    String username = (session != null) ? (String) session.getAttribute("username") : null;

    if (username == null) {
        response.sendRedirect("login_session.jsp");
        return;
    }
%>

<h2>Welcome, <%= username %> (Session Based)</h2>
<a href="logout_session.jsp">Logout</a>
```

- **views/logout_cookie.jsp**

```jsp
<%@ page contentType="text/html;charset=UTF-8" %>
<%@ page import="jakarta.servlet.http.*" %>

<%
    HttpSession session = request.getSession(false);
    if (session != null) {
        session.invalidate();
    }
    response.sendRedirect("login_session.jsp");
%>
```