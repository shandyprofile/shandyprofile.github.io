---
title: 'Episode 2: Product Model & MongoDB Setup'
description: >-
  Define the Product model using Mongoose and implement RESTful API endpoints for managing products.
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-10
categories: [(ExpressJS) Server-Side development, (ExpressJS) E-Commerce]
tags: [(ExpressJS) E-Commerce]
sort_index: 703
# pin: true
# media_subpath: '/posts/02'
---


**Technologies**
- Express.js
- Mongoose
- REST API
- MongoDB

## Project Structure
```
ecommerce-backend/
├── models/
│   └── Product.js
├── routes/
│   ├── products.js
│   └── index.js
├── app.js
├── ...
```

## 1. Create the Product Schema
Create a new folder:

```bash
mkdir models
```

In models/Product.js:

```js
const mongoose = require('mongoose');

const ProductSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
    trim: true
  },
  price: {
    type: Number,
    required: true
  },
  description: String,
  category: String,
  stock: {
    type: Number,
    default: 0
  },
  image: String
}, { timestamps: true });

module.exports = mongoose.model('Product', ProductSchema);
```

Data:
```json
[
  {
    "name": "RGB Mechanical Keyboard",
    "price": 120.50,
    "description": "Mechanical keyboard with customizable RGB backlighting",
    "category": "Peripherals",
    "stock": 30,
    "image": "https://example.com/keyboard_rgb.png"
  },
  {
    "name": "27-inch 4K Monitor",
    "price": 399.00,
    "description": "27-inch IPS monitor with 4K resolution",
    "category": "Monitors",
    "stock": 15,
    "image": "https://example.com/monitor_4k.png"
  },
  {
    "name": "1TB NVMe SSD",
    "price": 85.99,
    "description": "High-speed 1TB NVMe Solid State Drive",
    "category": "Computer Components",
    "stock": 100,
    "image": "https://example.com/ssd_nvme.png"
  },
  {
    "name": "Wireless Bluetooth Headphones",
    "price": 75.25,
    "description": "Over-ear wireless headphones with high-fidelity audio",
    "category": "Audio Devices",
    "stock": 70,
    "image": "https://example.com/headphone_wireless.png"
  },
  {
    "name": "Full HD 1080p Webcam",
    "price": 35.00,
    "description": "1080p resolution webcam for clear video calls",
    "category": "Peripherals",
    "stock": 40,
    "image": "https://example.com/webcam_hd.png"
  },
  {
    "name": "Wi-Fi 6 Router",
    "price": 99.99,
    "description": "High-speed router supporting Wi-Fi 6 standard",
    "category": "Networking Devices",
    "stock": 25,
    "image": "https://example.com/router_wifi6.png"
  },
  {
    "name": "Portable Bluetooth Speaker",
    "price": 45.00,
    "description": "Waterproof portable speaker with long battery life",
    "category": "Audio Devices",
    "stock": 60,
    "image": "https://example.com/speaker_portable.png"
  },
  {
    "name": "20000mAh Power Bank",
    "price": 29.50,
    "description": "High-capacity power bank with dual USB ports",
    "category": "Phone Accessories",
    "stock": 80,
    "image": "https://example.com/powerbank.png"
  },
  {
    "name": "Android Smartphone",
    "price": 499.00,
    "description": "Smartphone with 48MP camera and 5000mAh battery",
    "category": "Phones",
    "stock": 20,
    "image": "https://example.com/smartphone_android.png"
  },
  {
    "name": "10-inch Tablet",
    "price": 250.00,
    "description": "10-inch tablet with Full HD display",
    "category": "Tablets",
    "stock": 10,
    "image": "https://example.com/tablet_10inch.png"
  },
  {
    "name": "GPS Smartwatch",
    "price": 180.00,
    "description": "Smartwatch with GPS and heart rate sensor",
    "category": "Wearables",
    "stock": 35,
    "image": "https://example.com/smartwatch_gps.png"
  },
  {
    "name": "Canon DSLR Camera",
    "price": 750.00,
    "description": "DSLR camera with 18-55mm kit lens",
    "category": "Photography Equipment",
    "stock": 5,
    "image": "https://example.com/camera_dslr.png"
  },
  {
    "name": "Telephoto Camera Lens",
    "price": 290.00,
    "description": "70-300mm telephoto zoom camera lens",
    "category": "Photography Equipment",
    "stock": 8,
    "image": "https://example.com/lens_telephoto.png"
  },
  {
    "name": "E-Reader",
    "price": 110.00,
    "description": "E-reader with a 6-inch E-Ink display",
    "category": "E-books",
    "stock": 20,
    "image": "https://example.com/ebook_reader.png"
  },
  {
    "name": "Handheld Gaming Device",
    "price": 299.00,
    "description": "Portable gaming device with an OLED screen",
    "category": "Gaming Devices",
    "stock": 12,
    "image": "https://example.com/handheld_gaming.png"
  },
  {
    "name": "Smart RGB LED Light Bulb",
    "price": 15.00,
    "description": "Smart LED bulb controllable via app with RGB colors",
    "category": "Smart Home",
    "stock": 90,
    "image": "https://example.com/smart_led.png"
  },
  {
    "name": "Wi-Fi Smart Plug",
    "price": 19.99,
    "description": "Smart power outlet with timer functionality",
    "category": "Smart Home",
    "stock": 55,
    "image": "https://example.com/smart_plug.png"
  },
  {
    "name": "Robot Vacuum Cleaner",
    "price": 350.00,
    "description": "Automatic robot vacuum cleaner with room mapping",
    "category": "Smart Appliances",
    "stock": 7,
    "image": "https://example.com/robot_vacuum.png"
  },
  {
    "name": "500ml Stainless Steel Thermos",
    "price": 12.00,
    "description": "Insulated stainless steel thermos with 500ml capacity",
    "category": "Personal Items",
    "stock": 120,
    "image": "https://example.com/thermos.png"
  },
  {
    "name": "Waterproof Laptop Backpack",
    "price": 40.00,
    "description": "Water-resistant backpack designed for 15.6-inch laptops",
    "category": "Accessories",
    "stock": 65,
    "image": "https://example.com/laptop_backpack.png"
  }
]
```
## 2. Create Product Routes
Create a new file: `routes/products.js`
```js
const express = require('express');
const router = express.Router();
const Product = require('../models/Product');

router.get('/', async (req, res) => {
  const products = await Product.find();
  res.json(products);
});

router.get('/:id', async (req, res) => {
  const product = await Product.findById(req.params.id);
  if (!product) return res.status(404).json({ msg: 'Product not found' });
  res.json(product);
});

router.post('/', async (req, res) => {
  const newProduct = new Product(req.body);
  const savedProduct = await newProduct.save();
  res.status(201).json(savedProduct);
});

module.exports = router;
```
## 3. Register the Product Route in app.js
In app.js:

```js
const productRoutes = require('./routes/products');
app.use('/api/products', productRoutes);
```

## 4. Test with Postman
- GET http://localhost:3000/api/products → list all products
- GET http://localhost:3000/api/products/:id → get single product
- POST http://localhost:3000/api/products with JSON body:

```json
{
  "name": "Gaming Mouse",
  "price": 49.99,
  "description": "High DPI wireless mouse",
  "category": "Accessories",
  "stock": 50,
  "image": "https://example.com/mouse.png"
}
```