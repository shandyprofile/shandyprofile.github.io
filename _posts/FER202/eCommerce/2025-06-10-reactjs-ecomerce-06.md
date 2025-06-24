---
title: 'Episode 5: Cart Page'
description: >-
  Build Cart page, "Add to Cart" button, save cart to JSON-Server
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-18
categories: [(ReactJS) Front-End web development, (ReactJS) E-Commerce]
tags: [(ReactJS) E-Commerce]
sort_index: 6
# pin: true
# media_subpath: '/posts/02'
---

In this episode, we’ll add the ability to:
- Click **"Add to Cart"**
- Send product to `/cart` endpoint in JSON-Server
- Build the **Cart page** to display cart items

---

## 1. Add "Add to Cart" API

Update `ProductCard.jsx` to send POST request when the button is clicked:

```jsx
import axios from "axios";

const ProductCard = ({ product }) => {
  const addToCart = () => {
    const cartItem = {
      ...product,
      quantity: 1
    };

    axios.post("http://localhost:3001/cart", cartItem)
      .then(() => alert("Added to cart!"))
      .catch((err) => console.error(err));
  };

  return (
    <div className="border rounded-2xl shadow p-4 hover:shadow-lg transition">
      <img src={product.image} alt={product.title} className="w-full h-48 object-cover rounded-lg" />
      <h2 className="mt-4 text-xl font-semibold">{product.title}</h2>
      <p className="text-gray-700">${product.price}</p>
      <button
        onClick={addToCart}
        className="mt-2 w-full bg-blue-600 text-white py-2 rounded hover:bg-blue-700"
      >
        Add to Cart
      </button>
    </div>
  );
};

export default ProductCard;
```

## 2. Create Cart Page
Update Cart.jsx to fetch data from /cart and render it.

```jsx
import { useEffect, useState } from "react";
import axios from "axios";

const Cart = () => {
  const [cartItems, setCartItems] = useState([]);

  useEffect(() => {
    axios.get("http://localhost:3001/cart")
      .then((res) => setCartItems(res.data))
      .catch((err) => console.error(err));
  }, []);

  const totalPrice = cartItems.reduce((sum, item) => sum + item.price * item.quantity, 0);

  return (
    <div className="p-4">
      <h1 className="text-3xl font-bold mb-6">Your Cart</h1>
      {cartItems.length === 0 ? (
        <p>Your cart is empty.</p>
      ) : (
        <div className="space-y-4">
          {cartItems.map((item) => (
            <div key={item.id} className="border p-4 rounded-xl flex justify-between">
              <div>
                <h2 className="font-semibold">{item.title}</h2>
                <p>${item.price} x {item.quantity}</p>
              </div>
              <p className="font-bold">${(item.price * item.quantity).toFixed(2)}</p>
            </div>
          ))}
          <div className="text-right font-bold text-xl mt-4">
            Total: ${totalPrice.toFixed(2)}
          </div>
        </div>
      )}
    </div>
  );
};

export default Cart;
```

## 3. Notes
- Cart data is saved in JSON-Server’s `/cart` endpoint
- Product quantity is fixed at 1 for now — we’ll enhance it in Episode 8