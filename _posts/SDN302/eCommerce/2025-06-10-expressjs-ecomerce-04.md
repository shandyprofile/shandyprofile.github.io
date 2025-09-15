---
title: 'Episode 3: User Auth - Register, Login & JWT'
description: >-
  Enable users to register and log in to the application securely using JWT tokens and HTTP-only cookies. Add basic authentication middleware.
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-18
categories: [(ExpressJS) Server-Side development, (ExpressJS) E-Commerce]
tags: [(ExpressJS) E-Commerce]
sort_index: 704
# pin: true
# media_subpath: '/posts/02'
---

**Technologies**
- Express.js
- Mongoose
- bcryptjs
- jsonwebtoken
- cookie-parser

## 1. Install Required Packages
```bash
npm install bcryptjs jsonwebtoken
```
## 2. Create the User Schema
In `models/User.js`:
```js
const mongoose = require('mongoose');

const UserSchema = new mongoose.Schema({
  email: {
    type: String,
    required: true,
    unique: true,
    lowercase: true
  },
  password: {
    type: String,
    required: true
  },
  role: {
    type: String,
    default: 'customer',
    enum: ['customer', 'admin']
  }
}, { timestamps: true });

module.exports = mongoose.model('User', UserSchema);
```

## 3. Create Authentication Routes
Create routes/auth.js:

```js
const express = require('express');
const router = express.Router();
const User = require('../models/User');
const bcrypt = require('bcryptjs');
const jwt = require('jsonwebtoken');

// Register
router.post('/register', async (req, res) => {
  const { email, password } = req.body;

  try {
    const exists = await User.findOne({ email });
    if (exists) return res.status(400).json({ msg: 'User already exists' });

    const hashedPassword = await bcrypt.hash(password, 10);
    const user = new User({ email, password: hashedPassword });
    await user.save();

    res.status(201).json({ msg: 'User registered successfully' });
  } catch (err) {
    res.status(500).json({ msg: 'Server error' });
  }
});

// Login
router.post('/login', async (req, res) => {
  const { email, password } = req.body;

  try {
    const user = await User.findOne({ email });
    if (!user) return res.status(400).json({ msg: 'Invalid credentials' });

    const isMatch = await bcrypt.compare(password, user.password);
    if (!isMatch) return res.status(400).json({ msg: 'Invalid credentials' });

    const token = jwt.sign(
      { id: user._id, role: user.role },
      process.env.JWT_SECRET,
      { expiresIn: '2h' }
    );

    res.cookie('token', token, {
      httpOnly: true,
      secure: false, // set to true in production with HTTPS
      maxAge: 2 * 60 * 60 * 1000
    });

    res.json({ msg: 'Login successful' });
  } catch (err) {
    res.status(500).json({ msg: 'Server error' });
  }
});

module.exports = router;
```

## 4. Register the Route in app.js
```js
const authRoutes = require('./routes/auth');
app.use('/api/auth', authRoutes);
```

## 5. Test with Postman
- **POST /api/auth/register**

```json
{
  "email": "test@example.com",
  "password": "password123"
}
```
- **POST /api/auth/login**
```json
{
  "email": "test@example.com",
  "password": "password123"
}
```

> After login, check browser or Postman cookies: you should see a token cookie.

## 6. Create Auth Middleware
Create middleware/auth.js:

```js
const jwt = require('jsonwebtoken');

const verifyToken = (req, res, next) => {
  const token = req.cookies.token;
  if (!token) return res.status(401).json({ msg: 'Unauthorized' });

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded; // Contains { id, role }
    next();
  } catch (err) {
    res.status(403).json({ msg: 'Token is not valid' });
  }
};

module.exports = verifyToken;
```
Use it in protected routes like this:

```js
const auth = require('../middleware/auth');
router.get('/protected', auth, (req, res) => {
  res.json({ msg: `Hello ${req.user.id}, you are authorized.` });
});
```
6. Test with Postman
### Register:
- POST http://localhost:3000/api/auth/register
- Body:
```json
{
  "email": "test@example.com",
  "password": "123456"
}
```

### Login:
- POST http://localhost:3000/api/auth/login
- Returns: Set-Cookie header with JWT token