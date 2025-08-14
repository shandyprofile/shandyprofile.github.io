---
title: Epoch 5 — Bootstrap Integration
description: >-
  Set up a reusable Bootstrap layout for all JSP pages, serve static assets properly, and make home.jsp the default landing page.
author: [shandy]
date: 2025-08-13
categories: [(Java) Web Application, (PBL) Shopping Web]
tags: [(Java - PBL) Shopping Web]
sort_index: 105
# pin: true
# media_subpath: '/posts/01'
---

## Architechture for project:

```
/src/main/webapp
├── /views
│      ├──  layouts
│           └── layout.jsp
├──    ├──  pages
│           ├── home.jsp
│           └── home_content.jsp
└── /assets
       ├── css/
            ├── bootstrap.min.css
            └── style.css
       ├── js/
            ├── bootstrap.bundle.min.js
            └── main.js
       └── img/
```

## 1. Download & Add Bootstrap

1. Download Bootstrap from https://getbootstrap.com.

2. Place:
```
    bootstrap.min.css → ./assets/css/
    bootstrap.bundle.min.js → ./assets/js/
```
3. Link them:

```html
    <link rel="stylesheet" href="${pageContext.request.contextPath}/assets/css/bootstrap.min.css">
    
    <script src="${pageContext.request.contextPath}/assets/js/bootstrap.bundle.min.js"></script>
```
## 2. Create views/layout.jsp
A single, reusable layout that pulls in Bootstrap and renders a body section.

**./webapp/views/layout.jsp**

```jsp
<%@ page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>${param.pageTitle}</title>
    <link href="${pageContext.request.contextPath}/assets/css/bootstrap.css" rel="stylesheet">
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
        <jsp:include page="/views/pages/${param.bodyPage}" />
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
## 3. Create a simple Home page
**home.jsp** stays minimal: it only passes title and body into **layout.jsp**.
The actual content lives in partials **homeBody.jsp**.

- **/views/home.jsp**

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<jsp:include page="/views/layout/layout.jsp" >
    <jsp:param name="pageTitle" value="Home - JSP Shop" />
    <jsp:param name="bodyPage" value="home_content.jsp" />
</jsp:include>
```

- **./views/home_content.jsp**

```jsp
<%@ page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<div class="row justify-content-center">
  <div class="col-lg-8">
    <div class="card shadow-sm">
      <div class="card-body">
        <h1 class="h3 mb-3">Welcome to JSP Shop</h1>
        <p class="mb-0">
          This is a simple Home page.
          We’re using a shared 
          <code>
            views/layout.jsp
          </code> 
          for consistent UI with Bootstrap.
        </p>
      </div>
    </div>
  </div>
</div>
```

## 4. Publish (serve) assets correctly
Place your files under ./webapp/assets/.... They will be publicly available at:

```
http://localhost:8080/<context>/assets/css/style.css
http://localhost:8080/<context>/assets/js/main.js
http://localhost:8080/<context>/assets/images/logo.png
```

> **Examples:**

- style.css example

```css
body { min-height: 100vh; }
.navbar-brand { font-weight: 700; letter-spacing: .3px; }
```

- main.js example:

```javascript
console.log("Assets loaded (Model 1 + Bootstrap).");
```

> **Tip**: Always prefix asset URLs in JSP with `${pageContext.request.contextPath}` to work under any context path.

To publish resources then web.xml:

```xml
<servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>/assets/*</url-pattern>
</servlet-mapping>
```

- <servlet-name>default</servlet-name> — Refers to the Default Servlet provided by the servlet container (Tomcat, Jetty, etc.), which is responsible for serving static resources.
- <url-pattern>/assets/*</url-pattern> — This tells the servlet container to serve all requests starting with /assets/ directly from the file system under src/main/webapp/assets.
- This avoids your MVC controllers intercepting these static files.

## 5. Configure a welcome-file (open to Home by default)

- Update web.xml:

```xml
...
<welcome-file-list>
    <welcome-file>views/home.jsp</welcome-file>
</welcome-file-list>
```

- Now opening:

```url
http://localhost:8080/<your-context>/
```

## 6. Source demo

**Source demo:** [jsp-shop-05](https://github.com/shandyprofile/java-jsp-shop-basic/tree/main/jsp-shop-05)