---
title: 'Microservices with NodeJS'
description: >-
  Building an E-commerce Backend System using a Microservices Architecture.
author: [shandy]
date: 2025-07-17
updateDate: 2025-07-17
categories: [(ExpressJS) Server-Side development, (MicroServices) E-Commerce]
tags: [(MicroServices) E-Commerce]
sort_index: 901
# pin: true
# media_subpath: '/posts/02'
---

## Overview
This project guides students through building an E-commerce Backend System using a Microservices Architecture. Each service is developed independently with its own responsibilities, database, and codebase. The project introduces essential backend development practices such as authentication, inter-service communication, event-driven design, containerization, and API Gateway patterns.

By the end of the project, students will have hands-on experience in building, running, and deploying scalable microservices using modern tools and NodeJS-based frameworks.

##  e-Commerce Web App - Development Plan (12 Episodes)

| Epoch  | Topic                                                                                                      | Technologies & Concepts                          | Deliverables / Output                                   |
| ------ | ---------------------------------------------------------------------------------------------------------- | ------------------------------------------------ | ------------------------------------------------------- |
| **1**  | **Introduction to Microservices & Architecture**<br>Monolith vs Microservices, service boundaries          | Microservice Architecture, Service Decomposition | System architecture diagram, service definitions        |
| **2**  | **Build the User Service (ExpressJS)**<br>Registration, login, hashing, JWT generation, MongoDB connection | ExpressJS, Mongoose, JWT, bcrypt                 | Functional `user-service` with authentication API       |
| **3**  | **Build the Product Service (ExpressJS)**<br>CRUD products, input validation, clean folder structure       | ExpressJS, MongoDB, Joi                          | `product-service` with independent CRUD API             |
| **4**  | **Build the Order Service (ExpressJS)**<br>Save order documents, use product & user IDs internally         | ExpressJS, MongoDB                               | `order-service` with basic order creation functionality |
| **5**  | **Notification Service using HonoJS**<br>Minimalistic logging or alert service                             | HonoJS, REST API                                 | `notification-service` for async logs/notifications     |
| **6**  | **Event-Driven Architecture with RabbitMQ**<br>Publish/Subscribe model, message queue communication        | RabbitMQ, amqplib                                | Services publish and consume events asynchronously      |
| **7**  | **JWT Authentication Across Services**<br>Token verification, middleware, role-based access control        | JWT, Express Middleware                          | All protected routes verify JWT correctly               |
| **8**  | **API Gateway (Custom-built)**<br>Central routing, token checking, CORS handling                           | ExpressJS, Gateway Pattern                       | A working `api-gateway` routing requests to services    |
| **9**  | **Dockerize All Services**<br>Dockerfile per service, docker-compose, internal networking                  | Docker, Docker Compose                           | All services containerized and connected in one system  |
| **10** | **Redis Caching for Product Service**<br>GET request caching, cache invalidation on update/delete          | Redis, ioredis                                   | Cached product data with improved response time         |
| **11** | **Monitoring, Logging & CI/CD (Basic)**<br>Logging with Winston, health checks, CI/CD via GitHub Actions   | Winston, Morgan, Healthcheck, GitHub Actions     | System logs + automated test/deploy pipeline            |

## Technologies & Libraries Used

| Category                 | Tools & Libraries                       |
| ------------------------ | --------------------------------------- |
| **Languages**            | JavaScript (ES6+), NodeJS               |
| **Frameworks**           | ExpressJS, HonoJS                       |
| **Databases**            | MongoDB (via Mongoose), Redis (caching) |
| **Authentication**       | JSON Web Tokens (JWT), bcrypt           |
| **Message Broker**       | RabbitMQ, amqplib                       |
| **API Gateway**          | Custom Express Gateway                  |
| **Containerization**     | Docker, Docker Compose                  |
| **Logging & Monitor**    | Winston, Morgan, Healthcheck Routes     |
| **CI/CD**                | GitHub Actions                          |
| **Testing** *(Optional)* | Postman, Thunder Client                 |


## E-commerce Microservices Architecture Diagram
![alt text](/assets/img/SDN302/architecture-diagram.png)