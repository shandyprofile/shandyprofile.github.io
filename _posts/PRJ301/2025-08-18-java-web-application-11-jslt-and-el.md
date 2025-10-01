---
title: Epoch 11 – JSTL and Expression Languages
description: >-
  Add and use JSTL and EL in your Maven JSP/Servlet project.
author: [shandy]
date: 2025-08-18
categories: [(Java) Web Application, (PBL) Shopping Web]
tags: [(Java - PBL) Shopping Web]
sort_index: 111
# pin: true
# media_subpath: '/posts/01'
---

## 1. Add JSTL Library to Maven

Open your pom.xml and add this dependency:

```xml
<dependency>
    <groupId>org.glassfish.web</groupId>
    <artifactId>jakarta.servlet.jsp.jstl</artifactId>
    <version>2.0.0</version>
</dependency>
```

After running **build with dependences**.

## 2. Import JSTL in Your JSP

- At the top of your JSP file (e.g., home.jsp), include JSTL tag libraries:

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt" %>
```

- c → JSTL Core (if, forEach, choose, set, remove, etc.)
- fmt → Formatting (dates, numbers, i18n)

**EL Scopes in JSTL**: Expression Language (EL) automatically handles attributes across different scopes:
- pageScope
- requestScope
- sessionScope
- applicationScope

## 3. Example: Replace Scriptlets with JSTL + EL

- home.jsp

```jsp
<%
    <h2>Welcome, ${username} (Cookie Based)</h2>
%>

```

> replace to:

```jsp
<c:if test="${not empty username}">
    <h2>Welcome, ${username} (Cookie Based)</h2>
    
    <form action="${pageContext.request.contextPath}/logout" method="post">
        <button type="submit" class="btn btn-outline-primary">Logout</button>
    </form>
</c:if>

<!-- Show message if user not logged in -->
<c:if test="${empty username}">
    <h2 class="text-danger">You are not logged in.</h2>
    <a href="${pageContext.request.contextPath}/login" class="btn btn-primary">Login</a>
</c:if>
```

- Continues create a basic product model (Models/Product.java)

```java
public class Product {
    private String name;
    private double price;
    
    public Product(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }
}
```

- Load data in Servlet (Home.Servlet):

```java
    // Fake product list
    List<Product> products = new ArrayList<>();
    products.add(new Product("Laptop", 1200));
    products.add(new Product("Phone", 800));
    products.add(new Product("Headset", 150));
    
    request.setAttribute("products", products);

    request.getRequestDispatcher("/views/home.jsp").forward(request, response);
```

- Update home.jsp

```jsp

<!-- ... -->
<hr/>
<h3>Available Products</h3>
<c:choose>
    <c:when test="${not empty requestScope.products}">
        <table class="table table-bordered">
            <thead>
                <tr>
                    <th>#</th>
                    <th>Name</th>
                    <th>Price</th>
                </tr>
            </thead>
            <tbody>
                <c:forEach var="p" items="${products}" varStatus="status">
                    <tr>
                        <td>${status.index + 1}</td>
                        <td>${p.name}</td>
                        <td><fmt:formatNumber value="${p.price}" type="currency" currencySymbol="$" /></td>
                    </tr>
                </c:forEach>
            </tbody>
        </table>
    </c:when>
    <c:otherwise>
        <p class="text-muted">No products available at the moment.</p>
    </c:otherwise>
</c:choose>

```

![](/assets/img/2025-08-19-11-31-08.png)
