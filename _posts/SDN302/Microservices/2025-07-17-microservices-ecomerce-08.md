---
title: 'Epoch 8: API Gateway (Custom-built with ExpressJS)'
description: >-
  The API Gateway acts as the single entry point for all client requests. It routes requests to appropriate microservices, handles authentication (JWT), and can perform tasks like CORS handling, request logging, and rate limiting.
author: [shandy]
date: 2025-07-17
updateDate: 2025-07-17
categories: [(ExpressJS) Server-Side development, (MicroServices) E-Commerce]
tags: [(MicroServices) E-Commerce]
sort_index: 909
# pin: true
# media_subpath: '/posts/02'
---

##  Objectives
- Understand the API Gateway pattern.
- Build a basic gateway using ExpressJS.
- Route traffic to backend microservices.
- Forward client JWT tokens for downstream verification.
- Handle CORS and request logging centrally.

## 1. Initialize the API Gateway

```hash
mkdir api-gateway
cd api-gateway
npm init -y
npm install express http-proxy-middleware dotenv cors morgan
```

## 2. Folder Structure

```
api-gateway/
├── src/
│   └── index.js
│   └── routes/
│       └── proxyRoutes.js
├── .env
├── package.json
```

> Add JWT_SECRET to .env of each service.

## 3. Setup Environment Variables

```env
PORT=3000

USER_SERVICE_URL=http://user-service:3001
PRODUCT_SERVICE_URL=http://product-service:3002
ORDER_SERVICE_URL=http://order-service:3003
NOTIFICATION_SERVICE_URL=http://notification-service:3004
```

## 4. Proxy Configuration
In src/routes/proxyRoutes.js:
```js
import { createProxyMiddleware } from 'http-proxy-middleware';
import dotenv from 'dotenv';

dotenv.config();

export const userProxy = createProxyMiddleware('/users', {
  target: process.env.USER_SERVICE_URL,
  changeOrigin: true,
  pathRewrite: { '^/users': '' },
});

export const productProxy = createProxyMiddleware('/products', {
  target: process.env.PRODUCT_SERVICE_URL,
  changeOrigin: true,
  pathRewrite: { '^/products': '' },
});

export const orderProxy = createProxyMiddleware('/orders', {
  target: process.env.ORDER_SERVICE_URL,
  changeOrigin: true,
  pathRewrite: { '^/orders': '' },
});

export const notificationProxy = createProxyMiddleware('/notify', {
  target: process.env.NOTIFICATION_SERVICE_URL,
  changeOrigin: true,
  pathRewrite: { '^/notify': '' },
});
```

## 5. Main Server Logic
In src/index.js:

```js
import express from 'express';
import cors from 'cors';
import morgan from 'morgan';
import dotenv from 'dotenv';

import {
  userProxy,
  productProxy,
  orderProxy,
  notificationProxy,
} from './routes/proxyRoutes.js';

dotenv.config();

const app = express();
const PORT = process.env.PORT || 3000;

// Middleware
app.use(cors());
app.use(morgan('dev'));

// Proxies
app.use('/users', userProxy);
app.use('/products', productProxy);
app.use('/orders', orderProxy);
app.use('/notify', notificationProxy);

// Root
app.get('/', (req, res) => {
  res.send('API Gateway is running');
});

app.listen(PORT, () => {
  console.log(`API Gateway running at http://localhost:${PORT}`);
});
```

## 6. Docker Integration

Update docker-compose.yml:
```yaml
  api-gateway:
    build: ./api-gateway
    ports:
      - "3000:3000"
    depends_on:
      - user-service
      - product-service
      - order-service
      - notification-service
```

Add Dockerfile:
```dockerfile
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
CMD ["node", "src/index.js"]
```

## 7. Test Gateway Routing

- GET http://localhost:3000/users/me → proxies to user-service
- POST http://localhost:3000/products → proxies to product-service
- POST http://localhost:3000/notify → proxies to notification-service

## Optional: JWT Middleware at Gateway
Add token verification at the gateway like this:
- middleware/auth.js
```js
import jwt from 'jsonwebtoken';

export const gatewayAuth = (req, res, next) => {
  const token = req.headers.authorization?.split(" ")[1];
  if (!token) return res.status(401).json({ error: 'Missing token' });

  try {
    req.user = jwt.verify(token, process.env.JWT_SECRET);
    next();
  } catch {
    return res.status(403).json({ error: 'Invalid token' });
  }
};
```

Then use:
```js
app.use('/products', gatewayAuth, productProxy);
```