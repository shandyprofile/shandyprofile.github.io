---
title: 'Modern e-Commerce Web Platform'
description: >-
  An online marketplace where users can register, browse products, add items to their cart, and complete purchases. Admins can manage products, orders, and users. OAuth (Google) login support and secure authentication with JWT and cookies. Built using Express.js and later refactored to NestJS for maintainability and scalability.
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-10
categories: [(ExpressJS) Server-Side development, (ExpressJS) E-Commerce]
tags: [(ExpressJS) E-Commerce]
sort_index: 701
# pin: true
# media_subpath: '/posts/02'
---

## Overview
Building an e-Commerce Web Application using the following technologies:
- Express JS + Express Generator
- Mongoose + Population
- REST API
- JWT, Cookies
- Handlebars
- CORS
- OAuth + User Authentication
- NestJS
- Backend as a Service (BaaS) (e.g., Firebase, Supabase)

##  e-Commerce Web App - Development Plan (12 Episodes)

| Episode | Title                                 | Description                                                                 | Tech Focus                          |
| ------- | ------------------------------------- | --------------------------------------------------------------------------- | ----------------------------------- |
| 1       | **Project Initialization & Setup**    | Scaffold project using Express Generator, set up basic structure and CORS.  | Express JS, Express Generator, CORS |
| 2       | **Product Model & MongoDB Setup**     | Define Product schema, connect MongoDB, and build REST APIs for products.   | Mongoose, REST API                  |
| 3       | **User Auth: Register, Login & JWT**  | Create user auth with hashed passwords, JWT, cookies, and middleware.       | JWT, Cookies, Bcrypt                |
| 4       | **Shopping Cart System**              | Build user cart with product population, add/remove functionality.          | Mongoose Population, Auth           |
| 5       | **Order Model & Checkout Flow**       | Checkout process, order schema, update product stock, list user orders.     | Mongoose, REST API                  |
| 6       | **Admin Panel & Authorization**       | Role-based access, product and order management by admin users.             | Role-Based Auth, Middleware         |
| 7       | **Views with Handlebars**             | Build UI with server-side rendering using Handlebars templates.             | Handlebars, Express                 |
| 8       | **OAuth Integration (Google Login)**  | Login with Google using Passport, sync with user auth system.               | OAuth 2.0, Passport.js              |
| 9       | **Secure Auth with JWT + Cookies**    | Harden auth security, enable HTTP-only cookies, add CSRF protection.        | Secure Cookies, JWT                 |
| 10      | **BaaS Integration (e.g., Firebase)** | Use Firebase/Supabase for image uploads, analytics, or hosting.             | Firebase/Supabase                   |
| 11      | **Migration to NestJS**               | Refactor app to NestJS for modular structure, scalability, maintainability. | NestJS, Dependency Injection        |
| 12      | **Deployment & Final Polish**         | Deploy app, configure env variables, optimize performance & security.       | Deployment, Security, Hosting       |

## Technologies & Libraries Used

| Category                | Technology / Library         | Description                                                     |
| ----------------------- | ---------------------------- | --------------------------------------------------------------- |
| **Server Framework**    | **Express.js**               | Lightweight web framework for Node.js                           |
|                         | **Express Generator**        | CLI tool to scaffold Express projects                           |
|                         | **NestJS**                   | Modular, scalable Node.js framework for enterprise apps         |
| **Database**            | **MongoDB**                  | NoSQL document database                                         |
|                         | **Mongoose**                 | ODM for MongoDB in Node.js                                      |
|                         | **Mongoose Population**      | Method to auto-populate related documents (e.g., cart products) |
| **Authentication**      | **JWT (jsonwebtoken)**       | JSON Web Token for user sessions                                |
|                         | **cookie-parser**            | Parse and manage cookies on the server                          |
|                         | **bcrypt**                   | Password hashing for secure storage                             |
|                         | **passport.js**              | Middleware for OAuth strategies                                 |
|                         | **passport-google-oauth20**  | Google OAuth 2.0 strategy for Passport                          |
| **Frontend (SSR)**      | **Handlebars (hbs)**         | Templating engine for Express views                             |
| **API Architecture**    | **REST API**                 | Standard API design pattern using HTTP verbs                    |
| **Security & Utility**  | **CORS**                     | Enable Cross-Origin Resource Sharing                            |
|                         | **helmet**                   | Secure Express apps by setting HTTP headers                     |
|                         | **csurf**                    | CSRF protection middleware                                      |
| **File & Cloud**        | **Firebase / Supabase**      | BaaS for file storage, authentication, analytics                |
| **Deployment**          | **Render / Heroku / Vercel** | Cloud platforms to deploy backend                               |
|                         | **dotenv**                   | Load environment variables from `.env` file                     |
|                         | **PM2** (optional)           | Process manager for Node.js in production                       |
| **Testing & Debugging** | **Postman**                  | API testing tool                                                |
|                         | **nodemon**                  | Live-reloading server for development                           |
