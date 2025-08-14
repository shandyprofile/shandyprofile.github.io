---
title: 'Epoch 9: Dockerize All Services'
description: >-
  In this epoch, students will containerize each microservice using Docker and connect them using Docker Compose. This will simulate a real-world deployment environment, allowing services to communicate over a virtual network and be easily started or stopped with a single command.
author: [shandy]
date: 2025-07-17
updateDate: 2025-07-17
categories: [(ExpressJS) Server-Side development, (MicroServices) E-Commerce]
tags: [(MicroServices) E-Commerce]
sort_index: 910
# pin: true
# media_subpath: '/posts/02'
---

## Objectives
- Understand how Docker works for packaging NodeJS apps.
- Create Dockerfiles for each microservice (User, Product, Order, Notification, API Gateway).
- Set up a docker-compose.yml file to orchestrate all containers.
- Enable inter-service communication using Docker Compose networking.
- Test that services work seamlessly in a containerized environment.

## 1. Create Dockerfiles
Create a Dockerfile in each service directory (e.g., user-service/, product-service/, etc.).
```dockerfile
# Base image
FROM node:18

# Working directory
WORKDIR /app

# Copy files
COPY . .

# Install dependencies
RUN npm install

# Expose port
EXPOSE 3001  # Change this per service

# Run the service
CMD ["npm", "run", "dev"]
```

>  Make sure each service’s package.json has a "dev" script (e.g., node src/index.js or using nodemon).

## 2. Update docker-compose.yml
At the root of the project, create a docker-compose.yml file to define and run all services:
```yaml
version: "3.9"

services:
  user-service:
    build: ./user-service
    ports:
      - "3001:3001"
    environment:
      - MONGO_URL=mongodb://mongo:27017/userdb
    depends_on:
      - mongo

  product-service:
    build: ./product-service
    ports:
      - "3002:3002"
    environment:
      - MONGO_URL=mongodb://mongo:27017/productdb
    depends_on:
      - mongo

  order-service:
    build: ./order-service
    ports:
      - "3003:3003"
    environment:
      - MONGO_URL=mongodb://mongo:27017/orderdb
    depends_on:
      - mongo

  notification-service:
    build: ./notification-service
    ports:
      - "3004:3004"

  api-gateway:
    build: ./api-gateway
    ports:
      - "3000:3000"
    depends_on:
      - user-service
      - product-service
      - order-service
      - notification-service

  mongo:
    image: mongo:6.0
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:
```

> Note: Use environment variables like MONGO_URL inside each service’s code (.env) to match the Compose setup.

## 3. Update Service .env Files
Each service should have an .env like:
```
PORT=3001
MONGO_URL=mongodb://mongo:27017/userdb
JWT_SECRET=your_jwt_secret
```

Ensure each service listens to 0.0.0.0, not localhost, to allow Docker network access:
```js
app.listen(process.env.PORT || 3001, '0.0.0.0');
```

## 4. Run All Services Together
From the project root:
```bash
docker-compose up --build
```
> Use --build to rebuild images if the code changes.

To stop:
```bash
docker-compose down
```

## 5. Verify Everything Works
- Access API Gateway at http://localhost:3000
- Call GET /users, /products, /orders, etc.
- Check logs in each terminal or use:

```bash
docker-compose logs -f user-service
```

## 6. Docker Folder Structure

```
ecommerce-microservices/
├── docker-compose.yml
├── .env.example
├── user-service/
│   ├── Dockerfile
│   ├── package.json
│   ├── .env
│   └── src/
├── product-service/
│   ├── Dockerfile
│   ├── package.json
│   ├── .env
│   └── src/
├── order-service/
│   ├── Dockerfile
│   ├── package.json
│   ├── .env
│   └── src/
├── notification-service/
│   ├── Dockerfile
│   ├── package.json
│   └── src/
├── api-gateway/
│   ├── Dockerfile
│   ├── package.json
│   └── src/
└── mongo-data/ (optional volume mapping if not using Docker volume)
```

![alt text](/assets/img/SDN302/docker-folder-structure.png)

### 6.1 Each Folder Contains

| Folder                  | Purpose                                                         |
| ----------------------- | --------------------------------------------------------------- |
| `docker-compose.yml`    | Central file to spin up all services and MongoDB together       |
| `user-service/`         | ExpressJS API for users – has own Dockerfile & Mongo connection |
| `product-service/`      | Product CRUD service, ready for Redis caching (Epoch 10)        |
| `order-service/`        | Handles order creation and coordination with users/products     |
| `notification-service/` | Lightweight HonoJS logger or alert receiver                     |
| `api-gateway/`          | Central routing layer for external requests                     |
| `mongo-data/`           | (Optional) Persistent volume if not using Docker volume         |

### 6.2 Sample Dockerfile Recap (for Express-based services)

```dockerfile
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3001  # Replace per service
CMD ["npm", "run", "dev"]
```
