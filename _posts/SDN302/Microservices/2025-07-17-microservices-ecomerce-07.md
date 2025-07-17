---
title: 'Epoch 7: JWT Authentication Across Services'
description: >-
  This epoch ensures secure communication across your microservices using JWT (JSON Web Tokens). </br>
  Extract the JWT from incoming requests, </br>
  Create a shared verifyToken middleware, </br>
  Protect routes in each service, </br>
  And enable role-based access (e.g., admin vs. user).
author: [shandy]
date: 2025-07-17
updateDate: 2025-07-17
categories: [(ExpressJS) Server-Side development, (MicroServices) E-Commerce]
tags: [(MicroServices) E-Commerce]
sort_index: 908
# pin: true
# media_subpath: '/posts/02'
---

##  Objectives
- Create and verify JWT tokens securely.
- Share authentication logic across services.
- Apply middleware to secure endpoints.
- Pass authenticated requests through the API Gateway later.

## 1. What’s the Problem?
Each service (e.g., Product, Order) must verify the user’s identity — but we don’t want each one to manage login or passwords.

Solution: The user-service issues a JWT token, and other services verify it before processing requests.

## 2. Common JWT Middleware (Reusable)
In middleware/auth.js
```js
import jwt from 'jsonwebtoken';

export function verifyToken(req, res, next) {
  const token = req.headers.authorization?.split(" ")[1];

  if (!token) return res.status(401).json({ error: 'No token provided' });

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (err) {
    return res.status(403).json({ error: 'Invalid token' });
  }
}
```

> Add JWT_SECRET to .env of each service.

## 3. Protect Routes
Example in Product Service:
```js
import { verifyToken } from './middleware/auth.js';

app.post('/products', verifyToken, async (req, res) => {
  // Only authenticated users can create products
});
```

## 4. Add Role-Based Access (Optional)
Add this to the middleware:
```js
export function isAdmin(req, res, next) {
  if (req.user.role !== 'admin') {
    return res.status(403).json({ error: 'Admins only' });
  }
  next();
}
```
Apply like:
```
app.delete('/products/:id', verifyToken, isAdmin, async (req, res) => {
  // Only admin can delete products
});
```

## 5. Test Authentication Across Services
Use Postman or frontend to:
1. Login → receive JWT from user-service
2. Send request to product/order-service with:

```html
Authorization: Bearer <JWT>
```