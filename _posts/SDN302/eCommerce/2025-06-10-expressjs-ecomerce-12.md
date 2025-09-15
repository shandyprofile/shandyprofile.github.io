---
title: 'Episode 11 – Admin Dashboard with Role-Based Access Control (RBAC)'
description: >-
  - Implement an Admin-only dashboard for managing products, users, and orders </br>
  - Create a role field in the User model (admin, user) </br>
  - Add middleware to restrict access based on roles </br>
  - Build protected admin routes and a basic dashboard UI
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-10
categories: [(ExpressJS) Server-Side development, (ExpressJS) E-Commerce]
tags: [(ExpressJS) E-Commerce]
sort_index: 712
# pin: true
# media_subpath: '/posts/02'
---

## 1. Extend the User Model with Role
Update models/User.js:

```js
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  email: String,
  password: String,
  role: {
    type: String,
    enum: ['user', 'admin'],
    default: 'user'
  }
});

module.exports = mongoose.model('User', userSchema);
```
## 2. Assign Role During Registration or in MongoDB
Option 1: Assign manually after registering:
```bash
# In MongoDB shell or Compass
db.users.updateOne({ email: "admin@example.com" }, { $set: { role: "admin" } })
```

Option 2: Modify registration logic:
```js
// Example: default role or conditionally set to admin
const user = new User({
  email,
  password: hashedPassword,
  role: email === 'admin@example.com' ? 'admin' : 'user'
});
```

## 3. Create Middleware to Check Roles
In middlewares/roleCheck.js:
```js
module.exports = function requireRole(role) {
  return function (req, res, next) {
    if (!req.user) return res.status(401).send("Not logged in");
    if (req.user.role !== role) return res.status(403).send("Forbidden");
    next();
  };
};
```

Also ensure your authMiddleware sets req.user:
```js
const jwt = require('jsonwebtoken');
const User = require('../models/User');

async function authMiddleware(req, res, next) {
  const token = req.cookies.token;
  if (!token) return res.status(401).send('No token');

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    const user = await User.findById(decoded.id);
    req.user = user;
    next();
  } catch (err) {
    res.status(403).send('Invalid token');
  }
}
```

## 4. Protect Admin Routes
In your route file:

```js
const express = require('express');
const router = express.Router();
const authMiddleware = require('../middlewares/auth');
const requireRole = require('../middlewares/roleCheck');

// Protect admin panel
router.get('/admin', authMiddleware, requireRole('admin'), (req, res) => {
  res.render('admin-dashboard', { user: req.user });
});

module.exports = router;
```

## 5. Admin Dashboard View (Handlebars Example)
views/admin-dashboard.handlebars:

```hbs
<h1>Admin Dashboard</h1>
<p>Welcome, {{user.email}}</p>

<ul>
  <li><a href="/admin/products">Manage Products</a></li>
  <li><a href="/admin/orders">View Orders</a></li>
  <li><a href="/admin/users">Manage Users</a></li>
</ul>
```
> For React: You’d fetch from protected API endpoints like /api/admin/products, using the same authMiddleware + requireRole.

## 6. Frontend: Show Admin Links Only If Admin
In Handlebars layout:

```hbs
{{#if user}}
  {{#ifEquals user.role "admin"}}
    <a href="/admin">Admin Dashboard</a>
  {{/ifEquals}}
{{/if}}
```
Create custom Handlebars helper ifEquals:

```js
hbs.registerHelper('ifEquals', function (arg1, arg2, options) {
  return (arg1 === arg2) ? options.fn(this) : options.inverse(this);
});
```