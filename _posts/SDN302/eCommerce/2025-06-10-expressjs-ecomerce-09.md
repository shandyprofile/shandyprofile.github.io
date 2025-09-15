---
title: 'Episode 8: Using Cookies for Cart & Auth in Handlebars Views'
description: >-
  Allow Handlebars views to all pages
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-10
categories: [(ExpressJS) Server-Side development, (ExpressJS) E-Commerce]
tags: [(ExpressJS) E-Commerce]
sort_index: 709
# pin: true
# media_subpath: '/posts/02'
---

## 1. Install and Use cookie-parser
```bash
npm install cookie-parser
```
In app.js:

```js
const cookieParser = require('cookie-parser');
app.use(cookieParser());
```
## 2. Create Middleware to Pass Auth Info to Views
Create middleware/viewContext.js:

```js
const jwt = require('jsonwebtoken');
const User = require('../models/User');

module.exports = async function viewContext(req, res, next) {
  const token = req.cookies.token;

  if (token) {
    try {
      const decoded = jwt.verify(token, process.env.JWT_SECRET);
      const user = await User.findById(decoded.id).populate('cart.product');
      res.locals.user = {
        id: user._id,
        email: user.email,
        role: user.role,
        cartCount: user.cart.reduce((sum, item) => sum + item.quantity, 0)
      };
    } catch (err) {
      res.locals.user = null;
    }
  } else {
    res.locals.user = null;
  }

  next();
};
```
Register in app.js:

```js
const viewContext = require('./middleware/viewContext');
app.use(viewContext);
```
> This makes user available in all Handlebars templates.

## 3. Update main.hbs Layout to Show Auth Info
```handlebars
<nav class="navbar navbar-expand-lg navbar-light bg-light mb-4">
  <a class="navbar-brand" href="/">MyShop</a>
  <div class="collapse navbar-collapse">
    <ul class="navbar-nav ms-auto">
      {{#if user}}
        <li class="nav-item">
          <a class="nav-link" href="/cart">Cart ({{user.cartCount}})</a>
        </li>
        <li class="nav-item">
          <span class="nav-link">Hello, {{user.email}}</span>
        </li>
        <li class="nav-item">
          <form action="/api/auth/logout" method="POST">
            <button class="btn btn-link nav-link" type="submit">Logout</button>
          </form>
        </li>
      {{else}}
        <li class="nav-item">
          <a class="nav-link" href="/login">Login</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="/register">Register</a>
        </li>
      {{/if}}
    </ul>
  </div>
</nav>
```

## 4. Render Cart Page in Views
In routes/pages.js:
```js
router.get('/cart', async (req, res) => {
  if (!res.locals.user) {
    return res.redirect('/login');
  }

  const user = await User.findById(res.locals.user.id).populate('cart.product');
  res.render('cart', {
    title: 'My Cart',
    cart: user.cart
  });
});
```

In views/cart.hbs:

```handlebars
<h2>Your Shopping Cart</h2>

{{#if cart.length}}
  <ul class="list-group mb-3">
    {{#each cart}}
      <li class="list-group-item d-flex justify-content-between">
        <div>
          <strong>{{this.product.name}}</strong> ({{this.quantity}})
        </div>
        <div>${{this.product.price}}</div>
      </li>
    {{/each}}
  </ul>
  <form action="/api/orders/checkout" method="POST">
    <button class="btn btn-success">Checkout</button>
  </form>
{{else}}
  <p>Your cart is empty.</p>
{{/if}}
```
## 5. Enable Logout Button
In routes/auth.js:

```js
router.post('/logout', (req, res) => {
  res.clearCookie('token');
  res.redirect('/');
});
```

> Notes:
> - res.locals.user is accessible in any view thanks to middleware.
> - You can later show admin panels with {{#if (eq user.role "admin")}} using a Handlebars helper.
> - Don’t forget CSRF protection if doing full POST forms in production.