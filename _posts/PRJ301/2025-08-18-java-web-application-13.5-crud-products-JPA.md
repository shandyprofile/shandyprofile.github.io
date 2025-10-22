---
title: Epoch 13.5 – CRUD Products (JPA)
description: >-
  Upgrade CRUD Product using JPA (ORM)
author: [shandy]
date: 2025-08-18
updateDate: 2025-08-21
categories: [(Java) Web Application, (PBL) Shopping Web]
tags: [(Java - PBL) Shopping Web]
sort_index: 114
# pin: true
# media_subpath: '/posts/01'
---

## Architechture Project

```
/src
  ├── controllers/
  │     ├── LoginServlet.java
  │     ├── LogoutServlet.java
  │     ├── ProductServlet.java
  ├── models/
  │     ├── User.java
  │     ├── Product.java
  ├── repositories/             //  Replace DALs (DAO)
  │     ├── UserRepository.java
  │     ├── ProductRepository.java
  ├── Utils/
  │     ├── JPAUtil.java        //  That is EntityManagerFactory
                                //  Replace DBContext.java
  ├── filters/
  │     ├── AuthFilter.java (lọc quyền truy cập)
  │
/webapp
  ├── META-INF/
  │     ├── persistence.xml     //  JPA Configuration
  ├── WEB-INF/
  │     ├── web.xml
  │     ├── tags/
  │     ├── layout.tag  
  ├── views/
  │     ├── login.jsp
  │     ├── home.jsp
  │     ├── product-management/
  │     │     ├── addProduct.jsp  
  │     │     ├── updateProduct.jsp  
  │     │     ├── listProduct.jsp
  │     │     ├── deleteProduct.jsp  
```

## Install JPA in Hibernate

Add dependencies into porm.xml in projects:

```xml
<!-- JPA API -->
<dependency>
    <groupId>jakarta.persistence</groupId>
    <artifactId>jakarta.persistence-api</artifactId>
    <version>3.1.0</version>
</dependency>

<!-- Hibernate Core -->
<dependency>
    <groupId>org.hibernate.orm</groupId>
    <artifactId>hibernate-core</artifactId>
    <version>6.3.0.Final</version>
</dependency>
```

> **Make sure** you have `Build with dependencies` **before running the project**

## 1. persistence.xml?

`persistence.xml` is the main configuration file used by JPA (Jakarta Persistence API) to define how your Java application connects to a database and manages entities.

It is typically placed inside:

```
src/main/resources/META-INF/persistence.xml
```

This file tells the JPA provider (such as Hibernate, EclipseLink, etc.):
- Which entities (models) to manage.
- How to connect to the database (JDBC settings).
- Which JPA provider and dialect to use.
- Optional settings like auto table creation, logging, caching, etc.

### 1.1 Persistence Unit

Inside persistence.xml, you define one or more persistence units — each representing a separate database configuration.

Each persistence unit is identified by a name:

```xml
<persistence-unit name="JSPShop">
    ...
</persistence-unit>
```

> Note: **name** of **persistence-unit**. That is a name which select in DBContext. **We need exact name.**

When your code calls:

```java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("JSPShop");
```

JPA looks up the configuration with the name **JSPShop** from persistence.xml and uses it to connect to the database.

### 1.2 Example of persistence.xml in JSPShop:

Here’s a typical configuration using **Hibernate** with SQL Server:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence xmlns="https://jakarta.ee/xml/ns/persistence" version="3.0">
  <persistence-unit name="MyAppPU">
    <!-- Register entity classes -->
    <class>com.example.app.models.User</class>
    <class>com.example.app.models.Product</class>

    <!-- Database connection properties -->
    <properties>
      <property name="jakarta.persistence.jdbc.driver" value="com.microsoft.sqlserver.jdbc.SQLServerDriver"/>
      <property name="jakarta.persistence.jdbc.url" value="<CONNECTIONSTRING>"/>
      <property name="jakarta.persistence.jdbc.user" value="<USER>"/>
      <property name="jakarta.persistence.jdbc.password" value="<PASSWORDS>"/>

      <!-- Hibernate properties -->
      <property name="hibernate.dialect" value="org.hibernate.dialect.SQLServerDialect"/>
      <property name="hibernate.hbm2ddl.auto" value="update"/>
      <property name="hibernate.show_sql" value="true"/>
      <property name="hibernate.format_sql" value="true"/>
    </properties>
  </persistence-unit>
</persistence>
```

You need to fill all SQL Server infomation same as DBContext:
- `<CONNECTIONSTRING>`: Connection String
- `<USER>` and `<PASSWORDS>`: User login into SQL Server.

### 1.3 Key Elements

| Tag / Property                               | Description                                                                 |
| -------------------------------------------- | --------------------------------------------------------------------------- |
| `<persistence-unit name="...">`              | Defines a configuration group for JPA. Each unit has its own DB connection. |
| `<class>`                                    | Lists all entity classes that should be managed by JPA.                     |
| `jakarta.persistence.jdbc.driver`            | The JDBC driver used to connect to the database.                            |
| `jakarta.persistence.jdbc.url`               | The database connection URL.                                                |
| `jakarta.persistence.jdbc.user` / `password` | Credentials for the DB connection.                                          |
| `hibernate.hbm2ddl.auto`                     | Defines schema generation strategy: `create`, `update`, `validate`, `none`. |
| `hibernate.show_sql`                         | Prints SQL statements in the console for debugging.                         |
| `hibernate.format_sql`                       | Formats SQL output for better readability.                                  |

Common `hibernate.hbm2ddl.auto` Values:

| Value         | Behavior                                                      |
| ------------- | ------------------------------------------------------------- |
| `create`      | Drops and recreates the database schema on every startup.     |
| `create-drop` | Creates schema at startup and drops it when the app stops.    |
| `update`      | Updates schema automatically to match entities (most common). |
| `validate`    | Checks that schema matches entities; throws error if not.     |
| `none`        | Does nothing (manual schema management).                      |

## 2. Update code-behind

- Step 1: Using `JPAUtil.java` instead of `DBConect.java` 
- Step 2: Update `Models` by **anonymous parammeter** in `private variables`
- Step 3: Using `Repositories` pattern to contain data instead of `DALs`
- Step 4: Apply `Repositories` for `Servlets`.

### 2.1 JPAUtil.java (Old DBConect.java)

The JPA-based equivalent, which replaces DBContext.java.

```java
package Utils;

import jakarta.persistence.EntityManagerFactory;
import jakarta.persistence.Persistence;

public class JPAUtil {
    private static final EntityManagerFactory emf =
            Persistence.createEntityManagerFactory("JSPShop");

    public static EntityManagerFactory getEntityManagerFactory() {
        return emf;
    }

    public static void close() {
        emf.close();
    }
}
```

### 2.2 Update Models:

Update User Model (User.java)

```java
package Models;

import jakarta.persistence.*;

@Entity
@Table(name = "Users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "UserID")
    private int id;

    @Column(name = "Username", nullable = false, length = 50, unique = true)
    private String name;

    @Column(name = "Password", nullable = false, length = 255)
    private String password;

    @Column(name = "Role", nullable = false, length = 20)
    private String role;

    public User() {
        this.id = -1;
        this.name = "";
        this.password = "";
        this.role = "";
    }

    public User(int id, String name, String password, String role) {
        this.id = id;
        this.name = name;
        this.password = password;
        this.role = role;
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

    public String getPassword() {
        return password;
    }
    public void setPassword(String password) {
        this.password = password;
    }

    public String getRole() {
        return role;
    }
    public void setRole(String role) {
        this.role = role;
    }

    public static boolean isEmpty(User user) {
        return user == null || (user.getName() == null || user.getName().isEmpty());
    }
}
```

Update Product Models (Product.java):

```java
package Models;
import jakarta.persistence.*;

@Entity
@Table(name = "Products")
public class Product {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    @Column(name = "name", nullable = false, length = 100)
    private String name;

    @Column(name = "description", columnDefinition = "TEXT")
    private String description;

    @Column(name = "price", nullable = false, precision = 10, scale = 2)
    private double price;

    @Column(name = "quantity", nullable = false)
    private int quantity;

    @Column(name = "image", nullable = false, length = 100)
    private String image;

    public Product() {
        this.id = 0;
        this.name = "";
        this.price = 0;
        this.description = "";
        this.quantity = 0;
        this.image = "";
    }

    public Product(String name, double price, String description, int quantity, String image) {
        this.name = name;
        this.price = price;
        this.description = description;
        this.quantity = quantity;
        this.image = image;
    }

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

    public String getDescription() {
        return description;
    }
    public void setDescription(String description) {
        this.description = description;
    }

    public double getPrice() {
        return price;
    }
    public void setPrice(double price) {
        this.price = price;
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

### 2.2 Repository Layer (replace DALs)

Create UserRepository (Replace DALs/UserDAO.java)

```java
package Repositories;

import Models.User;
import Utils.JPAUtil;
import jakarta.persistence.*;
import java.util.List;

public class UserRepository {
    private EntityManager em;

    public UserRepository() {
        this.em = JPAUtil.getEntityManagerFactory().createEntityManager();
    }

    public List<User> getAll() {
        String sql = "SELECT * FROM Users";
        Query query = em.createNativeQuery(sql, User.class);
        return query.getResultList();
    }

    public User checkUser(String username, String password) {
        String sql = "SELECT * FROM Users WHERE Username = ? AND Password = ?";
        try {
            Query query = em.createNativeQuery(sql, User.class);
            query.setParameter(1, username);
            query.setParameter(2, password);

            return (User) query.getSingleResult();
        } catch (NoResultException e) {
            return null;
        }
    }

    public void addUser(User user) {
        EntityTransaction tx = em.getTransaction();
        try {
            tx.begin();
            em.persist(user);
            tx.commit();
        } catch (Exception e) {
            if (tx.isActive()) tx.rollback();
            e.printStackTrace();
        }
    }

    public void updateUser(User user) {
        EntityTransaction tx = em.getTransaction();
        try {
            tx.begin();
            em.merge(user);
            tx.commit();
        } catch (Exception e) {
            if (tx.isActive()) tx.rollback();
            e.printStackTrace();
        }
    }

    public void deleteUser(int id) {
        EntityTransaction tx = em.getTransaction();
        try {
            tx.begin();
            User user = em.find(User.class, id);
            if (user != null) {
                em.remove(user);
            }
            tx.commit();
        } catch (Exception e) {
            if (tx.isActive()) tx.rollback();
            e.printStackTrace();
        }
    }

    public void close() {
        if (em.isOpen()) {
            em.close();
        }
    }
}
```

Create ProductRepository (Replace DALs/ProductDAO.java)


```java
package Repositories;

import Models.Product;
import Utils.JPAUtil;
import jakarta.persistence.*;
import java.util.List;

public class ProductRepository {
    private EntityManager em;

    public ProductRepository() {
        this.em = JPAUtil.getEntityManagerFactory().createEntityManager();
    }

    public List<Product> getAllProducts() {
        String sql = "SELECT * FROM Products";
        Query query = em.createNativeQuery(sql, Product.class);
        return query.getResultList();
    }

    public Product getProductById(int id) {
        String sql = "SELECT * FROM Products WHERE id = ?";
        try {
            Query query = em.createNativeQuery(sql, Product.class);
            query.setParameter(1, id);
            return (Product) query.getSingleResult();
        } catch (NoResultException e) {
            return null;
        }
    }

    public boolean createProduct(String name, double price, String description, int quantity, String img) {
        EntityTransaction tx = em.getTransaction();
        try {
            tx.begin();
            Product product = new Product(name, price, description, quantity, img);
            em.persist(product);
            tx.commit();
            return true;
        } catch (Exception e) {
            if (tx.isActive()) tx.rollback();
            e.printStackTrace();
            return false;
        }
    }

    public boolean updateProductById(int id, String name, double price, String description, int quantity, String img) {
        EntityTransaction tx = em.getTransaction();
        try {
            tx.begin();
            Product product = em.find(Product.class, id);
            if (product != null) {
                product.setName(name);
                product.setPrice(price);
                product.setDescription(description);
                product.setQuantity(quantity);
                product.setImage(img);
                em.merge(product);
            }
            tx.commit();
            return product != null;
        } catch (Exception e) {
            if (tx.isActive()) tx.rollback();
            e.printStackTrace();
            return false;
        }
    }

    public boolean deleteProductById(int id) {
        EntityTransaction tx = em.getTransaction();
        try {
            tx.begin();
            Product product = em.find(Product.class, id);
            if (product != null) {
                em.remove(product);
            }
            tx.commit();
            return product != null;
        } catch (Exception e) {
            if (tx.isActive()) tx.rollback();
            e.printStackTrace();
            return false;
        }
    }

    public void close() {
        if (em.isOpen()) em.close();
    }
}
```

### 2.3 Using JPA via Repositories

Replace DAOs to Repositories. Example in LoginServlet.java:

```java
// Replace
UserDAO users = new UserDAO();
String uName = users.CheckUser(username, password);

// To 
UserRepository userRepository = new UserRepository();
String uName = userRepository.CheckUser(username, password);
```

> **Same as another places**

`GOOD LUCK!!! :D`