---
title: Epoch 4 - JDBC Driver Integration & DB Setup
description: >-
  JDBC Driver Integration & DB Setup </br>
  1. Has a clear goal at the start. </br>
  2. Includes SQL Server configuration prerequisites. </br>
  3. Provides a JDBC driver integration guide for NetBeans Maven. </br>
  4. Adds a SQL script to create a sample database, tables, and some initial data for the jsp-shop project.
author: [shandy]
date: 2025-08-12
categories: [(Java) Web Application, (PBL) Shopping Web]
tags: [(Java - PBL) Shopping Web]
sort_index: 104
# pin: true
# media_subpath: '/posts/01'
---
## Step 1: Verify SQL Server Configuration
Before coding, make sure SQL Server is ready:
- Enable TCP/IP
  - Open SQL Server Configuration Manager.
  - Navigate to SQL Server Network Configuration → Protocols for MSSQLSERVER.
  - Right-click TCP/IP → Enable.
  - Double-click TCP/IP → IP Addresses tab → Find IPAll section.
  - Set TCP Port to 1433.
  - Click OK → Restart SQL Server service.
- Enable Mixed Mode Authentication (SQL Server + Windows)
- Open SQL Server Management Studio (SSMS).
- Right-click server → Properties → Security.
- Select SQL Server and Windows Authentication mode → OK.
- Restart SQL Server service.
- Create a SQL login (example: **sa** with password **P@ssw0rd**).

## Step 2: Add JDBC Driver to Maven
- In pom.xml, add Microsoft SQL Server JDBC driver:

```xml
<dependencies>
    <!-- SQL Server JDBC Driver -->
    <dependency>
        <groupId>com.microsoft.sqlserver</groupId>
        <artifactId>mssql-jdbc</artifactId>
        <version>11.2.0.jre8</version>
    </dependency>
</dependencies>
```
- Run Right-click Project → Clean and Build to download dependencies.

## Step 3: Create Database and Tables
Open SSMS → New Query → Run:

```sql
-- Create Database
CREATE DATABASE JSPShop;
GO

USE JSPShop;
GO

-- Product Table
CREATE TABLE Products (
    ProductID INT PRIMARY KEY IDENTITY(1,1),
    Name NVARCHAR(100) NOT NULL,
    Price DECIMAL(10,2) NOT NULL,
    Stock INT NOT NULL
);

-- Users Table
CREATE TABLE Users (
    UserID INT PRIMARY KEY IDENTITY(1,1),
    Username NVARCHAR(50) NOT NULL UNIQUE,
    Password NVARCHAR(255) NOT NULL,
    Role NVARCHAR(20) NOT NULL
);

-- Insert sample data
INSERT INTO Products (Name, Price, Stock) VALUES
(N'Laptop Dell', 1500.00, 10),
(N'Logitech Mouse', 25.50, 50),
(N'Auto Keyboard', 75.00, 30);

INSERT INTO Users (Username, Password, Role) VALUES
(N'admin', 'admin123', N'ADMIN'),
(N'john', '123456', N'CUSTOMER');
```

## Step 4: Create DBContext class in Utility package
- Create package: Util. 
- Create DBContext.java with the code you specified.

```java
package com.jsp.shop.utils;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class DBContext {
    protected Connection connection;

    public DBContext() {
        try {
            String url = "jdbc:sqlserver://localhost:1433;"
                    + "databaseName=JSPShop;"
                    + "user=sa;"
                    + "password=P@ssword123;"
                    + "encrypt=true;"
                    + "trustServerCertificate=true;";
            Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
            connection = DriverManager.getConnection(url);
        } catch (ClassNotFoundException | SQLException ex) {
            System.out.println("Database connection failed: " + ex);
        }
    }
}
```

> **Notes:**
- Replace bussinessProject, sa, and password with your real DB name/credentials.
- encrypt=true;trustServerCertificate=true; is useful for local dev; adjust for production.
- protected Connection connection; lets subclasses/DAOs access the connection. You should still close connections when finished (see Improvements below).

## Step 5: Test the Connection
- Create package: Controllers. 
- Add TestDBServlet.java. Use Jakarta imports (Tomcat 10).

```java
package Controllers;

import dal.DBContext;
import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

public class TestDBServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        response.setContentType("text/html;charset=UTF-8");

        try (PrintWriter out = response.getWriter()) {
            DBContext db = new DBContext();
            Connection conn = db.connection;
            if (conn != null && !conn.isClosed()) {
                out.println("<h1>Database Connection Successful!</h1>");
            } else {
                out.println("<h1>Database Connection Failed!</h1>");
            }
            // Optionally close the connection here if DBContext didn't manage it.
            try {
                if (conn != null && !conn.isClosed()) conn.close();
            } catch (Exception ignored) {}
        } catch (Exception ex) {
            // Print a friendly message to browser and full stack trace to server logs
            response.getWriter().println("<h1>Test failed: " + ex.getMessage() + "</h1>");
            ex.printStackTrace();
        }
    }
}
```

> Note:Why no @WebServlet?

```java 
@WebServlet("/testdb")
public class TestDBServlet extends HttpServlet
```
We update in web.xml

## Step 5: Configure web.xml mapping

File: ./WEB-INF/web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee
                             https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
         version="5.0">

    <servlet>
        <servlet-name>TestDBServlet</servlet-name>
        <servlet-class>Controllers.TestDBServlet</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>TestDBServlet</servlet-name>
        <url-pattern>/testdb</url-pattern>
    </servlet-mapping>

    <!-- welcome file list if needed -->
    <welcome-file-list>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>
</web-app>
```

## Build & Deploy
- Clean & Build the project in NetBeans.
- Run the project (Deploy to Tomcat from NetBeans). Ensure the output window shows a successful deploy.
```url
http://localhost:8080/<your-context-root>/testdb
```

If your project artifactId is jsp-shop, context root usually /jsp-shop → http://localhost:8080/jsp-shop/testdb.
