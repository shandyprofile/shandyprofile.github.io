---
title: 'Episode 10: Final Touches & Deployment'
description: >-
  Add loading/error states, empty cart state, optional deploy (Vercel/etc.)
author: [shandy]
date: 2025-06-10
updateDate: 2025-06-18
categories: [(ReactJS) Front-End web development, (ReactJS) E-Commerce]
tags: [(ReactJS) E-Commerce]
sort_index: 11
# pin: true
# media_subpath: '/posts/02'
---

In this final episode, you will:
- Polish small UX and UI details
- Configure metadata (title, favicon)
- Deploy your app to the web (Vercel, Netlify, GitHub Pages)

---

## Update soon

## 1. Add Page Metadata and Favicon

Edit `public/index.html`:

```html
<title>E-Shop App</title>
<link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
<meta name="description" content="A simple e-commerce app built with React and Tailwind CSS." />
```

## 2. Simplify and Unify Button Styles
In tailwind.config.js, you can define a btn class in @layer for cleaner buttons:

```js
// inside tailwind.config.js or a global css file
@layer components {
  .btn {
    @apply bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700;
  }
}
```

Then use:

```jsx
<button className="btn">Add to Cart</button>
```

## 3. Add a 404 Page (Optional)
Create `src/pages/NotFound.jsx`:

```jsx
const NotFound = () => (
  <div className="text-center mt-20 text-xl">
    <h1 className="text-4xl font-bold mb-4">404 - Page Not Found</h1>
    <p>The page you are looking for does not exist.</p>
  </div>
);

export default NotFound;
```

In `App.js`:
```jsx
<Route path="*" element={<NotFound />} />
```
## 4. Deployment Options
**a) Option 1: Deploy with Vercel**
1. Go to vercel.com
2. Connect your GitHub repo
3. Click "New Project"
4. Import your React app repo
5. Hit Deploy

> Done! Vercel auto detects Create-React-App.

**b) Option 2: Deploy with Netlify**

1. Push your code to GitHub
2. Go to netlify.com
3. New site from Git
4. Choose repo, set build as:

```yaml
Build command: npm run build
Publish directory: build
```

5. Deploy

**c) Option 3: Deploy with GitHub Pages**

Install GitHub Pages package:

```bash
npm install gh-pages --save-dev
```

Update `package.json`:

```json
"homepage": "https://yourusername.github.io/your-repo-name",
"scripts": {
  "predeploy": "npm run build",
  "deploy": "gh-pages -d build"
}
```
Then:

```bash
npm run deploy
```