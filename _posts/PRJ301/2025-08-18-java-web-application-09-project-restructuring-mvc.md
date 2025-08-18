---
title: Epoch 9 – Project Restructuring to MVC
description: >-
  Create a clean MVC folder/package structure. Move all DB/business logic out of JSP pages and into DAO + Model classes.
author: [shandy]
date: 2025-08-18
categories: [(Java) Web Application, (PBL) Shopping Web]
tags: [(Java - PBL) Shopping Web]
sort_index: 108
# pin: true
# media_subpath: '/posts/01'
---

## 1. Architechture Project

```
/src/
  ├── Controllers
  │     ├── LoginServlet.java
  │     ├── HomeServlet.java
  │     ├── LogoutServlet.java
  ├── Models
  │     ├── User.java
  ├── DALs
  │     ├── UserDAO.java
  ├── Utils
  │     ├── DBContext.java
/webapp/
  ├── WEB-INF/
  │     ├── web.xml
  ├── views/
  │     ├── pages
  │     │     ├── login.jsp
  │     │     ├── login-content.jsp
  │     │     ├── home.jsp
  │     │     ├── home-content.jsp
  │     ├── layouts
  │     │     ├── layout.jsp
```

## 2. Why restructure from Model 1?

### Model 1 issues:

- Business logic is mixed inside JSP pages (login.jsp has username/password checking).
- Hard to maintain: changing the UI can break server logic.
- Poor reusability: you can’t reuse the login logic elsewhere without copying code.

### Model 2 (MVC) benefits:

- Controllers (Servlets) handle request/response flow.
- Models store business data and perform DB operations.
- Views (JSPs) only display data — no database code.

## 3. Migration Plan

- Create User model class to hold username/password.
- Create UserDAO to handle database logic (later DBContext can manage connection).
- Create LoginServlet and LogoutServlet for request handling.
- Modify JSP views (login.jsp, home.jsp) to receive data from Servlets, not to process it.

## 4. Refactoring MVC

### Models

**(A) User.java (Models)**

```java
package Models;

public class User {
    private String username;
    private String password;

    public User() {}
    public User(String username, String password) {
        this.username = username;
        this.password = password;
    }

    public String getUsername() { 
        return username; 
    }

    public void setUsername(String username) { 
        this.username = username; 
    }

    public String getPassword() { 
        return password; 
    }

    public void setPassword(String password) { 
        this.password = password; 
    }
}
```

**(B) UserDAO.java (DALs - Data Access Layer)**

```java
package DALs;

import Models.User;

public class UserDAO {
    public boolean checkLogin(String username, String password) {
        // ...
    }
}
```

### Controllers

**(C) Create LoginServlet.java (Controllers)**

- Create new package such as "Controllers"
- Create a new Servlet such as "LoginServlet" in Controller packages:

![](/assets/img/2025-08-15-13-18-08.png)

- That is a notice, we:
  - check "Add information ... (web.xml)"
  - update "URL Pattern(s)": That is maping url went connect server.

![](/assets/img/2025-08-15-13-24-00.png)

```java
package Controllers;

import DALs.UserDAO;
import java.io.IOException;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.Cookie;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

public class LoginServlet extends HttpServlet {
   
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {
        request.setAttribute("contentPage", "login_form_content.jsp");
        request.getRequestDispatcher("/views/pages/login.jsp").forward(request,response);
    } 

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {
        String username = request.getParameter("username");
        String password = request.getParameter("password");

        UserDAO users = new UserDAO();
        String uName = users.CheckUser(username, password);
        if (uName != null) {
            Cookie cookie1 = new Cookie("username", uName);
            cookie1.setMaxAge(60 * 60); // 1 hour
            response.addCookie(cookie1);

            response.sendRedirect(request.getContextPath() + "/home");
        } else {
            request.setAttribute("loginError", "Username or password are failed!");
        }
    }
}
```

**(D) LogoutServlet.java (Controllers)**

- URL: "/logout"

```
package Controllers;

import java.io.IOException;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.Cookie;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

public class LogoutServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {
        Cookie[] cookies = request.getCookies();
        if (cookies != null) {
            for (Cookie c : cookies) {
                if ("username".equals(c.getName()) || "id".equals(c.getName()) ||"gender".equals(c.getName())) {
                    c.setMaxAge(0);
                    response.addCookie(c);
                }
            }
        }
        
        response.sendRedirect(request.getContextPath() + "/login");
    }
}
```


**(E) HomeServlet.java (Controllers)**

```java
package Controllers;

import java.io.IOException;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.Cookie;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

public class HomeServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {
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
            response.sendRedirect(request.getContextPath() + "/login");
            return;
        }

        request.setAttribute("contentPage", "home_content.jsp");
        request.setAttribute("username", username);
        request.getRequestDispatcher("/views/pages/home.jsp").forward(request,response);
    } 
}
```

### Views

**(F) login.jsp (Views)**

```jsp
<jsp:include page="../layouts/layout.jsp" >
    <jsp:param name="pageTitle" value="Login - JSP Shop" />
</jsp:include>
```

**(G) login_form_content.jsp**

```jsp
<h2 class="mb-4">Login Form</h2>
<form action="${pageContext.request.contextPath}/login" method="post" class="col-md-4">
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

**(H) home.jsp (Views)**

```jsp
<jsp:include page="../layouts/layout.jsp" >
    <jsp:param name="pageTitle" value="Home - JSP Shop" />
</jsp:include>
```

**(I) home_content.jsp**

```jsp
<h2>Welcome, ${username} (Cookie Based)</h2>

<form action="${pageContext.request.contextPath}/logout" method="post">
    <button type="submit" class="btn btn-outline-primary">Logout</button>
</form>
```

### web.xml Changes

```xml
    <?xml version="1.0" encoding="UTF-8"?>
<web-app version="3.1" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd">
    
    <servlet>
        <servlet-name>LoginServlet</servlet-name>
        <servlet-class>Controllers.LoginServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>LoginServlet</servlet-name>
        <url-pattern>/login</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>Logout</servlet-name>
        <servlet-class>Controllers.LogoutServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>Logout</servlet-name>
        <url-pattern>/logout</url-pattern>
    </servlet-mapping>

    <servlet>
        <servlet-name>HomeServlet</servlet-name>
        <servlet-class>Controllers.HomeServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>HomeServlet</servlet-name>
        <url-pattern>/home</url-pattern>
    </servlet-mapping>
    
    <welcome-file-list>
        <welcome-file>
            login
        </welcome-file>
    </welcome-file-list>
    <servlet-mapping>
        <servlet-name>default</servlet-name>
        <url-pattern>/assets/*</url-pattern>
    </servlet-mapping>
    <session-config>
        <session-timeout>
            30
        </session-timeout>
    </session-config>
</web-app>
```