---
title: 'Episode 7: Route Protection (Private Routes)'
description: >-
  Protect the Cart page so that only logged in users can access it. If they are not logged in, they will be redirected to the Login page.
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-10
categories: [(ReactJS) Front-End web development, (ReactJS) E-Commerce]
tags: [(ReactJS) E-Commerce]
sort_index: 8
# pin: true
# media_subpath: '/posts/02'
---

In this episode, you’ll:
- Create a **PrivateRoute** component
- Restrict access to the **Cart** page
- Redirect unauthorized users to **Login**

---

## 1. Create `PrivateRoute` Component

In `src/components/PrivateRoute.jsx`:

```jsx
import { Navigate } from "react-router-dom";

const PrivateRoute = ({ children }) => {
  const user = JSON.parse(localStorage.getItem("user"));
  return user ? children : <Navigate to="/login" />;
};

export default PrivateRoute;
```

## 2. Use PrivateRoute in App.js
Update the Cart route in App.js:

```jsx
import PrivateRoute from "./components/PrivateRoute";

// ...

<Routes>
  <Route path="/" element={<Home />} />
  <Route
    path="/cart"
    element={
      <PrivateRoute>
        <Cart />
      </PrivateRoute>
    }
  />
  <Route path="/login" element={<Login />} />
</Routes>
```
## 3. Test the Protection
Try these steps:
- Visit `/cart` as a logged-out user => should redirect to `/login`
- Log in => `/cart` is now accessible