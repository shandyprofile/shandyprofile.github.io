---
title: 'Epoch 4: Build the Order Service (ExpressJS)'
description: >-
  In this epoch, students will build the order-service, a self-contained microservice that handles customer orders in an e-commerce system. This service will communicate with the user-service and product-service indirectly and manage the lifecycle of an order — including placing, viewing, updating, and deleting orders. We'll also introduce the concept of referencing other services' IDs without direct joins, using MongoDB references (ObjectId), and structuring order documents in a scalable way.
author: [shandy]
date: 2025-07-17
updateDate: 2025-07-17
categories: [(ExpressJS) Server-Side development, (MicroServices) E-Commerce]
tags: [(MicroServices) E-Commerce]
sort_index: 905
# pin: true
# media_subpath: '/posts/02'
---

##  Objectives
- Design and build an order schema using Mongoose.
- Implement CRUD operations for order management.
- Structure order data to reference userId and productId.
- Test order workflows with realistic payloads.
- Understand how microservices communicate via IDs (later: through APIs or message queues).



## 1. Microservice Overview
- **Service Name**: order-service
- **Responsibility**: Manage order catalog including create, update, view, and delete operations.
- **Technology Stack**: Node.js, ExpressJS, MongoDB, Mongoose

## 2. Structure architechture

```
order-service/
├── controllers/
│   └── order.controller.js
├── models/
│   └── order.model.js
├── routes/
│   └── order.routes.js
├── .env
├── server.js
├── package.json
```

### 2.1. Project Setup
- Step 1: Initialize the Project
```bash
mkdir order-service
cd order-service
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

### 2.2. Mongoose Order Schema
- models/order.model.js

```js
const mongoose = require('mongoose');

const orderSchema = new mongoose.Schema({
  userId: { type: String, required: true },
  products: [
    {
      productId: String,
      quantity: Number
    }
  ],
  totalPrice: { type: Number, required: true },
  status: {
    type: String,
    enum: ['pending', 'processing', 'shipped', 'delivered', 'cancelled'],
    default: 'pending'
  },
  createdAt: { type: Date, default: Date.now }
});

module.exports = mongoose.model('Order', orderSchema);
```

### 2.3. Order Controller
- controllers/order.controller.js
```js
const Order = require('../models/order.model');

exports.createOrder = async (req, res) => {
  try {
    const order = await Order.create(req.body);
    res.status(201).json(order);
  } catch (err) {
    res.status(400).json({ error: 'Failed to create order.' });
  }
};

exports.getAllOrders = async (req, res) => {
  const orders = await Order.find();
  res.json(orders);
};

exports.getOrderById = async (req, res) => {
  const order = await Order.findById(req.params.id);
  if (!order) return res.status(404).json({ error: 'Order not found' });
  res.json(order);
};

exports.updateOrder = async (req, res) => {
  const order = await Order.findByIdAndUpdate(req.params.id, req.body, { new: true });
  if (!order) return res.status(404).json({ error: 'Order not found' });
  res.json(order);
};

exports.deleteOrder = async (req, res) => {
  const order = await Order.findByIdAndDelete(req.params.id);
  if (!order) return res.status(404).json({ error: 'Order not found' });
  res.json({ message: 'Order deleted successfully' });
};
```

### 2.4. Routes
- routes/order.routes.js
```js
const express = require('express');
const router = express.Router();
const {
  createOrder,
  getAllOrders,
  getOrderById,
  updateOrder,
  deleteOrder
} = require('../controllers/order.controller');

router.post('/', createOrder);
router.get('/', getAllOrders);
router.get('/:id', getOrderById);
router.put('/:id', updateOrder);
router.delete('/:id', deleteOrder);

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

app.use('/api/orders', require('./routes/order.routes'));

mongoose.connect(process.env.MONGO_URI)
  .then(() => app.listen(process.env.PORT, () => {
    console.log(`Order service running on port ${process.env.PORT}`);
  }))
  .catch(err => console.error(err));
```

### 2.6. Environment Variables
Create a .env file in the root directory:
```
PORT=4003
MONGO_URI=mongodb://localhost:27017/order-service
```

## 3. Testing with Postman
- Example Request Payload
{
  "userId": "64adfa1...abcd",
  "products": [
    { "productId": "64aef123...xyz", "quantity": 2 },
    { "productId": "64aef124...klm", "quantity": 1 }
  ],
  "totalPrice": 1999
}

| Method | Endpoint          | Description         | Request Body |
| ------ | ----------------- | ------------------- | ------------ |
| POST   | `/api/orders`     | Create new order    | has Payload  |
| GET    | `/api/orders`     | Get all orders      | -            |
| GET    | `/api/orders/:id` | Get order by ID     | -            |
| PUT    | `/api/orders/:id` | Update order status | has Payload  |
| DELETE | `/api/orders/:id` | Delete order        | -            |


