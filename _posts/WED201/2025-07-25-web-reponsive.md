---
title: Responsive Layout
description: >-
  
author: [shandy]
date: 2025-06-05
categories: [Web Design, HTML5]
tags: [Responsive Layout]
sort_index: 1
# pin: true
# media_subpath: '/posts/01' 
---
# Epoch 1: Getting Started with Bootstrap

## Objectives
By the end of this session, learners will be able to:
- Understand what Bootstrap is and why it’s used.
- Set up Bootstrap in an HTML project (via CDN or local).
- Identify the structure of a basic Bootstrap HTML file.
- Build a simple grid layout using Bootstrap's 12-column system.
- Use basic utility classes like margin, padding, background, and text color.

## 1. What is Bootstrap?
Bootstrap is a popular open-source CSS framework developed by Twitter for building responsive and mobile-first websites quickly.

### Why Bootstrap?
- Speeds up development
- Mobile-first design out of the box
- Includes responsive grid system
- Provides ready-to-use UI components (buttons, cards, navbars, etc.)
- Rich utility classes (spacing, borders, colors…)

## 2. Setting Up Bootstrap
There are two main ways to include Bootstrap:
- Option 1: Using CDN (Recommended for beginners)
```html
<head>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
```

- Option 2: Local Setup (Advanced use cases)
Download Bootstrap files from https://getbootstrap.com, then link the CSS and JS locally.

## 3. Basic HTML Structure with Bootstrap
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Bootstrap Layout</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" />
</head>
<body>

  <div class="container">
    <h1 class="text-center mt-5">My First Bootstrap Page</h1>
    <div class="row">
      <div class="col bg-primary text-white p-3">Column 1</div>
      <div class="col bg-success text-white p-3">Column 2</div>
      <div class="col bg-danger text-white p-3">Column 3</div>
    </div>
  </div>

</body>
</html>
```
## 4. Introduction to Bootstrap Grid
Bootstrap uses a 12-column grid system to build responsive layouts.
- A row is divided into 12 columns.
- You can split the row into equal or unequal column widths.

Example:

```html
<div class="row">
  <div class="col-4">Width: 4/12</div>
  <div class="col-8">Width: 8/12</div>
</div>
``` 
## 5. Common Utility Classes

| Class                                   | Description                 |
| --------------------------------------- | --------------------------- |
| `bg-primary`, `bg-success`, `bg-danger` | Background color            |
| `text-white`, `text-center`             | Text color and alignment    |
| `p-3`, `m-2`                            | Padding and margin          |
| `border`, `rounded`                     | Borders and rounded corners |

Try combining these to style your layout easily.

---

# Epoch 2: Responsive Grid System and Breakpoints

- Understand the concept of responsive design using breakpoints.
- Use Bootstrap’s responsive grid classes (col-sm-*, col-md-*, etc.).
- Build layouts that adapt to different screen sizes.
- Use visibility/display classes to hide/show elements at different breakpoints.

## 1. What is Responsive Design?
Responsive design ensures your website adapts to different screen sizes — from phones to desktops — without breaking the layout.

Bootstrap is built as mobile-first, which means:

> The default styling applies to mobile (smallest screen), and you scale up with breakpoints.

## 2. Breakpoints in Bootstrap
Bootstrap uses five default breakpoints:

| Breakpoint  | Class Prefix | Min Width |
| ----------- | ------------ | --------- |
| Extra small | `col-`       | `<576px`  |
| Small       | `col-sm-`    | `≥576px`  |
| Medium      | `col-md-`    | `≥768px`  |
| Large       | `col-lg-`    | `≥992px`  |
| Extra large | `col-xl-`    | `≥1200px` |
| XXL         | `col-xxl-`   | `≥1400px` |


You can combine these to make your layout responsive.

## 3. Using Responsive Grid Classes
Example:
```html
<div class="row">
  <div class="col-12 col-md-6 col-lg-4 bg-primary text-white p-3">Column 1</div>
  <div class="col-12 col-md-6 col-lg-4 bg-success text-white p-3">Column 2</div>
  <div class="col-12 col-md-12 col-lg-4 bg-danger text-white p-3">Column 3</div>
</div>
```
> What happens?

- On small screens (<576px): all columns stack vertically (full width).
- On medium screens (≥768px): two columns side by side, third on next row.
- On large screens (≥992px): 3 columns side by side, each 4/12 width.

## 4. Responsive Visibility Utilities
You can show/hide elements depending on the screen size using `d-*` (display) classes.

| Class               | Description                     |
| ------------------- | ------------------------------- |
| `d-none`            | Hide element                    |
| `d-block`           | Display as block                |
| `d-md-none`         | Hide on `md` and above          |
| `d-none d-md-block` | Hide on mobile, show on desktop |

Example:
```html
<p class="d-block d-md-none">Visible on small screens only</p>
<p class="d-none d-md-block">Visible on medium and larger screens</p>
```

## 5. Real-world Practice Example

> Two-column layout on desktop, stacked on mobile
```html
<div class="container my-5">
  <div class="row">
    <div class="col-12 col-md-6 bg-warning p-4">
      <h4>Left Side</h4>
      <p>This appears first, full width on mobile.</p>
    </div>
    <div class="col-12 col-md-6 bg-info text-white p-4">
      <h4>Right Side</h4>
      <p>This appears second, beside left on desktop.</p>
    </div>
  </div>
</div>
```

## Tasks
**Task 1**: 3-column Responsive Layout
- Create a container with 3 columns.
- On xs: full-width stacked
- On md: 2 columns, one below
- On lg: 3 columns side by side

**Task 2:** Show/Hide Promo Message
- Add a `<p>` promo message visible only on mobile using visibility classes.

## Tips
- Think mobile-first: start with col-12, scale up using col-md-*, etc.
- Use spacing utilities like my-3, py-4 to create breathing space.
- Combine bg-* and text-* for quick styling during practice.

---

# Epoch 3: Containers, Nesting & Advanced Layout

- Understand the difference between container, container-fluid, and container-xxl.
- Nest rows and columns inside other columns.
- Use advanced layout tools like offset, order, and align-items.
- Align content and control spacing effectively.

## 1. Containers in Bootstrap
Containers are the outermost wrappers used to center content and provide horizontal padding.

**Types of Containers:**
| Class                 | Behavior                                        |
| --------------------- | ----------------------------------------------- |
| `container`           | Responsive fixed width at each breakpoint       |
| `container-sm/md/...` | Fixed width container for specific screen sizes |
| `container-fluid`     | Full-width container across all screen sizes    |

Example:
```html
<div class="container">
  <!-- Your grid layout here -->
</div>
```

## 2. Nesting Rows & Columns
Nesting allows you to place a new grid inside a column.

Example:
```html
<div class="container">
  <div class="row">
    <div class="col-md-6 bg-light p-3">
      <h5>Left Column</h5>
      <div class="row">
        <div class="col-6 bg-warning">Nested 1</div>
        <div class="col-6 bg-success text-white">Nested 2</div>
      </div>
    </div>
    <div class="col-md-6 bg-secondary text-white p-3">
      <h5>Right Column</h5>
    </div>
  </div>
</div>
```

> Note: Always nest a new row inside a col, not directly inside a container.

## 3. Offsets in Bootstrap
Use `offset-*` classes to move columns to the right by n columns.

Example:
```html
<div class="row">
  <div class="col-md-4 offset-md-4 bg-info text-white text-center p-3">
    Centered Column
  </div>
</div>
```
> This centers a 4-column-wide div by offsetting it 4 columns (4+4+4=12).

## 4. Column Ordering
You can change the order of columns using order-* classes.

| Class        | Description                    |
| ------------ | ------------------------------ |
| `order-1`    | Display first                  |
| `order-2`    | Display second                 |
| `order-md-1` | Order on medium screens and up |

Example:
```html
<div class="row">
  <div class="col-md-6 order-2 order-md-1 bg-warning p-3">First on desktop</div>
  <div class="col-md-6 order-1 order-md-2 bg-success text-white p-3">First on mobile</div>
</div>
```

## 5. Vertical and Horizontal Alignment
Bootstrap uses Flexbox for alignment inside .row or .d-flex containers.

- Horizontal alignment inside a row:
justify-content-start, center, end, between, around, evenly

- Vertical alignment: align-items-start, center, end

Example:
```html
<div class="row align-items-center" style="height: 200px;">
  <div class="col-4 bg-info text-white">Vertically Centered</div>
</div>
```

## Hands-on Tasks
- Task 1: Nested Grid
  - Build a layout with two main columns.
  - Inside the first column, nest another row with two sub-columns.
  - Use background colors to differentiate each section.

- Task 2: Offset + Order
  - Create a centered block using `offset-*`.
  - Try reversing column order on mobile vs desktop using `order-*`.

- Task 3: Alignment
  - Build a row of three columns of different heights.
  - Vertically center the shorter ones using align-items-center.

## Tips
- Use Chrome DevTools (mobile simulator) to test responsive behavior.
- Use `p-*`, `m-*`, and `g-*` to control spacing.
- When nesting, always wrap inner grids in .row.

---

# Epoch 4: Components & Responsive Media

- Use common Bootstrap components (Navbar, Card, Button, Alert, etc.).
- Make images and videos responsive.
- Use utility classes (d-flex, flex-wrap, text-center, etc.) to enhance layouts.
- Understand how components adapt across screen sizes.

## 1. Bootstrap Components Overview
Bootstrap offers ready-made UI components that work well with the grid system and are responsive by default.

**Commonly Used Components:**

| Component | Class                                 |
| --------- | ------------------------------------- |
| Navbar    | `navbar`, `navbar-expand-*`           |
| Buttons   | `btn`, `btn-primary`, `btn-outline-*` |
| Card      | `card`, `card-body`, `card-img-top`   |
| Alert     | `alert`, `alert-success`, etc.        |
| Collapse  | `collapse`, `accordion`               |
| Modal     | `modal` (optional for later sessions) |


## 2. Navbar (Responsive Navigation)
Bootstrap Navbars collapse into a hamburger menu on small screens.

Example:
```html
<nav class="navbar navbar-expand-md navbar-dark bg-dark">
  <div class="container">
    <a class="navbar-brand" href="#">MySite</a>
    <button class="navbar-toggler" data-bs-toggle="collapse" data-bs-target="#mainmenu">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="mainmenu">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item"><a class="nav-link" href="#">Home</a></li>
        <li class="nav-item"><a class="nav-link" href="#">About</a></li>
      </ul>
    </div>
  </div>
</nav>
```

> Notes:
- Use navbar-expand-md to collapse only below medium screens.
- Use ms-auto to align items right.

## 3. Cards (Reusable content blocks)
Example:
```html
<div class="card" style="width: 18rem;">
  <img src="https://via.placeholder.com/300x200" class="card-img-top img-fluid" alt="...">
  <div class="card-body">
    <h5 class="card-title">Card Title</h5>
    <p class="card-text">A short description goes here.</p>
    <a href="#" class="btn btn-primary">Read More</a>
  </div>
</div>
```

> Use img-fluid for responsive images inside cards.

## 4. Buttons & Alerts
**Buttons:**
```html
<button class="btn btn-primary">Primary</button>
<button class="btn btn-outline-success">Success</button>
```

```**Alerts:**
html
<div class="alert alert-warning" role="alert">
  This is a warning alert—check it out!
</div>
```

## 5. Responsive Media (Images & Videos)
- Responsive Images

```html
<img src="https://via.placeholder.com/600x300" class="img-fluid rounded" alt="Responsive Image">
```

| Class           | Purpose                              |
| --------------- | ------------------------------------ |
| `img-fluid`     | Makes image scale with its parent    |
| `rounded`       | Applies border-radius                |
| `img-thumbnail` | Adds border and padding around image |


- Responsive Video Embed
Use .ratio utility to make embedded videos scale correctly.

```html
<div class="ratio ratio-16x9">
  <iframe src="https://www.youtube.com/embed/dQw4w9WgXcQ" allowfullscreen></iframe>
</div>
```

> Supported ratios: 1x1, 4x3, 16x9, 21x9.

## 6. Flexbox Utilities for Layout Enhancements
Bootstrap uses Flexbox under the hood. You can apply utilities directly:

Example:
```html
<div class="d-flex justify-content-between flex-wrap">
  <div class="p-2 bg-primary text-white">Box 1</div>
  <div class="p-2 bg-secondary text-white">Box 2</div>
</div>
```

| Class                     | Effect                      |
| ------------------------- | --------------------------- |
| `d-flex`                  | Display as flex container   |
| `justify-content-between` | Space between children      |
| `flex-wrap`               | Allow wrapping to new lines |
| `align-items-center`      | Vertical alignment          |


## Hands-on Tasks
- Task 1: Build a Responsive Navbar
  - Use navbar-expand-md and navbar-toggler for mobile behavior.
  - Include at least 2 links and a logo/title.

- Task 2: Create a Card Gallery
  - Place 3 cards side-by-side on desktop, stacked on mobile.
  - Each card contains an image, title, description, and a button.

- Task 3: Embed a Responsive Video
  - Use .ratio class to embed a YouTube video that scales with screen.

## Tips & Best Practices
- Combine row, col, and card for flexible content sections.
- Always add img-fluid to images inside a column or card.
- Test components across mobile/tablet/desktop using DevTools.

---

# Epoch 5: Building a Responsive Landing Page
- Combine containers, rows, columns, and components into a full-page layout.
- Structure a typical landing page with a header, content, and footer.
- Use responsive techniques to adapt layout across devices.
- Apply spacing, alignment, and component styling best practices.

## 1. What is a Landing Page?
A landing page is a standalone web page with focused content, often used for:

- Product introductions
- Campaigns
- Portfolios
- Signups or contact prompts

It typically includes:
- Hero/Header section
- Features/Info sections
- CTA (Call to Action)
- Footer

## 2. Suggested Layout Structure
```html
<body>
  <!-- Navbar -->
  <nav>...</nav>

  <!-- Hero Section -->
  <section class="hero">...</section>

  <!-- Features Section -->
  <section class="features">...</section>

  <!-- CTA Section -->
  <section class="cta">...</section>

  <!-- Footer -->
  <footer>...</footer>
</body>
```

> Each section uses Bootstrap’s container > row > col layout pattern.

## 3. Hero Section (Header Banner)

```html
<section class="bg-light text-center py-5">
  <div class="container">
    <h1 class="display-4">Welcome to My Site</h1>
    <p class="lead">Clean, fast, and responsive design with Bootstrap 5.</p>
    <a href="#" class="btn btn-primary btn-lg mt-3">Get Started</a>
  </div>
</section>
```

**Tips:**
- Use display-* classes for large headlines.
- btn-lg and mt-3 create visual emphasis.

## 4. Features Section (Grid Layout)
```html
<section class="py-5">
  <div class="container">
    <div class="row text-center">
      <div class="col-md-4 mb-4">
        <div class="card h-100">
          <div class="card-body">
            <h5 class="card-title">Fast</h5>
            <p class="card-text">Optimized for performance and speed.</p>
          </div>
        </div>
      </div>
      <div class="col-md-4 mb-4">
        <div class="card h-100">
          <div class="card-body">
            <h5 class="card-title">Responsive</h5>
            <p class="card-text">Looks great on all devices.</p>
          </div>
        </div>
      </div>
      <div class="col-md-4 mb-4">
        <div class="card h-100">
          <div class="card-body">
            <h5 class="card-title">Simple</h5>
            <p class="card-text">Built with clean, reusable code.</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>
```

**Notes:**
- Use h-100 inside card to ensure equal height.
- mb-4 adds vertical spacing on smaller screens.

## 5. Call to Action (CTA)
```html
<section class="bg-primary text-white text-center py-5">
  <div class="container">
    <h2 class="mb-4">Ready to get started?</h2>
    <a href="#" class="btn btn-light btn-lg">Join Now</a>
  </div>
</section>
```
> This section contrasts with the background and focuses on user conversion.

## 6. Footer
```html
<footer class="bg-dark text-white text-center py-3">
  <p class="mb-0">&copy; 2025 My Website. All rights reserved.</p>
</footer>
```
> `mb-0` removes unnecessary spacing at the bottom of paragraph.

## Tasks
- Task 1: Build Your Own Landing Page Structure a responsive single-page site with:
  - A navbar
  - A hero header with headline + button
  - A 3-column features section using cards
  - A CTA section with a large button
  - A footer

- Task 2: Customize and Improve
  - Try different breakpoints (e.g., stack features at sm).
  - Add icons using Bootstrap Icons.
  - Use container-fluid for full-width sections.

## Tips
- Use consistent vertical spacing (py-5, mb-4, etc.).
- Stack columns on smaller devices by default (col-12, then col-md-4).
- Use text-center in combination with container to center content.
- Keep contrast strong in CTA sections.

---
# Mini Project (Bootstrap Responsive Page)

- By the end of this session, learners will be able to:
- Build a full responsive webpage from scratch using Bootstrap 5.
- Apply layout, components, and utilities in a real-world structure.
- Review and reflect on key Bootstrap techniques.
- Present and explain their layout choices.

## 1. Project Objective: Build a Responsive Portfolio Website
**Project Theme:**
"Create a personal portfolio landing page showcasing your skills, projects, and contact information."

## 2. Project Requirements
The page must include the following sections:

| Section      | Description                                                            |
| ------------ | ---------------------------------------------------------------------- |
| **Navbar**   | Sticky or scrollable with name/logo and navigation links               |
| **Hero**     | Full-width intro section with name, title, CTA button                  |
| **About**    | 2-column section: text + profile image                                 |
| **Projects** | Card grid layout (at least 2–3 project cards with title, desc, button) |
| **Contact**  | Simple contact info or embedded form (can be fake)                     |
| **Footer**   | Social media links or copyright notice                                 |

## 3. Technical Requirements
- Use container, row, and col-* consistently
- Navbar should be responsive with a toggle menu
- Use at least 2 Bootstrap components (e.g., cards, buttons, alerts)
- All images must be responsive (img-fluid)
- Page must look good on mobile, tablet, and desktop
- Include spacing utilities (py-*, mb-*, etc.)
- Optional: Add Bootstrap Icons

## 4. Suggested Folder Structure
```bash
project-folder/
├── index.html
├── /img (store profile and project images)
└── /css (optional custom styles)
└── /js (optional code behind)
```

## 5. Project Rubric for Grading

| **Category**                        | **Criteria**                                                                 | **Points** |
| ----------------------------------- | ---------------------------------------------------------------------------- | ---------- |
| **1. Layout & Grid Usage**          | Correct use of `container`, `row`, and `col-*`; consistent responsive layout | 10 pts     |
| **2. Responsiveness**               | Layout adapts correctly on mobile, tablet, and desktop devices               | 10 pts     |
| **3. Navbar**                       | Responsive navbar with toggle on smaller screens                             | 10 pts     |
| **4. Components Integration**       | Uses at least 2 Bootstrap components (cards, buttons, alert, etc.)           | 10 pts     |
| **5. Visual Design & Spacing**      | Use of `py-*`, `mb-*`, `g-*`, and alignment classes for clean design         | 10 pts     |
| **6. Image & Media Responsiveness** | Images/videos are responsive using `img-fluid`, `ratio`, etc.                | 10 pts     |
| **7. Content Structure**            | Clear sections: Hero, About, Projects, Contact, Footer                       | 10 pts     |
| **8. Code Quality & Organization**  | Clean, readable code; proper indentation; file structure                     | 10 pts     |
| **9. Creativity & Customization**   | Visual creativity, custom styling, icons, etc.                               | 10 pts     |
| **10. Presentation (if required)**  | Able to explain layout, decisions, responsiveness (optional)                 | 10 pts     |

## 6. Template Folder Structure (Bootstrap Project)
```
bootstrap-portfolio/
│
├── index.html                # Main HTML file
├── README.md                 # (Optional) Brief project description
│
├── /css
│   └── style.css             # Optional: custom CSS overrides
│
├── /img
│   ├── profile.jpg           # Your photo for About section
│   ├── project1.jpg          # Project images for cards
│   └── ...                   # Additional images
│
├── /js
│   └── script.js             # (Optional) custom JavaScript
│
└── /fonts or /icons          # (Optional) Bootstrap Icons or Google Fonts
```

**CDN Recommendations (add to index.html)**
```html
<!-- Bootstrap 5 CSS -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" />

<!-- Bootstrap Icons (Optional) -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.10.5/font/bootstrap-icons.css" rel="stylesheet">

<!-- Bootstrap 5 JS (for Navbar toggle, etc.) -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
```

## Landing Page Template

https://www.figma.com/community/website-templates/landing-pages