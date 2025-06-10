---
title: 'Episode 2: Home Page UI'
description: >-
  In this episode, we'll build the **Home page** of the e-commerce site. We'll create a **product card component**, use **dummy JSON data**, and style everything using **Tailwind CSS**.
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-10
categories: [(ReactJS) Front-End web development, E-Commerce]
tags: [Episode 2 - Home Page UI]
sort_index: 3
# pin: true
# media_subpath: '/posts/02'
---

## 1. Create `Home` Page
Create a new file: `src/pages/Home.jsx`
```jsx
import ProductCard from "../components/ProductCard";
import products from "../data/products.json";

const Home = () => {
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

## 2. Create Product Card Component
Create a file: `src/components/ProductCard.jsx`
```jsx
const ProductCard = ({ product }) => {
  return (
    <div className="border rounded-2xl shadow p-4 hover:shadow-lg transition">
      <img src={product.image} alt={product.title} className="w-full h-48 object-cover rounded-lg" />
      <h2 className="mt-4 text-xl font-semibold">{product.title}</h2>
      <p className="text-gray-700">${product.price}</p>
      <button className="mt-2 w-full bg-blue-600 text-white py-2 rounded hover:bg-blue-700">
        Add to Cart
      </button>
    </div>
  );
};

export default ProductCard;
```
## 3. Add Dummy Product Data
Create folder & file: `src/data/products.json`

```json
[
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
]
```

## 4. Preview Home Page in App.js
Update `App.js` temporarily:

```jsx
import Home from "./pages/Home";

function App() {
  return <Home />;
}

export default App;
```