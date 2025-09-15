---
title: 'Episode 10 – CORS & BaaS Integration for Frontend Connections'
description: >-
  Allow cross-origin requests from frontend apps. Explore how Backend-as-a-Service (BaaS) platforms can complement your custom Express backend
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-10
categories: [(ExpressJS) Server-Side development, (ExpressJS) E-Commerce]
tags: [(ExpressJS) E-Commerce]
sort_index: 711
# pin: true
# media_subpath: '/posts/02'
---

## 1. Install CORS Middleware
```bash
npm install cors
```
## 2. Basic CORS Configuration in app.js
```js
const cors = require('cors');
const allowedOrigins = ['http://localhost:3000', 'http://localhost:3001'];

app.use(cors({
  origin: allowedOrigins,
  credentials: true
}));
```
> This enables requests from a specific domain (e.g. your React frontend) and allows cookies (for JWTs stored in cookies).
> Example: React frontend run in http://localhost:3001

## 3. Test Cross-Origin with Frontend
From a frontend React app, try this fetch:

```js
fetch('http://localhost:3000/api/products', {
  method: 'GET',
  credentials: 'include'
})
.then(res => res.json())
.then(data => console.log(data));
```
> Make sure your Express backend is listening on a different port (e.g. 4000), and that cookies are enabled with credentials: 'include'.

## 4. Securing CORS in Production
Never use origin: "*".

Use environment variables for production URLs:

```js
const cors = require('cors');
app.use(cors({
  origin: process.env.FRONTEND_URL,
  credentials: true
}));
```
In .env:

```ini
FRONTEND_URL=http://localhost:3001
```
## 5. Optional: Enable Preflight (OPTIONS) Responses
Express handles this automatically if using cors() middleware. For advanced cases:

```js
app.options('*', cors());
```
## 6. BaaS Integration Overview
If you're exploring Backend-as-a-Service (BaaS) options, here’s how they can work with or extend your backend:

| BaaS            | Use Case Example                         | Integration Method                |
| --------------- | ---------------------------------------- | --------------------------------- |
| **Firebase**    | Auth, Firestore (NoSQL DB), file storage | Firebase SDK / REST API           |
| **Supabase**    | Postgres DB, real-time data, auth        | Supabase client SDK or REST API   |
| **Appwrite**    | Self-hosted auth, DB, file handling      | REST or GraphQL API               |
| **Backendless** | Drag-and-drop API builder                | Auto-generated APIs & mobile SDKs |

You can:
- Use your own Express backend for complex logic (e.g. payment, auth tokens)
- Offload simple features like email auth, image storage, or database to BaaS
- Combine both via REST or GraphQL calls

## 7. Example: Supabase with ReactJS (Option in Frontend)
### 1. Install Supabase Client SDK
In your React project:

```bash
npm install @supabase/supabase-js
```
### 2. Create Supabase Client
In src/supabaseClient.js:

```js
import { createClient } from '@supabase/supabase-js';

const supabaseUrl = process.env.REACT_APP_SUPABASE_URL;
const supabaseKey = process.env.REACT_APP_SUPABASE_KEY;

export const supabase = createClient(supabaseUrl, supabaseKey);
```
> You can get the URL and anon key from the Supabase dashboard under Project Settings → API

### 3. Query Supabase Data in a Component
```jsx
import { useEffect, useState } from 'react';
import { supabase } from './supabaseClient';

function SupabaseProductList() {
  const [products, setProducts] = useState([]);

  useEffect(() => {
    const fetchProducts = async () => {
      const { data, error } = await supabase
        .from('products')
        .select('*');
      
      if (error) {
        console.error('Error fetching products:', error);
      } else {
        setProducts(data);
      }
    };

    fetchProducts();
  }, []);

  return (
    <div>
      <h2>Supabase Products</h2>
      <ul>
        {products.map(p => (
          <li key={p.id}>{p.name} – ${p.price}</li>
        ))}
      </ul>
    </div>
  );
}

export default SupabaseProductList;
```
### 4. Set Your Environment Variables
```Create a .env file in your React root:

ini
REACT_APP_SUPABASE_URL=https://xyzcompany.supabase.co
REACT_APP_SUPABASE_KEY=your-anon-public-api-key
```
> The anon key is safe to use in the frontend, but avoid exposing service keys.
