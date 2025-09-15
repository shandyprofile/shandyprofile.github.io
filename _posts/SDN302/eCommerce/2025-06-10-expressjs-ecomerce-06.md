---
title: 'Episode 5: Order Model & Checkout Flow'
description: >-
  Implement a system for users to place orders based on their cart. Store order history and support retrieving past orders.
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-18
categories: [(ExpressJS) Server-Side development, (ExpressJS) E-Commerce]
tags: [(ExpressJS) E-Commerce]
sort_index: 706
# pin: true
# media_subpath: '/posts/02'
---

## 1. Create Order Schema
Create a new file models/Order.js:

```js
const mongoose = require('mongoose');

const OrderSchema = new mongoose.Schema({
  user: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User',
    required: true
  },
  items: [
    {
      product: {
        type: mongoose.Schema.Types.ObjectId,
        ref: 'Product'
      },
      quantity: Number
    }
  ],
  total: {
    type: Number,
    required: true
  },
  status: {
    type: String,
    enum: ['pending', 'processing', 'shipped', 'delivered'],
    default: 'pending'
  },
  createdAt: {
    type: Date,
    default: Date.now
  }
});

module.exports = mongoose.model('Order', OrderSchema);
```
## 2. Create Order Routes
Create routes/orders.js:

```js
const express = require('express');
const router = express.Router();
const auth = require('../middleware/auth');
const Order = require('../models/Order');
const User = require('../models/User');

// Checkout (Place order)
router.post('/checkout', auth, async (req, res) => {
  const user = await User.findById(req.user.id).populate('cart.product');

  if (!user.cart.length) {
    return res.status(400).json({ msg: 'Cart is empty' });
  }

  const items = user.cart.map(item => ({
    product: item.product._id,
    quantity: item.quantity
  }));

  const total = user.cart.reduce(
    (sum, item) => sum + item.product.price * item.quantity,
    0
  );

  const order = new Order({
    user: user._id,
    items,
    total
  });

  await order.save();

  // Clear user's cart
  user.cart = [];
  await user.save();

  res.status(201).json({ msg: 'Order placed successfully', order });
});

// Get user's order history
router.get('/my-orders', auth, async (req, res) => {
  const orders = await Order.find({ user: req.user.id }).populate('items.product');
  res.json(orders);
});
```
## 3. Register Order Routes in app.js
```js
const orderRoutes = require('./routes/orders');
app.use('/api/orders', orderRoutes);
```
## 4. Test With Postman
- POST `/api/orders/checkout` => places order from cart
- GET `/api/orders/my-orders` => view past orders

> Make sure you're logged in with a valid JWT cookie.

> **Notes**
> - You could later add payment logic (Stripe, PayPal) here.
> - Admin routes for updating order status can be added later in Episode 11.