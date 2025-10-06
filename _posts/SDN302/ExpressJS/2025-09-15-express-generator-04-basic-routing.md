---
title: 'Basic routing'
description: ""
author: [shandy]
date: 2025-09-15
updateDate: 2025-09-15
categories: [(ExpressJS) Server-Side development, Express JS]
tags: [Express JS]
sort_index: 104
# pin: true
# media_subpath: '/posts/02'
---

***Routing*** refers to determining how an application responds to a client request to a particular endpoint, which is a URI (or path) and a specific HTTP request method (GET, POST, and so on).

Each route can have one or more handler functions, which are executed when the route is matched.

Route definition takes the following structure:

```
app.METHOD(PATH, HANDLER)
```

Where:
- `app` is an instance of **express**.
- `METHOD` is an HTTP request method, in lowercase.
- `PATH` is a path on the server.
- `HANDLER` is the function executed when the route is matched.

> This tutorial assumes that an instance of **express** named **app** is created and the server is running. If you are not familiar with creating an app and starting it. See the [Hello world example](https://shandyprofile.github.io/post/2025-09-15-express-generator-01-installing).

The following examples illustrate defining simple routes.

Respond with **Hello World!** on the homepage:

```js
app.get('/', (req, res) => {
  res.send('Hello World!')
})
```

Respond to a POST request on the root route (/), the application’s home page:

```js
app.post('/', (req, res) => {
  res.send('Got a POST request')
})
```

Respond to a PUT request to the /user route:

```js
app.put('/user', (req, res) => {
  res.send('Got a PUT request at /user')
})
```

Respond to a DELETE request to the /user route:

```js
app.delete('/user', (req, res) => {
  res.send('Got a DELETE request at /user')
})
```
For more details about routing, see the routing guide.