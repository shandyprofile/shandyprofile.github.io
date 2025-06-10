---
title: 'Episode 4: Connect with JSON-Server to fetch real product data'
description: >-
  In this episode, we'll set up a **mock backend** using [JSON-Server](https://github.com/typicode/json-server). This allows us to simulate a REST API for products, users, and cart items.
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-10
categories: [(ReactJS) Front-End web development, E-Commerce]
tags: [Episode 4 – Connect with JSON-Server to fetch real product data]
sort_index: 5
# pin: true
# media_subpath: '/posts/02'
---

## 1. Install JSON-Server Globally
Install JSON-Server
```bash
npm install -g json-server
```
## 2. Create db.json File
At the root of your project (outside src/), create a file named db.json:

```json
{
  "products": [
    {
      "id": 1,
      "title": "Basic T-Shirt",
      "price": 19.99,
      "image": "https://via.placeholder.com/300x200"
    },
    {
      "id": 2,
      "title": "Stylish Hat",
      "price": 25.99,
      "image": "https://via.placeholder.com/300x200"
    },
    {
      "id": 3,
      "title": "Running Shoes",
      "price": 49.99,
      "image": "https://via.placeholder.com/300x200"
    }
  ],
  "users": [
    {
      "id": 1,
      "email": "user@example.com",
      "password": "123456"
    }
  ],
  "cart": []
}
```

## 3. Start JSON-Server
Run this command in the terminal:

```bash
json-server --watch db.json --port 3001
```

This will start the mock API at:
http://localhost:3001/

Available endpoints:
- /products
- /users
- /cart

## 4. Fetch Products from API
Replace Dummy Data in Home.jsx:
```jsx
import { useEffect, useState } from "react";
import ProductCard from "../components/ProductCard";
import axios from "axios";

const Home = () => {
  const [products, setProducts] = useState([]);

  useEffect(() => {
    axios.get("http://localhost:3001/products")
      .then((res) => setProducts(res.data))
      .catch((err) => console.error(err));
  }, []);

  return (
    <div className="p-4">
      <h1 className="text-3xl font-bold mb-6">All Products</h1>
      <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6">
        {products.map((item) => (
          <ProductCard key={item.id} product={item} />
        ))}
      </div>
    </div>
  );
};

export default Home;
```