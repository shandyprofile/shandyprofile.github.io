---
title: 'Serving static files in Express'
description: ""
author: [shandy]
date: 2025-09-15
updateDate: 2025-09-15
categories: [(ExpressJS) Server-Side development, Express JS]
tags: [Express JS]
sort_index: 105
# pin: true
# media_subpath: '/posts/02'
---

To serve static files such as images, CSS files, and JavaScript files, use the express.static built-in middleware function in Express.

The function signature is:

```js
express.static(root, [options])
```

- The `root` argument specifies the root directory from which to serve static assets. 
- For more information on the `options` argument. See [express.static](https://expressjs.com/en/5x/api.html#express.static)

For example, use the following code to serve images, CSS files, and JavaScript files in a directory named public:

```
app.use(express.static('public'))
```

Now, you can load the files that are in the **public** directory:

```
http://localhost:3000/images/kitten.jpg
http://localhost:3000/css/style.css
http://localhost:3000/js/app.js
http://localhost:3000/images/bg.png
http://localhost:3000/hello.html
```

> Express looks up the files relative to the static directory, so the name of the static directory is not part of the URL.    

To use multiple static assets directories, call the **express.static** middleware function multiple times:

```js
app.use(express.static('public'))
app.use(express.static('files'))
```

Express looks up the files in the order in which you set the static directories with the **express.static** middleware function.

> Note: For best results, use [a reverse proxy](https://expressjs.com/en/5x/api.html#app.use) cache to improve performance of serving static assets.

To create a virtual path prefix (where the path does not actually exist in the file system) for files that are served by the express.static function, specify a mount path for the static directory, as shown below:

```js
app.use('/static', express.static('public'))
```
Now, you can load the files that are in the **public** directory from the **/static** path prefix.

```
http://localhost:3000/static/images/kitten.jpg
http://localhost:3000/static/css/style.css
http://localhost:3000/static/js/app.js
http://localhost:3000/static/images/bg.png
http://localhost:3000/static/hello.html
```

However, the path that you provide to the **express.static** function is relative to the directory from where you launch your node process. If you run the express app from another directory, it’s safer to use the absolute path of the directory that you want to serve:

```
const path = require('path')
app.use('/static', express.static(path.join(__dirname, 'public')))
```

For more details about the **serve-static** function and its options, see serve-static.