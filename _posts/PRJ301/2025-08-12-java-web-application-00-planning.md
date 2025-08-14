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
- JDK: 1.8_231 (Java SE 8)
- IDE: NetBeans 13
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
| **7**  | **JDBC Connection Test**                         | Connect to SQL Server database and test connectivity via a simple JSP page. Output query results to confirm setup.                                                                                          |
| **8**  | **Login with Database (JSP Model 1)**            | Modify login form to validate credentials from the `users` table using JDBC queries.                                                                                                                        |
| **9**  | **CRUD for Products (JSP Model 1)**              | Create JSP pages for listing, adding, editing, and deleting products using JDBC directly from JSP pages.                                                                                                    |
| **10** | **Filter Implementation (Authentication)**       | Implement a servlet filter to restrict access to certain JSP pages unless logged in.                                                                                                                        |
| **11** | **JSTL & Expression Language (EL)**              | Replace Java scriptlets in JSP pages with JSTL tags and EL expressions for cleaner code.                                                                                                                    |
| **12** | **Shopping Cart (Session-based)**                | Create a simple cart stored in session, allowing users to add and remove products.                                                                                                                          |
| **13** | **Order Placement & Confirmation (JSP Model 1)** | Implement order form submission, save data to DB, and display confirmation.                                                                                                                                 |
| **14** | **JSP Model 1 Review & Refactor**                | Identify limitations of Model 1 (business logic inside JSP) and prepare for migration to MVC.                                                                                                               |


## Migration to MVC

| Epoch  | Title                                    | Description                                                                                                                  |
| ------ | ---------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| **15** | **Introduction to MVC in JSP/Servlets**  | Explain MVC structure and its benefits over Model 1.                                                                         |
| **16** | **Project Restructuring to MVC**         | Create `model`, `view`, `controller` packages. Move DB logic to DAO classes, servlets as controllers, and JSP as pure views. |
| **17** | **Login/Logout in MVC**                  | Reimplement login/logout flow using MVC pattern with DAO and servlet-based validation.                                       |
| **18** | **Product CRUD in MVC**                  | Move product management from JSP scriptlets to MVC structure.                                                                |
| **19** | **Shopping Cart in MVC**                 | Store cart data in session via servlet controllers, display with JSP views.                                                  |
| **20** | **Order Management in MVC**              | Handle order creation, database insertion, and order history retrieval.                                                      |
| **21** | **Error Handling & Validation**          | Add server-side validation and error pages (404, 500).                                                                       |
| **22** | **Final UI Polishing with Bootstrap**    | Improve layout, responsive design, and UI consistency.                                                                       |
| **23** | **Deployment & Testing**                 | Deploy on Tomcat, test all functionalities, and fix bugs.                                                                    |
| **24** | **Project Presentation & Documentation** | Create project report, class diagrams, and present features.                                                                 |
