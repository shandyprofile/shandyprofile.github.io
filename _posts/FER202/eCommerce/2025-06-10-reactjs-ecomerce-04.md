---
title: 'Episode 3: Add React Router'
description: >-
  In this episode, we’ll integrate **React Router** into our app to allow page navigation between **Home**, **Cart**, and **Login**.
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-10
categories: [(ReactJS) Front-End web development, E-Commerce]
tags: [Episode 3 – Add React Router]
sort_index: 4
# pin: true
# media_subpath: '/posts/02'
---

## 1. Setup Router in `App.js`

Update `src/App.js`:

```jsx
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
import Home from "./pages/Home";
import Cart from "./pages/Cart";
import Login from "./pages/Login";
import Navbar from "./components/Navbar";

function App() {
  return (
    <Router>
      <Navbar />
      <div className="p-4">
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/cart" element={<Cart />} />
          <Route path="/login" element={<Login />} />
        </Routes>
      </div>
    </Router>
  );
}

export default App;
```

## 2. Create Basic Page Components
Create Cart page: `src/pages/Cart.jsx`
```js
const Cart = () => {
  return <h1 className="text-2xl">Cart Page</h1>;
};

export default Cart;

```

Create Login page: `src/pages/Login.jsx`

```jsx
const Login = () => {
  return <h1 className="text-2xl">Login Page</h1>;
};

export default Login;
```
## 3. Create Navbar Component
Create Navbar component: `src/components/Navbar.jsx`:

```jsx
import { Link } from "react-router-dom";

const Navbar = () => {
  return (
    <nav className="bg-gray-800 text-white px-6 py-4 flex justify-between items-center">
      <Link to="/" className="text-xl font-bold">E-Shop</Link>
      <div className="space-x-4">
        <Link to="/" className="hover:underline">Home</Link>
        <Link to="/cart" className="hover:underline">Cart</Link>
        <Link to="/login" className="hover:underline">Login</Link>
      </div>
    </nav>
  );
};

export default Navbar;
```

## 4. Test the Navigation
Run the app: 

```bash
npm start
```

- Click on Home, Cart, and Login links in the navbar
- Each route should display the corresponding page