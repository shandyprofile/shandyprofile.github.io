---
title: 'Episode 4: Shopping Cart System'
description: >-
  Enable each user to add, remove, and view products in their shopping cart. Use Mongoose’s population feature to retrieve full product details.
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-18
categories: [(ExpressJS) Server-Side development, (ExpressJS) E-Commerce]
tags: [(ExpressJS) E-Commerce]
sort_index: 705
# pin: true
# media_subpath: '/posts/02'
---

## 1. Update User Schema to Include Cart
In models/User.js:

```js
const mongoose = require('mongoose');

const UserSchema = new mongoose.Schema({
  email: String,
  password: String,
  role: { type: String, default: 'user' },
  cart: [
    {
      product: {
        type: mongoose.Schema.Types.ObjectId,
        ref: 'Product'
      },
      quantity: {
        type: Number,
        default: 1
      }
    }
  ]
}, { timestamps: true });

module.exports = mongoose.model('User', UserSchema);
```
## 2. Create Cart Routes
In a new file routes/cart.js:
```js
const express = require('express');
const router = express.Router();
const auth = require('../middleware/auth');
const User = require('../models/User');
const Product = require('../models/Product');

// Get cart (with product details)
router.get('/', auth, async (req, res) => {
  const user = await User.findById(req.user.id).populate('cart.product');
  res.json(user.cart);
});

// Add to cart
router.post('/add', auth, async (req, res) => {
  const { productId, quantity } = req.body;
  const user = await User.findById(req.user.id);

  const existingItem = user.cart.find(item => item.product.toString() === productId);

  if (existingItem) {
    existingItem.quantity += quantity;
  } else {
    user.cart.push({ product: productId, quantity });
  }

  await user.save();
  res.json({ msg: 'Product added to cart' });
});

// Remove from cart
router.post('/remove', auth, async (req, res) => {
  const { productId } = req.body;
  const user = await User.findById(req.user.id);

  user.cart = user.cart.filter(item => item.product.toString() !== productId);
  await user.save();
  res.json({ msg: 'Product removed from cart' });
});
```
## 3. Register Cart Routes in app.js
```js
const cartRoutes = require('./routes/cart');
app.use('/api/cart', cartRoutes);
```
## 4. Test With Postman
- GET `/api/cart` => returns user’s cart with product info
- POST `/api/cart/add`

```json
{
  "productId": "PRODUCT_ID",
  "quantity": 2
}
```

- POST `/api/cart/remove`

```json
{
  "productId": "PRODUCT_ID"
}
```

> You must be logged in and have the JWT cookie for these endpoints to work.

> **Notes**
> - We use populate(`cart.product`) to include full product details in the response.
> - Cart is stored inside the User document for simplicity. For high-scale systems, consider a separate Cart collection.