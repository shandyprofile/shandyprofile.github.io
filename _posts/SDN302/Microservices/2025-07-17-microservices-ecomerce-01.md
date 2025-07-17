---
title: 'Epoch 1: Introduction to Microservices'
description: >-
  This first epoch introduces the core concepts of Microservice Architecture, providing students with a solid theoretical foundation before diving into code. Learners will explore how microservices differ from monolithic systems, understand why many modern applications use them, and design the architecture for a real-world e-commerce platform that will serve as the basis for the entire course.
author: [shandy]
date: 2025-07-17
updateDate: 2025-07-17
categories: [(ExpressJS) Server-Side development, (MicroServices) E-Commerce]
tags: [(MicroServices) E-Commerce]
sort_index: 902
# pin: true
# media_subpath: '/posts/02'
---

##  Objectives

- Define microservices and explain how they differ from monolithic architectures.
- Identify the benefits and trade-offs of using microservices.
- Understand the core components of a microservices system.
- Design a basic microservices architecture for an e-commerce platform.
- Prepare their local development environment for microservices development.

## 1. Monolithic and Microservice
### 1.1. Monolithic Architecture
A monolithic application is built as a single, tightly-coupled unit. All logic (user interface, business logic, data access) exists in one codebase and is typically deployed as a single executable.

Characteristics:
- Single codebase and deployment unit
- Shared database
- Easy to develop at the beginning
- Becomes harder to scale and maintain as it grows

### 1.2. Microservice Architecture
A microservices system is composed of multiple small, autonomous services. Each service is responsible for a specific business capability and communicates with others over a network (usually HTTP or messaging queues).

Characteristics:
- Independent services, each with its own codebase and database
- Services can be developed, deployed, and scaled independently
- Services communicate via REST APIs or messaging queues (e.g., RabbitMQ, Kafka)

Benefits:
- Scalability
- Technology flexibility (different services can use different tech stacks)
- Better fault isolation
- Easier for large teams to work in parallel

Challenges:
- Higher complexity in deployment and orchestration
- Requires good DevOps and monitoring
- Inter-service communication can be tricky

## 2. Target Architecture: E-commerce System
We will build a simplified E-commerce platform with the following microservices:

| Service Name           | Responsibilities                            | Technology       |
| ---------------------- | ------------------------------------------- | ---------------- |
| `user-service`         | User registration, login, authentication    | Node.js, Express |
| `product-service`      | Product catalog management (CRUD)           | Node.js, Express |
| `order-service`        | Handles order creation, status, total price | Node.js, Express |
| `notification-service` | Sends notifications or logs events          | Node.js, Hono    |
| `api-gateway`          | Centralized routing and authentication      | Node.js, Express |

Each service will have its own:
- Database (MongoDB for most services)
- REST API
- Dockerfile for containerization

## 3. System Design: Architecture Diagram
- The API Gateway handles all incoming requests from clients and forwards them to the appropriate service.
- Each microservice handles a single responsibility and maintains its own isolated database.
- Services do not directly query each other’s databases.
- The Order Service communicates with the Notification Service using event-driven messaging (e.g., RabbitMQ), simulating an asynchronous flow.

## 4. Local Development Environment Setup

Required Tools:
- Node.js
- MongoDB (local or Atlas)
- Postman – API testing
- Docker Desktop – for containerization (used later)
- VS Code or any code editor

Folder Structure Example:
```
/ecommerce-system/
├── user-service/
├── product-service/
├── order-service/
├── notification-service/
├── api-gateway/
└── docker-compose.yml (added in later epochs)
```

## 5. Hands-on Activity: Architecture Brainstorm
Task: In pairs or small groups, break down a well-known application (e.g., Shopee, Lazada, Amazon) into microservices.

Questions to discuss:
- What are the major business functions?
- Which features would become services?
- How would the services communicate?

Example:
- `payment-service` – handles transactions
- `review-service` – manages product reviews
- `search-service` – full-text product search
- `inventory-service` – manages stock quantities

