---
title: 'Episode 7: Integrating Handlebars for Server-Side Views'
description: >-
  Use Handlebars as the templating engine to serve HTML pages (e.g. home, product list, product details, login page) directly from the backend.
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-18
categories: [(ExpressJS) Server-Side development, (ExpressJS) E-Commerce]
tags: [(ExpressJS) E-Commerce]
sort_index: 708
# pin: true
# media_subpath: '/posts/02'
---

## 1. Install Handlebars Engine
```bash
npm install express-handlebars
```
## 2. Setup Handlebars in app.js
```js
const express = require('express');
const exphbs = require('express-handlebars');
const app = express();

app.engine('hbs', exphbs.engine({ extname: '.hbs' }));
app.set('view engine', 'hbs');
app.set('views', './views');
```
## 3. Create Folder Structure
```/views
  ├── layouts/
  │     └── main.hbs
  ├── home.hbs
  ├── product-list.hbs
  ├── product-detail.hbs
  ├── login.hbs
  └── register.hbs
```
## 4. Create Main Layout
views/layouts/main.hbs:

```handlebars
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>{{title}}</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css">
</head>
<body>
  <div class="container mt-5">
    {{{body}}}
  </div>
</body>
</html>
```
## 5. Render Homepage

- In `routes/pages.js` (create new):

```js
const express = require('express');
const router = express.Router();
const Product = require('../models/Product');

router.get('/', async (req, res) => {
  const products = await Product.find();
  res.render('home', {
    title: 'Home',
    products
  });
});

module.exports = router;
```
- In `app.js`:

```js
const pageRoutes = require('./routes/pages');
app.use('/', pageRoutes);
```
## 6. Create Product List View
- `views/home.hbs`:

```handlebars
<h1 class="mb-4">Product List</h1>

<div class="row">
  {{#each products}}
    <div class="col-md-4">
      <div class="card mb-4">
        <div class="card-body">
          <h5>{{this.name}}</h5>
          <p>Price: ${{this.price}}</p>
          <a href="/product/{{this._id}}" class="btn btn-primary btn-sm">View</a>
        </div>
      </div>
    </div>
  {{/each}}
</div>
```
## 7. Create Product Detail View
- Add route in `routes/pages.js`:

```js
router.get('/product/:id', async (req, res) => {
  const product = await Product.findById(req.params.id);
  res.render('product-detail', {
    title: product.name,
    product
  });
});
```

- Create `views/product-detail.hbs`:

```handlebars
<h2>{{product.name}}</h2>
<p><strong>Price:</strong> ${{product.price}}</p>
<p>{{product.description}}</p>
<a href="/" class="btn btn-secondary btn-sm">Back</a>
```
## 8. Render Auth Pages
- In routes/pages.js, add:

```js
router.get('/login', (req, res) => {
  res.render('login', { title: 'Login' });
});

router.get('/register', (req, res) => {
  res.render('register', { title: 'Register' });
});
```
- Example `views/login.hbs`:

```handlebars
<h2>Login</h2>
<form action="/api/auth/login" method="POST">
  <input name="email" type="email" placeholder="Email" class="form-control mb-2" />
  <input name="password" type="password" placeholder="Password" class="form-control mb-2" />
  <button type="submit" class="btn btn-primary">Login</button>
</form>
```
> **Notes**
> - Use res.render(viewName, data) to pass dynamic content.
>- You can add conditional views (e.g. show/hide buttons) using if in Handlebars.
>- For partials and reusability, register partials in Handlebars config.

