---
title: 'Epoch 2: Build the User Service (ExpressJS)'
description: >-
  This epoch focuses on building the user-service using Node.js, ExpressJS, and MongoDB. The service will include user registration and login functionality, secured using JWT (JSON Web Token). Students will learn how to structure an Express project as a standalone microservice with its own database, controller, and routing layers.
author: [shandy]
date: 2025-07-17
updateDate: 2025-07-17
categories: [(ExpressJS) Server-Side development, (MicroServices) E-Commerce]
tags: [(MicroServices) E-Commerce]
sort_index: 903
# pin: true
# media_subpath: '/posts/02'
---

##  Objectives
- Create a REST API using ExpressJS and connect it to MongoDB.
- Design and validate a user schema using Mongoose.
- Implement user registration and login endpoints.
- Generate and verify JWTs for authentication.
- Structure a microservice project using best practices.

## 1. Microservice Overview
- **Service Name**: user-service
- **Responsibility**: Manage all user-related operations such as registration, login, and authentication.
- **Technology Stack**: Node.js, ExpressJS, MongoDB (via Mongoose), JWT

## 2. Structure architechture

```
user-service/
├── controllers/
│   └── auth.controller.js
├── models/
│   └── user.model.js
├── routes/
│   └── auth.routes.js
├── middlewares/
│   └── validateToken.js
├── utils/
│   └── jwt.js
├── .env
├── server.js
├── package.json
```

### 2.1. ExpressJS Setup
Install necessary packages:

```bash
npm init -y
npm install express mongoose dotenv jsonwebtoken bcryptjs
```

Basic Express server setup (server.js)
```js
const express = require('express');
const mongoose = require('mongoose');
require('dotenv').config();

const app = express();
app.use(express.json());

// Routes
app.use('/api/auth', require('./routes/auth.routes'));

mongoose.connect(process.env.MONGO_URI)
  .then(() => app.listen(process.env.PORT, () => {
    console.log(`User service running on port ${process.env.PORT}`);
  }))
  .catch(err => console.error(err));
```

### 2.2. User Schema (Mongoose)

```js
// models/user.model.js
const mongoose = require('mongoose');
const bcrypt = require('bcryptjs');

const userSchema = new mongoose.Schema({
  name: String,
  email: { type: String, unique: true },
  password: String,
  role: { type: String, default: 'customer' }
});

userSchema.pre('save', async function(next) {
  if (this.isModified('password')) {
    this.password = await bcrypt.hash(this.password, 10);
  }
  next();
});

module.exports = mongoose.model('User', userSchema);
```

### 2.3 Authentication Controller

```js
// controllers/auth.controller.js
const User = require('../models/user.model');
const jwt = require('jsonwebtoken');
const bcrypt = require('bcryptjs');

exports.register = async (req, res) => {
  const { name, email, password } = req.body;
  try {
    const user = new User({ name, email, password });
    await user.save();
    res.status(201).json({ message: 'User registered successfully.' });
  } catch (err) {
    res.status(400).json({ error: 'Registration failed.' });
  }
};

exports.login = async (req, res) => {
  const { email, password } = req.body;
  try {
    const user = await User.findOne({ email });
    if (!user || !await bcrypt.compare(password, user.password))
      return res.status(401).json({ error: 'Invalid credentials' });

    const token = jwt.sign({ userId: user._id, role: user.role }, process.env.JWT_SECRET);
    res.json({ token });
  } catch (err) {
    res.status(500).json({ error: 'Login failed.' });
  }
};
```

### 2.4. Routes
```js
// routes/auth.routes.js
const router = require('express').Router();
const { register, login } = require('../controllers/auth.controller');

router.post('/register', register);
router.post('/login', login);

module.exports = router;
```

### 2.5. Environment Variables (.env)
```env
PORT=4001
MONGO_URI=mongodb://localhost:27017/user-service
JWT_SECRET=your_jwt_secret
```

## 4. API Testing with Postman

| Method | Endpoint             | Payload                     | Description              |
| ------ | -------------------- | --------------------------- | ------------------------ |
| POST   | `/api/auth/register` | `{ name, email, password }` | Register new user        |
| POST   | `/api/auth/login`    | `{ email, password }`       | Log in and receive token |

- Set Content-Type: application/json in headers.
- On successful login, copy the JWT token to use for protected routes in later epochs.