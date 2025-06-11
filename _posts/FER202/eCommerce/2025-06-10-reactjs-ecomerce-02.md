---
title: 'Episode 1: Project Setup'
description: >-
  In this episode, we'll set up the foundation of our e-commerce app using React and Tailwind CSS.
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-10
categories: [(ReactJS) Front-End web development, (ReactJS) E-Commerce]
tags: [(ReactJS) E-Commerce]
sort_index: 2
# pin: true
# media_subpath: '/posts/02'
---

## 1. Initialize React App

Install library `create-react-app` into root NodeJS.
``` bash
npm install create-reate-app -g
```
Create your project using Create React App:
```bash
npx create-react-app ecommerce-app
cd ecommerce-app
```

## 2. Install Tailwind CSS:
Install Tailwind and its required dependencies:

```bash
npm install -D tailwindcss@3.4.1 postcss autoprefixer
npx tailwindcss init -p
```

## 3. Configure Tailwind
Update `tailwind.config.js`:

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./src/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

Update `src/index.css`:
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

## 4. Clean Up Default Files
Remove unnecessary files: Delete `logo.svg` and contents of `App.css`

Replace `App.js` with a simple starter component:
```jsx
function App() {
  return (
    <div className="text-2xl text-center mt-10">
      Welcome to E-Commerce App!
    </div>
  );
}

export default App;
```

## 5. Suggested Architech Structure
```uml
ecommerce-app/
├── public/
├── src/
│   ├── assets/         # Images, icons, etc.
│   ├── components/     # Shared UI components
│   ├── pages/          # Home, Cart, Login pages
│   ├── services/       # API calls (e.g., productService.js)
│   ├── utils/          # Helper functions
│   ├── App.js
│   ├── index.css
│   └── main.jsx
```

## 6. Install Router & Axios
We'll need these libraries for routing and API requests:

```bash
npm install react-router-dom axios
```
