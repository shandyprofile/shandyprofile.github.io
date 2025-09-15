---
title: 'Episode 6: Admin Panel & Authorization'
description: >-
  Create Login page, use JSON-Server `/users`, manage login with localStorage
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-18
categories: [(ExpressJS) Server-Side development, (ExpressJS) E-Commerce]
tags: [(ExpressJS) E-Commerce]
sort_index: 707
# pin: true
# media_subpath: '/posts/02'
---

## 1. Add Role-Based Middleware
Create a middleware file middleware/admin.js:

```js
function adminOnly(req, res, next) {
  if (req.user.role !== 'admin') {
    return res.status(403).json({ msg: 'Admin access required' });
  }
  next();
}

module.exports = adminOnly;
```
> Use this together with your auth middleware.

## 2. Extend Product Routes for Admin
Update routes/products.js:

```js
const auth = require('../middleware/auth');
const adminOnly = require('../middleware/admin');

// Admin: Create product
router.post('/', auth, adminOnly, async (req, res) => {
  const product = new Product(req.body);
  await product.save();
  res.status(201).json(product);
});

// Admin: Update product
router.put('/:id', auth, adminOnly, async (req, res) => {
  const updated = await Product.findByIdAndUpdate(req.params.id, req.body, { new: true });
  res.json(updated);
});

// Admin: Delete product
router.delete('/:id', auth, adminOnly, async (req, res) => {
  await Product.findByIdAndDelete(req.params.id);
  res.json({ msg: 'Product deleted' });
});
```
## 3. Create Admin Order Routes
Create routes/admin.js:

```js
const express = require('express');
const router = express.Router();
const auth = require('../middleware/auth');
const adminOnly = require('../middleware/admin');
const Order = require('../models/Order');

// Get all orders
router.get('/orders', auth, adminOnly, async (req, res) => {
  const orders = await Order.find().populate('user').populate('items.product');
  res.json(orders);
});

// Update order status
router.put('/orders/:id', auth, adminOnly, async (req, res) => {
  const order = await Order.findByIdAndUpdate(
    req.params.id,
    { status: req.body.status },
    { new: true }
  ).populate('items.product');

  res.json(order);
});
```
## 4. Register Admin Routes in app.js
```js
const adminRoutes = require('./routes/admin');
app.use('/api/admin', adminRoutes);
```
## 5. Create Admin User Manually (optional)
In MongoDB or in a script, set a user’s role to "admin":

```js
db.users.updateOne(
  { email: "admin@example.com" },
  { $set: { role: "admin" } }
)
```
## 6. Test With Postman
- Create Product: POST /api/products
- Update Product: PUT /api/products/:id
- Delete Product: DELETE /api/products/:id
- View All Orders: GET /api/admin/orders
- Update Order Status: PUT /api/admin/orders/:id

> Remember to login as an admin to get the correct JWT cookie.

> **Notes**
>- This is a backend-only panel — a real UI dashboard can be built later using React + Admin Template.
>- Use status values like "processing", "shipped", and "delivered" to update orders.