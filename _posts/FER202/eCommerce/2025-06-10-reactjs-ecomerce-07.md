---
title: 'Episode 6: Login Functionality with LocalStorage'
description: >-
  Build Login functionality with simple authentication (based on email/password in db.json) and use LocalStorage to save login state
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-18
categories: [(ReactJS) Front-End web development, (ReactJS) E-Commerce]
tags: [(ReactJS) E-Commerce]
sort_index: 7
# pin: true
# media_subpath: '/posts/02'
---

In this episode, you'll build a basic login system:
- Match email/password from JSON-Server
- Save login info in `localStorage`
- Show conditional content in Navbar

---

## 1. Create Login Form

Update `src/pages/Login.jsx`:

```jsx
import { useState } from "react";
import axios from "axios";
import { useNavigate } from "react-router-dom";

const Login = () => {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const navigate = useNavigate();

  const handleLogin = async (e) => {
    e.preventDefault();

    try {
      const res = await axios.get(`http://localhost:3001/users?email=${email}&password=${password}`);
      if (res.data.length > 0) {
        const user = res.data[0];
        localStorage.setItem("user", JSON.stringify(user));
        alert("Login successful!");
        navigate("/");
      } else {
        alert("Invalid credentials");
      }
    } catch (err) {
      console.error(err);
      alert("Login failed");
    }
  };

  return (
    <div className="max-w-md mx-auto p-6 mt-10 bg-white shadow rounded">
      <h2 className="text-2xl font-bold mb-4">Login</h2>
      <form onSubmit={handleLogin} className="space-y-4">
        <input
          type="email"
          placeholder="Email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
          required
          className="w-full border px-4 py-2 rounded"
        />
        <input
          type="password"
          placeholder="Password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
          required
          className="w-full border px-4 py-2 rounded"
        />
        <button
          type="submit"
          className="w-full bg-blue-600 text-white py-2 rounded hover:bg-blue-700"
        >
          Login
        </button>
      </form>
    </div>
  );
};

export default Login;
```

## 2. Update Navbar with User Info
Update Navbar.jsx to show the user email and Logout button if logged in:

```jsx
import { Link, useNavigate } from "react-router-dom";

const Navbar = () => {
  const user = JSON.parse(localStorage.getItem("user"));
  const navigate = useNavigate();

  const handleLogout = () => {
    localStorage.removeItem("user");
    navigate("/");
  };

  return (
    <nav className="bg-gray-800 text-white px-6 py-4 flex justify-between items-center">
      <Link to="/" className="text-xl font-bold">E-Shop</Link>
      <div className="space-x-4 flex items-center">
        <Link to="/" className="hover:underline">Home</Link>
        <Link to="/cart" className="hover:underline">Cart</Link>
        {user ? (
          <>
            <span className="text-sm text-gray-300">Hello, {user.email}</span>
            <button onClick={handleLogout} className="hover:underline">Logout</button>
          </>
        ) : (
          <Link to="/login" className="hover:underline">Login</Link>
        )}
      </div>
    </nav>
  );
};

export default Navbar;
```

3. Sample User in db.json
Ensure your `db.json` has at least one valid user:

```json
"users": [
  {
    "id": 1,
    "email": "user@example.com",
    "password": "123456"
  }
]
```