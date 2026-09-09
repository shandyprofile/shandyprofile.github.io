---
title: 'Episode 9: Persistent Login, Auto Logout & UI Polish with TailwindCSS'
description: >-
  Auto login persists between page loads, and set up auto logout after a period of inactivity.
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-18
categories: [(ReactJS) Front-End web development, (ReactJS) E-Commerce]
tags: [(ReactJS) E-Commerce]
sort_index: 10
# pin: true
# media_subpath: '/posts/02'
---

In this episode, you will:
- Implement persistent login using `localStorage`
- Automatically log users out after 10 minutes
- Polish the UI using TailwindCSS
- Make layout responsive across screen sizes

---

## Part 1: Persistent Login & Auto Logout

### 1. Persistent Login (Already Done with LocalStorage)

Since we already save user info in `localStorage`, the session **persists between refreshes**.  
Now, we’ll add a `loginTime` and manage **session expiration**.

---

### 2. Save Login Time

Update the login logic in `Login.jsx`:

```js
localStorage.setItem("user", JSON.stringify(user));
localStorage.setItem("loginTime", Date.now());
```

## 3. Enhance PrivateRoute.jsx to Check Timeout
Update src/components/PrivateRoute.jsx:

```jsx
import { Navigate } from "react-router-dom";

const SESSION_TIMEOUT = 10 * 60 * 1000; // 10 minutes

const PrivateRoute = ({ children }) => {
  const user = JSON.parse(localStorage.getItem("user"));
  const loginTime = parseInt(localStorage.getItem("loginTime"));

  const now = Date.now();
  const expired = now - loginTime > SESSION_TIMEOUT;

  if (!user || expired) {
    localStorage.removeItem("user");
    localStorage.removeItem("loginTime");
    return <Navigate to="/login" />;
  }

  return children;
};

export default PrivateRoute;
```
## 4. Optional: Show Logout Countdown
Inside Navbar.jsx, show how long before session expires:

```jsx
const loginTime = parseInt(localStorage.getItem("loginTime"));
const remaining = Math.max(0, 10 * 60 - Math.floor((Date.now() - loginTime) / 1000));
```
You can render this with:
```html
<span className="text-sm text-gray-200">Session: {secondsLeft}s</span>
```

5. Test the Flow

- Log in
- Refresh page => still logged in => Sucess
- Wait 10+ minutes => try to access /cart => Can't accesss and redirected to login page

## Part 2: Polish UI with TailwindCSS
### 1. Product Card Styling
In ProductCard.jsx:

```jsx
<div className="bg-white rounded-2xl shadow-md p-4 flex flex-col hover:shadow-lg transition">
  <img src={product.image} className="w-full h-48 object-cover rounded-lg mb-4" />
  <h2 className="text-lg font-semibold line-clamp-2">{product.title}</h2>
  <p className="text-blue-600 font-bold mt-2">${product.price}</p>
  <button className="mt-auto bg-blue-600 text-white py-2 rounded-lg hover:bg-blue-700 transition">
    Add to Cart
  </button>
</div>
```

### 2. Responsive Grid in Home.jsx
```jsx
<div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
  {products.map(product => (
    <ProductCard key={product.id} product={product} />
  ))}
</div>
```
### 3. Cart Item Styling in Cart.jsx
```jsx
<div className="border p-4 rounded-xl flex flex-col sm:flex-row justify-between gap-4 items-center">
  <div>
    <h2 className="font-semibold text-lg">{item.title}</h2>
    <p className="text-gray-600">${item.price}</p>
    <div className="flex items-center gap-2 mt-2">
      <button className="bg-gray-200 px-2 py-1 rounded" onClick={...}>−</button>
      <span>{item.quantity}</span>
      <button className="bg-gray-200 px-2 py-1 rounded" onClick={...}>+</button>
    </div>
  </div>
  <div className="text-right">
    <p className="font-bold">${(item.price * item.quantity).toFixed(2)}</p>
    <button className="text-red-500 mt-2 hover:underline" onClick={...}>Remove</button>
  </div>
</div>
```
### 4. Responsive Navbar
In `Navbar.jsx`:
```jsx
<nav className="flex items-center justify-between p-4 bg-blue-600 text-white flex-wrap">
  <div className="text-xl font-bold">E-Shop</div>
  <div className="space-x-4 mt-2 sm:mt-0">
    <Link to="/" className="hover:underline">Home</Link>
    <Link to="/cart" className="hover:underline">Cart</Link>
    <Link to="/login" className="hover:underline">Login</Link>
  </div>
</nav>
```