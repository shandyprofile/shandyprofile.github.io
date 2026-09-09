---
title: 'Episode 8: Quantity Update and Remove Items from Cart'
description: >-
  Quantity Update and Remove Items from Cart – you will upgrade your cart page with 2 new features:
  </br>- Increase/decrease product quantity
  </br>- Remove products from cart
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-18
categories: [(ReactJS) Front-End web development, (ReactJS) E-Commerce]
tags: [(ReactJS) E-Commerce]
sort_index: 9
# pin: true
# media_subpath: '/posts/02'
---

In this episode, you'll:
- Add "+" and "−" buttons to update quantity
- Allow user to remove items from cart
- Sync all changes with `JSON-Server`

---

## 1. Update Cart Page with Quantity Controls

Modify `src/pages/Cart.jsx`:

```jsx
import { useEffect, useState } from "react";
import axios from "axios";

const Cart = () => {
  const [cartItems, setCartItems] = useState([]);

  const fetchCart = () => {
    axios.get("http://localhost:3001/cart")
      .then((res) => setCartItems(res.data))
      .catch((err) => console.error(err));
  };

  useEffect(() => {
    fetchCart();
  }, []);

  const updateQuantity = (id, newQty) => {
    if (newQty < 1) return;

    const item = cartItems.find(item => item.id === id);
    axios.patch(`http://localhost:3001/cart/${id}`, {
      quantity: newQty
    }).then(fetchCart);
  };

  const removeItem = (id) => {
    axios.delete(`http://localhost:3001/cart/${id}`)
      .then(fetchCart);
  };

  const totalPrice = cartItems.reduce((sum, item) => sum + item.price * item.quantity, 0);

  return (
    <div className="p-4">
      <h1 className="text-3xl font-bold mb-6">Your Cart</h1>
      {cartItems.length === 0 ? (
        <p>Your cart is empty.</p>
      ) : (
        <div className="space-y-4">
          {cartItems.map((item) => (
            <div key={item.id} className="border p-4 rounded-xl flex justify-between items-center">
              <div>
                <h2 className="font-semibold">{item.title}</h2>
                <p>${item.price}</p>
                <div className="flex items-center gap-2 mt-2">
                  <button
                    className="bg-gray-300 px-2 rounded"
                    onClick={() => updateQuantity(item.id, item.quantity - 1)}
                  >−</button>
                  <span>{item.quantity}</span>
                  <button
                    className="bg-gray-300 px-2 rounded"
                    onClick={() => updateQuantity(item.id, item.quantity + 1)}
                  >+</button>
                </div>
              </div>
              <div className="text-right">
                <p className="font-bold">${(item.price * item.quantity).toFixed(2)}</p>
                <button
                  onClick={() => removeItem(item.id)}
                  className="text-red-500 mt-2 hover:underline"
                >
                  Remove
                </button>
              </div>
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

## 2. Explanation

| Function         | What it Does                                         |
| ---------------- | ---------------------------------------------------- |
| `updateQuantity` | Updates quantity via `PATCH /cart/:id`               |
| `removeItem`     | Deletes item via `DELETE /cart/:id`                  |
| `fetchCart`      | Refetches updated cart from server after each change |
