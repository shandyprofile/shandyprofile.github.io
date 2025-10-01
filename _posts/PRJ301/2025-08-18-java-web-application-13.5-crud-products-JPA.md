---
title: Epoch 13.5 – CRUD Products
description: >-
  Add and use JSTL and EL in your Maven JSP/Servlet project.
author: [shandy]
date: 2025-08-18
updateDate: 2025-08-21
categories: [(Java) Web Application, (PBL) Shopping Web]
tags: [(Java - PBL) Shopping Web]
sort_index: 114
# pin: true
# media_subpath: '/posts/01'
---

## 1. SQL Products


```sql
CREATE TABLE Products (
    id INT PRIMARY KEY IDENTITY(1,1),
    name VARCHAR(100) NOT NULL,
    description TEXT,
    price DECIMAL(10, 2) NOT NULL,
    quantity INT NOT NULL,
    image VARCHAR(100) NOT NULL
);

INSERT INTO Products (name, description, price, quantity, image)
VALUES
('Laptop Dell XPS 13', '13-inch Ultrabook with Intel i7 processor', 1299.99, 10, 'product1.jpg'),
('iPhone 15 Pro', 'Apple smartphone with A17 Bionic chip', 999.00, 20, 'product1.jpg'),
('Sony WH-1000XM5', 'Noise-cancelling wireless headphones', 349.99, 15, 'product1.jpg'),
('Logitech MX Master 3S', 'Ergonomic wireless mouse', 99.99, 30, 'product1.jpg'),
('Samsung 4K Monitor 27"', 'Ultra HD monitor with HDR support', 299.50, 12, 'product1.jpg');
```

## 2. Code CRUD

Update Models/Product.java

```java
public class Product {
    private int id;
    private String name;
    private double price;
    private String description;
    private int quantity;
    private String image;

    public Product(int id, String name, double price, String description, int quantity, String image) {
        this.id = id;
        this.name = name;
        this.price = price;
        this.description = description;
        this.quantity = quantity;
        this.image = image;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
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

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public int getQuantity() {
        return quantity;
    }

    public void setQuantity(int quantity) {
        this.quantity = quantity;
    }

    public String getImage() {
        return image;
    }

    public void setImage(String image) {
        this.image = image;
    }
}
```

### 1. Read all products

- Create DALs/ProductDAO.java:

```java
public class ProductDAO extends DBContext {
    public ProductDAO() {
        super();
    }

    public List<Product> GetAllProducts() {
        List<Product> listProducts = new ArrayList<>();
        String query = "Select * from Products";
        try {
            PreparedStatement ps = connection.prepareStatement(query);            
            ResultSet rs = ps.executeQuery();
            while (rs.next()) {
                Product product = new Product(
                        rs.getInt("id"), 
                        rs.getString("name"), 
                        rs.getDouble("price"),
                        rs.getString("description"),
                        rs.getInt("quantity"),
                        rs.getString("image"));
                
                listProducts.add(product); 
            }
        } catch (SQLException e) {
            System.out.println(e);
        }
        
        return listProducts;
    }
}
```

- Update HomeServlet.java

```java
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

        request.setAttribute("username", username);
        
        ProductDAO productDAO = new ProductDAO();
        List<Product> products = productDAO.GetAllProducts();
        request.setAttribute("products", products);
    
        request.getRequestDispatcher("/views/home.jsp").forward(request,response);
    } 
// ...
```

- Update home.jsp

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt" %>
<%@ page contentType="text/html;charset=UTF-8" %>
    
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
    
    
<hr/>
    
<h3>Available Products</h3>
<c:choose>
    <c:when test="${not empty requestScope.products}">
        <table class="table table-bordered">
            <thead>
                <tr>
                    <th>#</th>
                    <th>Name</th>
                    <th>description</th>
                    <th>Price</th>
                    <th>Quantity</th>
                    <th>Image</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody>
                <c:forEach var="p" items="${products}" varStatus="status">
                    <tr>
                        <td>${status.index + 1}</td>
                        <td>${p.getName()}</td>
                        <td>${p.getDescription()}</td>
                        <td><fmt:formatNumber value="${p.getPrice()}" type="currency" currencySymbol="VND" /></td>
                        <td>${p.getQuantity()}</td>
                        <td>
                            <img height='50px' src="./assets/images/${p.getImage()}" alt="${p.getName()}"/>
                        </td>
                        <td>
                            // Add Edit/Delete action
                        </td>
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

### 2. Add new product

- Update DALs/ProductDAO.java:

```java
    // ...
    public boolean CreateProduct(String name, double price, String description, int quantity, String img) {
        String query = "INSERT INTO Products (name, description, price, quantity, image)\n" +
"VALUES (?, ?, ?, ?, ?);";
        try {
            PreparedStatement ps = connection.prepareStatement(query);
            ps.setString(1, name);
            ps.setString(2, description);
            ps.setDouble(3, price);
            ps.setInt(4, quantity);
            ps.setString(5, img);
            int rs = ps.executeUpdate();
            return rs > 0;
            
        } catch (SQLException e) {
            System.out.println(e);
        }
        return false;        
    }
```

- Create Controllers/ProductServlet.java

```java
    package Controllers;

import DALs.ProductDAO;
import Models.Product;
import java.io.IOException;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

/**
 *
 * @author shandy
 */
public class ProductServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {
        String action = request.getParameter("action");
        
        switch (action) {
            case "add":
                request.setAttribute("contentPage", "addProduct_content.jsp");
                request.getRequestDispatcher("/views/addProduct.jsp").forward(request,response);
                break;
            case "update":
                
                break;
                
            case "delete":
                
                break;
        }

        
    } 

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {
        String action = request.getParameter("action");
        
        switch (action) {
            case "add":
                AddActionPost(request, response);
                break;
            case "update":
                
                break;
                
            case "delete":
                
                break;
        }
    }
    
    private void AddActionPost(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
        String name = request.getParameter("name");
        String description = request.getParameter("description");
        String price = request.getParameter("price");
        String quantity = request.getParameter("quantity");
        String img = request.getParameter("img");
        
        ProductDAO products = new ProductDAO();
        boolean rs = products.CreateProduct(name, Double.parseDouble(price), description, Integer.parseInt(quantity), img);
        if (rs) {           
            response.sendRedirect(request.getContextPath() + "/home");
        } else {
            request.setAttribute("contentPage", "addProduct_content.jsp");
            request.setAttribute("error", "Add products Error!");
            request.getRequestDispatcher("/views/pages/addProduct.jsp").forward(request,response);
        }
    }
}
```

- Update Button Add Product in home page: (home_content.jsp) into line 29.

```jsp
<!-- line 29 -->

<a href="${pageContext.request.contextPath}/product?action=add" 
   class="btn btn-primary">
   Add product 
</a>
```

> Create Add page
- Create views/addProduct.jsp

```jsp

<h2 class="mb-4">Add Product</h2>
<form action="${pageContext.request.contextPath}/product?action=add" method="post" class="col-md-4">
    <div class="mb-3">
        <label for="name" class="form-label">Name</label>
        <input type="text" id="name" name="name" class="form-control" required>
    </div>
    <div class="mb-3">
        <label for="description" class="form-label">Description</label>
        <input type="text" id="description" name="description" class="form-control" required>
    </div>
    <div class="mb-3">
        <label for="price" class="form-label">Price</label>
        <input type="text" id="price" name="price" class="form-control" required>
    </div>
    <div class="mb-3">
        <label for="quantity" class="form-label">Quantity</label>
        <input type="text" id="quantity" name="quantity" class="form-control" required>
    </div>
    <div class="mb-3">
        <label for="img" class="form-label">Image</label>
        <input type="text" id="img" name="img" class="form-control" required>
    </div>
    <button type="submit" class="btn btn-primary">Add Product</button>
    <a href="${pageContext.request.contextPath}/home" class="btn btn-outline-info">Back</a>
    
    <p style="color:red;"><%= request.getAttribute("error") != null ? request.getAttribute("error") : ""%></p>
</form>
```

### 3. Update product by id

- Update DALs/ProductDAO.java:

```java
    public boolean UpdateProductById(String id, String name, double price, String description, int quantity, String img) {
        String query = "Update Products SET "
                + "name = ?, "
                + "description = ?, "
                + "price = ?, "
                + "quantity = ?, "
                + "image = ? "
                + "where id = ? ;";
        try {
            PreparedStatement ps = connection.prepareStatement(query);
            ps.setString(1, name);
            ps.setString(2, description);
            ps.setDouble(3, price);
            ps.setInt(4, quantity);
            ps.setString(5, img);
            ps.setString(6, id);
            int rs = ps.executeUpdate();
            return rs > 0;
            
        } catch (SQLException e) {
            System.out.println(e);
        }
        return false;        
    }
```

- Update Controllers/ProductServlet.java:

```java
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {
        //...
            case "update":
                String idProduct = request.getParameter("id");
                ProductDAO productDAO = new ProductDAO();
                Product pro = productDAO.GetProductById(Integer.parseInt(idProduct));
                request.setAttribute("product", pro);
                request.setAttribute("contentPage", "updateProduct_content.jsp");
                request.getRequestDispatcher("/views/pages/updateProduct.jsp").forward(request,response);
                break;
        //...
    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {
        // ...
        
            case "update":
                UpdateActionPost(request, response);
                break;

        // ...
    }

    private void UpdateActionPost(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
        String id = request.getParameter("id");
        String name = request.getParameter("name");
        String description = request.getParameter("description");
        String price = request.getParameter("price");
        String quantity = request.getParameter("quantity");
        String img = request.getParameter("img");
        
        ProductDAO products = new ProductDAO();
        boolean rs = products.UpdateProductById(id, name, Double.parseDouble(price), description, Integer.parseInt(quantity), img);
        if (rs) {           
            response.sendRedirect(request.getContextPath() + "/home");
        } else {
            request.setAttribute("contentPage", "updateProduct_content.jsp");
            request.setAttribute("error", "Update products error!");
            request.getRequestDispatcher("/views/pages/updateProduct.jsp").forward(request,response);
        }
    }
```

- Update home-content.jsp

```jsp
    <!-- Line 61 -->
    <td>
        <a href="${pageContext.request.contextPath}/product?action=update&id=${p.getId()}" class="btn btn-warning">
            Update
        </a>
    </td>
```

- Create updateProduct.jsp

```jsp
<jsp:include page="../layouts/layout.jsp" >
    <jsp:param name="pageTitle" value="Update Product - JSP Shop" />
</jsp:include>
```

- Create updateProduct-content.jsp

```jsp
<h2 class="mb-4">Add Product</h2>
<form action="${pageContext.request.contextPath}/product?action=update" method="post" class="col-md-4">
    <div class="mb-3">
        <label for="id" class="form-label">Id</label>
        <input type="text" id="id" name="id" class="form-control" readonly value="${product.getId()}" >
    </div>
    
    <div class="mb-3">
        <label for="name" class="form-label">Name</label>
        <input type="text" id="name" name="name" class="form-control" value="${product.getName()}" required>
        
    </div>
    <div class="mb-3">
        <label for="description" class="form-label">Description</label>
        <input type="text" id="description" name="description" class="form-control" value="${product.getDescription()}" required>
    </div>
    <div class="mb-3">
        <label for="price" class="form-label">Price</label>
        <input type="text" id="price" name="price" class="form-control" value="${product.getPrice()}" required>
    </div>

    <div class="mb-3">
        <label for="quantity" class="form-label">Quantity</label>
        <input type="text" id="quantity" name="quantity" class="form-control" value="${product.getQuantity()}" required>
    </div>

    <div class="mb-3">
        <label for="img" class="form-label">Image</label>
        <input type="text" id="img" name="img" class="form-control" value="${product.getImage()}" required>
    </div>
    
    <button type="submit" class="btn btn-primary">Update Product</button>
    <a href="${pageContext.request.contextPath}/home" class="btn btn-outline-info">Back</a>
    
    <p style="color:red;"><%= request.getAttribute("error") != null ? request.getAttribute("error") : ""%></p>

</form>
```

### 4. Delete product by id

- Update DALs/ProductDAO.java:

```java
    public boolean DeleteProductById(String id) {
        String query = "Delete Products where id = ? ;";
        try {
            PreparedStatement ps = connection.prepareStatement(query);
            ps.setString(1, id);
            int rs = ps.executeUpdate();
            return rs > 0;
            
        } catch (SQLException e) {
            System.out.println(e);
        }
        return false; 
    }
```

- Update Controllers/ProductServlet.java:

```java
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {
        //...
            case "delete":
                String idProduct1 = request.getParameter("id");
                ProductDAO productDAO1 = new ProductDAO();
                Product pro1 = productDAO1.GetProductById(Integer.parseInt(idProduct1));
                request.setAttribute("product", pro1);
                request.setAttribute("contentPage", "deleteProduct_content.jsp");
                request.getRequestDispatcher("/views/pages/deleteProduct.jsp").forward(request,response);
                break;
        //...
    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response)
    throws ServletException, IOException {
        // ...
        
            case "update":
                DeleteActionPost(request, response);
                break;

        // ...
    }

    private void DeleteActionPost(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
        String id = request.getParameter("id");
        
        ProductDAO products = new ProductDAO();
        boolean rs = products.DeleteProductById(id);
        if (rs) {           
            response.sendRedirect(request.getContextPath() + "/home");
        } else {
            request.setAttribute("contentPage", "deleteProduct_content.jsp");
            request.setAttribute("error", "Delete products error!");
            request.getRequestDispatcher("/views/pages/deleteProduct.jsp").forward(request,response);
        }
    }
```

- Update home-content.jsp

```jsp
    <!-- Line 61 -->
    <td>
        <a href="${pageContext.request.contextPath}/product?action=update&id=${p.getId()}" class="btn btn-warning">
            Update
        </a>

        <a href="${pageContext.request.contextPath}/product?action=delete&id=${p.getId()}" class="btn btn-danger">
            Delete
        </a>
    </td>
```

- Create deleteProduct.jsp

```jsp
<jsp:include page="../layouts/layout.jsp" >
    <jsp:param name="pageTitle" value="Delete Product - JSP Shop" />
</jsp:include>
```

- Create deleteProduct-content.jsp

```jsp
<h2 class="mb-4">Add Product</h2>
<form action="${pageContext.request.contextPath}/product?action=delete" method="post" class="col-md-4">
    <div class="mb-3">
        <label for="id" class="form-label">Id</label>
        <input type="text" id="id" name="id" class="form-control" readonly value="${product.getId()}" >
    </div>
    
    <div class="mb-3">
        <label for="name" class="form-label">Name</label>
        <input type="text" id="name" class="form-control" value="${product.getName()}" readonly>
    </div>
    <div class="mb-3">
        <label for="description" class="form-label">Description</label>
        <input type="text" id="description" class="form-control" value="${product.getDescription()}" readonly>
    </div>
    <div class="mb-3">
        <label for="price" class="form-label">Price</label>
        <input type="text" id="price" class="form-control" value="${product.getPrice()}" readonly>
    </div>

    <div class="mb-3">
        <label for="quantity" class="form-label">Quantity</label>
        <input type="text" id="quantity" class="form-control" value="${product.getQuantity()}" readonly>
    </div>

    <div class="mb-3">
        <label for="img" class="form-label">Image</label>
        <input type="text" id="img" class="form-control" value="${product.getImage()}" readonly>
    </div>
    
    <button type="submit" class="btn btn-danger">Delete Product</button>
    
    <a href="${pageContext.request.contextPath}/home" class="btn btn-outline-info">Back</a>

    <p style="color:red;"><%= request.getAttribute("error") != null ? request.getAttribute("error") : ""%></p>

</form>
```

[Source Demo](https://github.com/shandyprofile/java-jsp-shop-basic/tree/main/jsp-shop-12)