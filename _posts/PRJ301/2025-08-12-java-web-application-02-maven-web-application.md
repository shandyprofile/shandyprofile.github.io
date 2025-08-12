---
title: Epoch 2: Maven Web Application Setup
description: >-
  Ensure students can create a Maven Web Application in NetBeans with the correct Jakarta EE configuration by first updating the NetBeans template so it matches the project’s requirements (Tomcat 10, JSP, Servlet, JSTL).
author: [shandy]
date: 2025-08-12
categories: [(Java) Web Application, (PBL) Shopping Web]
tags: [(Java - PBL) Shopping Web]
sort_index: 101
# pin: true
# media_subpath: '/posts/01'
---
## Update templates Jakarta EE:

### 1. Open NetBeans Templates Manager
- In NetBeans, go to: **Tools → Templates**
- In the Templates dialog, expand: **Project → Web -> Servlet**
- Update **javax** to **jakarta**:

```
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import java.io.IOException;
```
## Maven Web Application

### Step 1: Create a New Maven Web Application
- Open NetBeans 13.
- Go to File → New Project.
- Choose:
  - Categories: Java with Maven
  - Projects: Web Application
- Click Next.
- Fill in:
  - Project Name: JSP_Shop
  - Project Location: Choose your preferred workspace folder.
  - Group ID: com.jsp.shop
  - Artifact ID: jsp-shop
  - Version: 1.0-SNAPSHOT
- Click Next.

### Step 2: Configure Server & Java Version
- In Server, select Apache Tomcat 10.0.23.
- In Java EE Version, choose Jakarta EE 9 Web (Tomcat 10+ uses Jakarta namespaces).
- Make sure Java Platform is JDK 1.8_231.

### Step 3: Verify Folder Structure
Your Maven project will have:

```
jsp-shop/
 ├─ src/main/java/      (Java source files)
 ├─ src/main/resources/ (Config files)
 ├─ src/main/webapp/    (JSP, HTML, CSS, JS)
 │    ├─ META-INF/
 │    ├─ WEB-INF/
 │    │    └─ web.xml
 └─ pom.xml             (Maven configuration)
```

### Step 4: Edit pom.xml
Add dependencies for JSP and Servlet:

```xml
<dependencies>
    <!-- Servlet API -->
    <dependency>
        <groupId>jakarta.servlet</groupId>
        <artifactId>jakarta.servlet-api</artifactId>
        <version>5.0.0</version>
        <scope>provided</scope>
    </dependency>

    <!-- JSP API -->
    <dependency>
        <groupId>jakarta.servlet.jsp</groupId>
        <artifactId>jakarta.servlet.jsp-api</artifactId>
        <version>2.0.0</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```
> **Note:** Scope provided means the server (Tomcat) will provide these libraries at runtime.

### Step 5: Create Your First JSP File
- Right-click src/main/webapp → New → JSP File.
- Name it index.jsp.
- Add simple HTML:

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Welcome to JSP Shop</title>
</head>
<body>
    <h1>Hello, JSP World!</h1>
</body>
</html>
```
### Step 6: Configure web.xml (Optional)
If you want to set a default welcome page:

```xml
<welcome-file-list>
    <welcome-file>index.jsp</welcome-file>
</welcome-file-list>
```
Step 7: Build & Run
- Right-click project → Clean and Build.
- Right-click project → Run.

The browser should open:

```bash
http://localhost:8080/jsp-shop
```

> You’ll see Hello, JSP World!