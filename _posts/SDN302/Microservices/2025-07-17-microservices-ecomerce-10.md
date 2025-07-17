---
title: 'Epoch 10: Redis Caching for Product Service'
description: >-
  In this epoch, students will improve performance of the product-service by caching frequently requested data using Redis. This ensures faster response times for GET requests (e.g., product listings) and reduces database load.
author: [shandy]
date: 2025-07-17
updateDate: 2025-07-17
categories: [(ExpressJS) Server-Side development, (MicroServices) E-Commerce]
tags: [(MicroServices) E-Commerce]
sort_index: 911
# pin: true
# media_subpath: '/posts/02'
---

## Objectives
- Understand the role of caching in microservices.
- Use Redis to store and retrieve product data.
- Implement cache-checking logic before querying MongoDB.
- Invalidate or update the cache after product updates/deletes.
- Connect Redis container via Docker Compose.

## 1. Add Redis to Docker Compose
Update your root docker-compose.yml to include a Redis container:
```yaml
services:
  redis:
    image: redis:7-alpine
    container_name: redis
    ports:
      - "6379:6379"
```
Make sure product-service depends on it:
```yaml
product-service:
  build: ./product-service
  ports:
    - "3002:3002"
  environment:
    - MONGO_URL=mongodb://mongo:27017/productdb
    - REDIS_URL=redis://redis:6379
  depends_on:
    - mongo
    - redis
```

## 2. Install Redis Client
Inside product-service:
```bash
npm install ioredis
```

## 3. Configure Redis Connection
Inside product-service/src/config/redis.js:

```js
const Redis = require('ioredis');
const redis = new Redis(process.env.REDIS_URL);
module.exports = redis;
```
> Load REDIS_URL=redis://redis:6379 from .env.

## 4. Implement Caching in Product Controller
Here’s how to cache GET /products:

```js
const redis = require('../config/redis');
const Product = require('../models/Product');

// GET /products
const getAllProducts = async (req, res) => {
  try {
    // Check cache first
    const cached = await redis.get('products');
    if (cached) {
      return res.status(200).json(JSON.parse(cached));
    }

    // If not cached, query DB
    const products = await Product.find();

    // Cache result for 1 hour
    await redis.set('products', JSON.stringify(products), 'EX', 3600);

    res.status(200).json(products);
  } catch (err) {
    res.status(500).json({ message: 'Server error' });
  }
};
```

## 5. Invalidate Cache on Update/Delete
Update or delete actions must clear or refresh the cache:

```js
// After update/delete:
await redis.del('products');
```
Or, if updating a specific product:

```js
await redis.del(`product:${productId}`);
```

## Cache Individual Products (Optional)
For GET /products/:id, cache per product:
```js
const cached = await redis.get(`product:${id}`);
if (cached) return res.json(JSON.parse(cached));

const product = await Product.findById(id);
await redis.set(`product:${id}`, JSON.stringify(product), 'EX', 3600);
```