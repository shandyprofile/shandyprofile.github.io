---
title: 'Epoch 3: Build the Product Service (ExpressJS)'
description: >-
  In this epoch, students will create the product-service, a standalone microservice responsible for managing the product catalog. It will support full CRUD operations (Create, Read, Update, Delete), include data validation, and be structured following microservice best practices using ExpressJS and MongoDB.
author: [shandy]
date: 2025-07-17
updateDate: 2025-07-17
categories: [(ExpressJS) Server-Side development, (MicroServices) E-Commerce]
tags: [(MicroServices) E-Commerce]
sort_index: 904
# pin: true
# media_subpath: '/posts/02'
---

##  Objectives
- Build a separate microservice for managing product data.
- Connect the service to its own MongoDB database.
- Implement CRUD operations for products using RESTful API principles.
- Apply input validation and error handling.
- Maintain a clean and modular folder structure.

## 1. Microservice Overview
- **Service Name**: product-service
- **Responsibility**: Manage product catalog including create, update, view, and delete operations.
- **Technology Stack**: Node.js, ExpressJS, MongoDB, Mongoose

## 2. Structure architechture

```
product-service/
├── controllers/
│   └── product.controller.js
├── models/
│   └── product.model.js
├── routes/
│   └── product.routes.js
├── middlewares/
│   └── errorHandler.js
├── .env
├── server.js
├── package.json
```

### 2.1. Project Setup
- Step 1: Initialize the Project
```bash
mkdir product-service
cd product-service
npm init -y
```

- Step 2: Install Dependencies
```bash
npm install express mongoose dotenv
```

- Step 3: Project Structure: Create the following folders and files:
```bash
mkdir controllers models routes middlewares
touch server.js .env
```

### 2.2. Mongoose Product Schema

```js
// models/product.model.js
const mongoose = require('mongoose');

const productSchema = new mongoose.Schema({
  name: { type: String, required: true },
  description: String,
  price: { type: Number, required: true, min: 0 },
  stock: { type: Number, default: 0 },
  category: String,
  createdAt: { type: Date, default: Date.now }
});

module.exports = mongoose.model('Product', productSchema);
```

### 2.3. Product Controller
- controllers/product.controller.js
```js
const Product = require('../models/product.model');

exports.createProduct = async (req, res) => {
  try {
    const product = await Product.create(req.body);
    res.status(201).json(product);
  } catch (err) {
    res.status(400).json({ error: 'Failed to create product.' });
  }
};

exports.getAllProducts = async (req, res) => {
  const products = await Product.find();
  res.json(products);
};

exports.getProductById = async (req, res) => {
  const product = await Product.findById(req.params.id);
  if (!product) return res.status(404).json({ error: 'Product not found' });
  res.json(product);
};

exports.updateProduct = async (req, res) => {
  const product = await Product.findByIdAndUpdate(req.params.id, req.body, { new: true });
  if (!product) return res.status(404).json({ error: 'Product not found' });
  res.json(product);
};

exports.deleteProduct = async (req, res) => {
  const product = await Product.findByIdAndDelete(req.params.id);
  if (!product) return res.status(404).json({ error: 'Product not found' });
  res.json({ message: 'Product deleted successfully' });
};
```

### 2.4. Routes
- routes/product.routes.js
```js
const express = require('express');
const router = express.Router();
const {
  createProduct,
  getAllProducts,
  getProductById,
  updateProduct,
  deleteProduct
} = require('../controllers/product.controller');

router.post('/', createProduct);
router.get('/', getAllProducts);
router.get('/:id', getProductById);
router.put('/:id', updateProduct);
router.delete('/:id', deleteProduct);

module.exports = router;
```

### 2.5. Server Setup
- server.js
```js
// server.js
const express = require('express');
const mongoose = require('mongoose');
require('dotenv').config();

const app = express();
app.use(express.json());

// Routes
app.use('/api/products', require('./routes/product.routes'));

mongoose.connect(process.env.MONGO_URI)
  .then(() => app.listen(process.env.PORT, () => {
    console.log(`Product service running on port ${process.env.PORT}`);
  }))
  .catch(err => console.error(err));
```

### 2.6. Environment Variables
Create a .env file in the root directory:
```
PORT=4002
MONGO_URI=mongodb://localhost:27017/product-service
```

## 3. Testing with Postman
- Example Request Payload
{
  "name": "iPhone 15",
  "description": "Latest Apple smartphone",
  "price": 999,
  "stock": 10,
  "category": "Electronics"
}

| Method | Endpoint            | Description        | Request Body |
| ------ | ------------------- | ------------------ | ------------ |
| POST   | `/api/products`     | Create new product | has Payload  |
| GET    | `/api/products`     | Get all products   | -            |
| GET    | `/api/products/:id` | Get product by ID  | -            |
| PUT    | `/api/products/:id` | Update product     | has Payload  |
| DELETE | `/api/products/:id` | Delete product     | -            |

