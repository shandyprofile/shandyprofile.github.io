---
title: Java Web Application - Planning by Project-base learning
description: >-
  Java Web Application for (PRJ301)
author: [shandy]
date: 2025-08-12
categories: [(Java) Web Application, (PBL) Shopping Web]
tags: [(Java - PBL) Shopping Web]
sort_index: 100
# pin: true
# media_subpath: '/posts/01'
---
## Project Description
This Project-Based Learning (PBL) plan is designed for students to develop a Sales Website using Java JSP (Jakarta EE) with Maven, running on Tomcat, and styled with Bootstrap. The project follows a step-by-step, hands-on approach over 17 epoches, guiding students from environment setup to deployment.

The primary objectives are:
- Equip students with practical experience in developing a complete JSP web application.
- Enable students to configure and integrate a Jakarta EE backend with SQL Server for database operations.
- Practice core JSP features including JSTL, Expression Language (EL), filters, and CRUD operations.
- Learn how to customize project templates in NetBeans for better UI/UX and maintainable code structure.
- Introduce professional development workflows: Maven build automation, server configuration, and database connectivity.

The project emphasizes practical coding rather than theory, with each epoch covering a specific milestone. Students will:
- Set up the required development tools and frameworks.
- Implement authentication (Login/Logout) using Jakarta Servlet and JSP.
- Create responsive pages with Bootstrap styling.
- Perform CRUD operations linked to SQL Server database.
- Implement filters for request handling and security.
- Apply JSTL and EL for dynamic content rendering.
- Deploy the application on Tomcat server with Maven build.

The course is divided into three main phases:
1. Foundation Setup – Installing and configuring all tools, creating the base project, ensuring database connectivity.
2. Core Features Implementation – Building the sales website with login, CRUD, filters, and UI.
3. Enhancement & Deployment – Styling, refining code structure, testing, and deployment.

## Project Requirements
### 1. Environment
- Operating System: Windows 10 or later
- JDK: Java SE 24
- IDE: NetBeans 25
- Web Server: Apache Tomcat 10.0.23
- Database: SQL Server 2019 or later
- Build Tool: Maven (Maven Web Application model)
- Package Namespace: Use jakarta.* instead of javax.*
- (Option) Version Control: Git (GitHub/Bitbucket)

### 2. Libraries & Frameworks
- Jakarta Servlet & JSP API
- Bootstrap for styling (CSS/JS)
- JSTL (Jakarta Standard Tag Library)
- Expression Language (EL) for dynamic data binding
- JDBC for database connectivity

### 3. Project Scope

Web-based sales application with:
- Login form
- CRUD operations
- Filtering feature
- Bootstrap UI
- JSTL & EL usage
- Maven project structure
- SQL Server database integration

## Jakarta JSP Maven Web App


| Epoch  | Title                                            | Description                                                                                                                                                                                                 |
| ------ | ------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1**  | **Project Introduction & Environment Setup**     | Overview of project objectives (E-Commerce Web App), required skills, and methodology. Install and configure **JDK 1.8\_231**, **NetBeans 13**, **Tomcat 10.0.23** with **Maven Web Application** template. |
| **2**  | **Switching NetBeans Maven Template to Jakarta** | Demonstrate replacing `javax.*` imports with `jakarta.*` equivalents in the NetBeans Maven Web Application template (Tools → Templates → Web → Servlets). Verify compatibility with Tomcat 10.              |
| **3**  | **SQL Server Configuration**                     | Enable TCP/IP protocol, set port (1433), configure mixed authentication mode, enable SA account, and test connection with SSMS.                                                                             |
| **4**  | **JDBC Driver Integration & DB Setup**           | Add SQL Server JDBC driver dependency to `pom.xml`. Create SQL Server database for the project. Set up sample tables (`users`, `products`, `orders`).                                                       |
| **5**  | **Bootstrap Integration**                        | Add Bootstrap via CDN in JSP pages. Apply a base layout to improve UI styling.                                                                                                                              |
| **6**  | **Hardcoded Login/Logout (Session or Cookie)**   | Create a login form (HTML + JSP). Validate user credentials using hardcoded values. Implement session or cookie-based login/logout. Show/hide content based on login status.                                |
| **7**  | **Login with Database (JSP Model 1)**            | Modify login form to validate credentials from the `users` table using JDBC queries.                                                                                                                                                                                                                         |

## Migration to MVC (JSP model 2)

| Epoch  | Title                                    | Description                                                                                                                  |
| ------ | ---------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| **8** | **Introduction to MVC in JSP/Servlets**  | Explain MVC structure and its benefits over Model 1.
| **9** | **Project Restructuring to MVC**         | Create `model`, `view`, `controller` packages. Move DB logic to DAO classes, servlets as controllers, and JSP as pure views. |
| **10** | **Validation**                  | Reimplement logi validation.   

| **11** | **JSTL & Expression Language (EL)**              | Replace Java scriptlets in JSP pages with JSTL tags and EL expressions for cleaner code.                                                                                                                    |
| **12** | **Product CRUD in MVC**                  | Move product management from JSP scriptlets to MVC structure.                                                                |
| **13** | **Shopping Cart in MVC**                 | Store cart data in session via servlet controllers, display with JSP views.                                                  |
| **14** | **Order Management in MVC**              | Handle order creation, database insertion, and order history retrieval.                                                      |
| **15** | **Error Handling & Validation**          | Add server-side validation and error pages (404, 500).                                                                       |
| **16** | **Filter Implementation (Authentication)**       | Implement a servlet filter to restrict access to certain JSP pages unless logged in.                                                                                                                        |