---
title: Epoch 7 - Login by SQL Server
description: >-
  Implement a basic login/logout feature using hardcoded credentials in JSP Model 1 architecture, with both Session and Cookie handling options.
author: [shandy]
date: 2025-08-14
categories: [(Java) Web Application, (PBL) Shopping Web]
tags: [(Java - PBL) Shopping Web]
sort_index: 107
# pin: true
# media_subpath: '/posts/01'
---

## Architechture for project:

```
/views/
    ├── layouts
        ├── layout.jsp
    ├── pages
        ├── login.jsp
        ├── login_form_content.jsp
        ├── logout.jsp
        ├── home.jsp
        ├── home_content.jsp
/src/
    ├── DALs
        ├── UserDAO.java
    ├── Models
        ├── User.java
```

## SQL server:

### Connection String:

- Connection String in DBContext:
```java
String url = "jdbc:sqlserver://localhost:1433;"
        + "databaseName=JSPShop;"
        + "user=sa;"
        + "password=123456x@X;"
        + "encrypt=true;"
        + "trustServerCertificate=true;";
```

- Users Model in SQL Server:

```sql
CREATE TABLE Users (
    UserID INT PRIMARY KEY IDENTITY(1,1),
    Username NVARCHAR(50) NOT NULL UNIQUE,
    Password NVARCHAR(255) NOT NULL,
    Role NVARCHAR(20) NOT NULL
);

INSERT INTO Users (Username, Password, Role) VALUES
(N'admin', 'admin123', N'ADMIN'),
(N'john', '123456', N'CUSTOMER');
```

## Code project

- Create **User.java**:

```java
public class User {
    private String id;
    private String name;
    private String password;
    private String role;

    public User() {
        id = "";
        name = "";
        password = "";
        role = "";
    }
    
    public User(String id, String name, String password, String role) {
        this.id = id;
        this.name = name;
        this.password = password;
        this.role = role;
    }
    
    public static boolean isEmpty(User user) {
        return user.getId().isEmpty() && user.getName().isEmpty();
    }

    public User(String string, String string0, String string1) {
        throw new UnsupportedOperationException("Not supported yet."); // Generated from nbfs://nbhost/SystemFileSystem/Templates/Classes/Code/GeneratedMethodBody
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
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
}
```

- Create **UserDAO.java**:

```java
import Utils.DBContext;
import Models.*;
import java.sql.*;
import java.util.ArrayList;
import java.util.List;
import java.util.logging.Level;
import java.util.logging.Logger;

/**
 *
 * @author shandy
 */
public class UserDAO extends DBContext {
    public UserDAO() {
        super();
    }
    
    public List<User> GetAll() {
        List<User> list = new ArrayList<>();
        String query = "Select * from Users";
        try {
            PreparedStatement ps = connection.prepareStatement(query);            
            ResultSet rs = ps.executeQuery();
            while (rs.next()) {
                User cus = new User(
                        rs.getString("UserID"), 
                        rs.getString("Username"), 
                        rs.getString("Password"),
                        rs.getString("Role"));
                list.add(cus); 
            }
        } catch (SQLException e) {
            System.out.println(e);
        }
        return list;
    }
    
    public String CheckUser(String userName, String password) {
        String query = "Select * from Users where Username = ? and Password = ?;";
        try {
            PreparedStatement pstmt = connection.prepareStatement(query);
            pstmt.setString(1, userName);
            pstmt.setString(2, password);
            
            ResultSet rs = pstmt.executeQuery();
            if (rs.next()) {
                User cus = new User(
                        rs.getString("UserID"), 
                        rs.getString("Username"), 
                        rs.getString("Password"),
                        rs.getString("Role"));
                return rs.getString("Username");
            }
            return null;
        } catch (SQLException e) {
            System.out.println(e);
        }
        
        return null;
    }
}
```

- Update **login.jsp**

```jsp
    <%
        // ...
        UserDAO userDAO = new UserDAO();
        String name = userDAO.CheckUser(username, password);
        if (name != null) {
            Cookie cookie = new Cookie("username", name);
            cookie.setMaxAge(60 * 60); // 1 hour
            response.addCookie(cookie);

            response.sendRedirect("home.jsp");
            return;
        } else {
            request.setAttribute("loginError", "Username or password are failed!");
        }
        // ...
    %>
```

[Source Demo](https://github.com/shandyprofile/java-jsp-shop-basic/tree/main/jsp-shop-07)