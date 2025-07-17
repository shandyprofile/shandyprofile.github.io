---
title: 'Epoch 5: Notification Service using HonoJS'
description: >-
  In this epoch, students will build a lightweight notification-service using HonoJS, a minimal and ultra-fast web framework for building modern web APIs. This service will act as a simple receiver of logs or events from other microservices (such as order-service) — either as synchronous HTTP calls or eventually via message queues.
author: [shandy]
date: 2025-07-17
updateDate: 2025-07-17
categories: [(ExpressJS) Server-Side development, (MicroServices) E-Commerce]
tags: [(MicroServices) E-Commerce]
sort_index: 906
# pin: true
# media_subpath: '/posts/02'
---

##  Objectives
- Set up a lightweight microservice using HonoJS.
- Create REST API endpoints for receiving notification or event messages.
- Simulate event handling using HTTP POST requests.
- Prepare this service for asynchronous message queue integration in the next epoch.

## 1. Microservice Overview
- **Service Name**: notification-service
- **Technology Stack**: Node.js, Hono

## 2. Structure architechture

```
notification-service/
├── src/
│   └── index.js
├── .env
├── package.json
```

### 2.1. Project Setup
- Step 1: Initialize the Project
```bash
mkdir notification-service
cd notification-service
npm init -y
```

- Step 2: Install Dependencies
```bash
npm install hono dotenv
```

- Step 3: Project Structure: Create the following folders and files:
```bash
mkdir controllers models routes middlewares
touch server.js .env
```

### 2.2. Create the Notification API
- src/index.js

```js
import { Hono } from 'hono';
import { logger } from 'hono/logger';
import { cors } from 'hono/cors';

const app = new Hono();

// Middleware
app.use('*', logger());
app.use('*', cors());

// POST /notify
app.post('/notify', async (c) => {
  const body = await c.req.json();
  console.log('New notification received:', body);

  return c.json({ message: 'Notification logged.' }, 200);
});

export default app;
```

### 2.3. Run the Server
- Update your package.json:
```json
"scripts": {
  "dev": "node --watch src/index.js"
}
```
- Then run:
```bash
npm run dev
```
> By default, Hono runs on port 3000. You can also customize this if needed.

## 3. Testing with Postman
- Endpoint: POST http://localhost:3000/notify
- Request Body:
```json
{
  "type": "order_created",
  "message": "New order #01234 created for user ID 789",
  "timestamp": "2025-07-17T10:00:00Z"
}
```
- Response:

```json
{
  "message": "Notification logged."
}
```

- Console Output:
```bash
New notification received: {
  type: 'order_created',
  message: 'New order #01234 created for user ID 789',
  timestamp: '2025-07-17T10:00:00Z'
}
```