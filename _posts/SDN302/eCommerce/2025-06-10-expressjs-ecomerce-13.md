---
title: 'Episode 12: Deployment & Final Polish'
description: >-
  Prepare your full-stack e-Commerce application for production deployment. Ensure all core features work reliably through testing, and deploy the backend and frontend using a production-ready service (e.g., Render, Railway, Vercel, or Netlify).
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-10
categories: [(ExpressJS) Server-Side development, (ExpressJS) E-Commerce]
tags: [(ExpressJS) E-Commerce]
sort_index: 713
private: true
# pin: true
# media_subpath: '/posts/02'
---

## 1. Manual Feature Testing
Test all critical use cases in development mode:

| Feature             | Test Description                               |
| ------------------- | ---------------------------------------------- |
| User Register/Login | Create user, login/logout, invalid credentials |
| JWT Auth            | Ensure tokens expire and validate correctly    |
| Cart                | Add/remove/update items in cart                |
| Checkout            | Create order, view past orders                 |
| Admin Access        | Restricted to admin users only                 |
| OAuth               | Test Google/GitHub login                       |
| Cookies             | Confirm cookies are set, expire, and secure    |

## 2.  Optional: Automated Testing (Jest + Supertest)
Install:
```bash
npm install --save-dev jest supertest
```
Example test (tests/auth.test.js):
```js
const request = require('supertest');
const app = require('../app');

describe('Auth Routes', () => {
  it('should register a new user', async () => {
    const res = await request(app).post('/api/auth/register').send({
      email: 'test@example.com',
      password: 'password123'
    });
    expect(res.statusCode).toBe(200);
  });
});
```

In package.json:

```json
"scripts": {
  "test": "jest"
}
```

Run:

```bash
npm test
```

## 3. Prepare for Production
- Disable console.logs
- Enable compression middleware:
```js
const compression = require('compression');
app.use(compression());
```

Set Secure Cookie Options:
```js
res.cookie('token', token, {
  httpOnly: true,
  secure: true, // use with HTTPS only
  sameSite: 'Lax',
});
```