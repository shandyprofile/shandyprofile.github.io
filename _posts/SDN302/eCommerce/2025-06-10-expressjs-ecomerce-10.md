---
title: 'Episode 9: Using OAuth for Social Login (Google, GitHub)'
description: >-
  Enable users to log in using Google and GitHub accounts via OAuth 2.0, using Passport.js and passport-google-oauth20 / passport-github2.
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-10
categories: [(ExpressJS) Server-Side development, (ExpressJS) E-Commerce]
tags: [(ExpressJS) E-Commerce]
sort_index: 710
# pin: true
# media_subpath: '/posts/02'
---

## Dependences:
- Express.js
- Passport.js
- passport-google-oauth20
- passport-github2
- JWT in cookies
- dotenv


## 1. Install OAuth Packages
```bash
npm install passport passport-google-oauth20 passport-github2
```

## 2. Set Environment Variables
In .env:
```ini
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret

GITHUB_CLIENT_ID=your-github-client-id
GITHUB_CLIENT_SECRET=your-github-client-secret
```

> You can get these from:
- Google Cloud Console
- GitHub Developer Settings

Make sure to set the callback URLs correctly:
- http://localhost:3000/auth/google/callback
- http://localhost:3000/auth/github/callback


## 3. Create a Basic Passport Setup
Create config/passport.js:

```js
const passport = require('passport');
const GoogleStrategy = require('passport-google-oauth20').Strategy;
const GitHubStrategy = require('passport-github2').Strategy;
const User = require('../models/User');

passport.use(new GoogleStrategy({
  clientID: process.env.GOOGLE_CLIENT_ID,
  clientSecret: process.env.GOOGLE_CLIENT_SECRET,
  callbackURL: '/auth/google/callback'
}, async (accessToken, refreshToken, profile, done) => {
  let user = await User.findOne({ oauthId: profile.id, provider: 'google' });
  if (!user) {
    user = await User.create({
      oauthId: profile.id,
      provider: 'google',
      email: profile.emails[0].value,
      name: profile.displayName
    });
  }
  done(null, user);
}));

passport.use(new GitHubStrategy({
  clientID: process.env.GITHUB_CLIENT_ID,
  clientSecret: process.env.GITHUB_CLIENT_SECRET,
  callbackURL: '/auth/github/callback'
}, async (accessToken, refreshToken, profile, done) => {
  let user = await User.findOne({ oauthId: profile.id, provider: 'github' });
  if (!user) {
    user = await User.create({
      oauthId: profile.id,
      provider: 'github',
      email: profile.emails?.[0]?.value || '',
      name: profile.displayName || profile.username
    });
  }
  done(null, user);
}));
```

## 4. Register Passport in app.js
```js
const passport = require('passport');
require('./config/passport');

app.use(passport.initialize());
```
## 5. Update User Model to Support OAuth
In models/User.js, extend the schema:
```js
oauthId: { type: String },
provider: { type: String },
name: String
```
## 6. Create OAuth Routes
In routes/oauth.js:

```js
const express = require('express');
const router = express.Router();
const jwt = require('jsonwebtoken');
const passport = require('passport');

// Google
router.get('/google', passport.authenticate('google', { scope: ['profile', 'email'] }));

router.get('/google/callback', passport.authenticate('google', { session: false }), (req, res) => {
  const token = jwt.sign({ id: req.user._id }, process.env.JWT_SECRET);
  res.cookie('token', token, { httpOnly: true });
  res.redirect('/');
});

// GitHub
router.get('/github', passport.authenticate('github', { scope: ['user:email'] }));

router.get('/github/callback', passport.authenticate('github', { session: false }), (req, res) => {
  const token = jwt.sign({ id: req.user._id }, process.env.JWT_SECRET);
  res.cookie('token', token, { httpOnly: true });
  res.redirect('/');
});

module.exports = router;
```
In app.js:
```js
const oauthRoutes = require('./routes/oauth');
app.use('/auth', oauthRoutes);
```
## 7. Add Buttons in Login View
In views/login.hbs:
```handlebars
<h2>Login</h2>
<form action="/api/auth/login" method="POST">
  <input name="email" type="email" class="form-control mb-2" placeholder="Email" />
  <input name="password" type="password" class="form-control mb-2" placeholder="Password" />
  <button type="submit" class="btn btn-primary mb-2">Login</button>
</form>

<p>Or login with:</p>
<a href="/auth/google" class="btn btn-danger me-2">Google</a>
<a href="/auth/github" class="btn btn-dark">GitHub</a>
```


> **Notes**
> - No passwords are stored for OAuth users.
> - If you later want to distinguish OAuth users, use user.provider field.
> - Combine OAuth with regular login for best UX flexibility.