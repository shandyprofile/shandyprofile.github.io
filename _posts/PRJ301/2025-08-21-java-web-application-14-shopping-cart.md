---
title: Epoch 14 – Shopping Cart in MVC
description: >-
  Implement a shopping cart feature using Servlets, Session, DAO, and JSP views. Users can add products to a cart, update quantities, and view their cart before placing an order.
author: [shandy]
date: 2025-08-21
categories: [(Java) Web Application, (PBL) Shopping Web]
tags: [(Java - PBL) Shopping Web]
sort_index: 114
# pin: true
# media_subpath: '/posts/01'
---

## 1. Architechture Projects

```
/src/
  ├── Controllers
  │     ├── CheckoutServlet.java
  │
  ├── Models
  │     ├── CartItem.java
  │     ├── Order.java
  │     ├── OrderItem.java
  │
  ├── DALs
  │     ├── ProductDAO.java
  │     ├── OrderDAO.java
  │
/webapp/
  ├── views/
  │     ├── pages/
  │     │     ├── products.jsp
  │     │     ├── cart.jsp
  │     │     ├── checkout.jsp

```
- Intercept requests before they reach a servlet or JSP.
- Modify requests/responses (e.g., authentication, logging, compression).
- Decide whether to forward, block, or modify the request.

It is useful for cross-cutting concerns (authentication, logging, encoding, etc.) that should not be mixed with business logic.

## Update template filter by Jakarta

- In Narbar (Menu bar) => Tools => Templates
    ![](/assets/img/2025-08-21-12-12-13.png)

- Web/Filter => Open in Editor
    ![](/assets/img/2025-08-21-12-13-23.png)

- **Replace "javax" to "jakarta"**

## 3. Create filters

- Step 1: Create package "Filters"
- Step 2: Right mouse in Filters packages => New/Filter
  - Fill Class Name (AuthFilter) and Package (Filters)

![](/assets/img/2025-08-21-12-06-31.png)

- Step 3: Check add ... web.xml.
![](/assets/img/2025-08-21-12-08-06.png)

- Update Filter/AuthFilter.java

```java
public class AuthFilter implements Filter {

    @Override
    public void doFilter(ServletRequest request, ServletResponse response,
                         FilterChain chain)
	throws IOException, ServletException {

	HttpServletRequest req = (HttpServletRequest) request;
        HttpServletResponse res = (HttpServletResponse) response;
        
        // Option 1: Login URL
        String loginURI = req.getContextPath() + "/login";
        boolean loginRequest = req.getRequestURI().toLowerCase().contains(loginURI.toLowerCase());
        
        // Option 2: Resources
        boolean resourceRequest = req.getRequestURI().startsWith(req.getContextPath() + "/assets");
        
        // Option 3: Session/Cookie
        String key = "username";
        boolean loggedIn = false;
        Cookie[] cookies = req.getCookies();
        if (cookies != null) {
            for (Cookie c : cookies) {
                if (key.equals(c.getName())) {
                    loggedIn = true;
                    request.setAttribute(key, c.getValue());
                    break;
                }
            }
        }
        
        if (loggedIn || loginRequest || resourceRequest) {
            chain.doFilter(request, response); 
        } else {
            res.sendRedirect(loginURI); 
        }
    }
}
```

- Remove check cookie in HomeServlet.java

```java
public class HomeServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {
        ProductDAO productDAO = new ProductDAO();
        List<Product> products = productDAO.GetAllProducts();
        request.setAttribute("products", products);
        
        request.setAttribute("contentPage", "home_content.jsp");
        request.getRequestDispatcher("/views/pages/home.jsp").forward(request,response);
    } 
}
```

---
## Note: Has 3 option can't pass via filter
> - Option 1: login page
> - Option 2: Public resource such as "assets/*"
> - Option 3: Has login

[Source Demo](https://github.com/shandyprofile/java-jsp-shop-basic/tree/main/jsp-shop-13)