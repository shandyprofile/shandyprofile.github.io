---
title: 'Episode 1: Project Initialization & Setup'
description: >-
  Set up a basic Express.js project using Express Generator, configure environment, folder structure, and enable CORS for cross-origin requests.
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-10
categories: [(ExpressJS) Server-Side development, (ExpressJS) E-Commerce]
tags: [(ExpressJS) E-Commerce]
sort_index: 702
# pin: true
# media_subpath: '/posts/02'
---

**Technologies**
- Express.js
- Express Generator
- Node.js
- dotenv
- CORS

## 1. Install Express Generator

Install library `express-generator` into root NodeJS.
``` bash
npm install -g express-generator
```

## 2. Scaffold the Project

```bash
express ecommerce-backend --view=hbs
cd ecommerce-backend
npm install
```

--view=hbs: Uses Handlebars as the view engine.

## 3. Install Required Dependencies
Update `tailwind.config.js`:

```bash
npm install dotenv cors mongoose cookie-parser
```
## 4. Setup Environment Variables
Create a `.env` file at the root:

```env
PORT=3000
MONGO_URI=mongodb://localhost:27017/ecommerce_db
JWT_SECRET=your_jwt_secret
```

## 5. Configure app.js
Load `.env` and set up middlewares:

```js
require('dotenv').config();
const cors = require('cors');
const cookieParser = require('cookie-parser');

app.use(cors({
  origin: 'http://localhost:5173', // Frontend URL
  credentials: true
}));

app.use(cookieParser());
```

## 6. Connect to MongoDB
In a new file config/db.js:
```js
const mongoose = require('mongoose');

const connectDB = async () => {
  try {
    await mongoose.connect(process.env.MONGO_URI);
    console.log('MongoDB connected');
  } catch (err) {
    console.error(err.message);
    process.exit(1);
  }
};

module.exports = connectDB;
```
And in `bin/www` or `app.js`, initialize the DB:

```js
const connectDB = require('./config/db');
connectDB();
```

## 7. Test the Server
```bash
npm start
```
Visit: http://localhost:3000

You should see the default Handlebars index page.

## Project Structure After Setup
```
ecommerce-backend/
│
├── app.js
├── bin/www
├── config/
│   └── db.js
├── routes/
│   ├── index.js
│   └── users.js
├── views/
│   └── index.hbs
├── .env
├── package.json
```