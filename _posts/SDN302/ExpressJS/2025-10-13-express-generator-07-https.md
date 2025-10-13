---
title: 'HTTPS'
description: ""
author: [shandy]
date: 2025-10-12
updateDate: 
categories: [(ExpressJS) Server-Side development, Express JS]
tags: [Express JS]
sort_index: 107
# pin: true
# media_subpath: '/posts/02'
---

## Introduction to HTTPS in Node.js Express

HTTPS (Hypertext Transfer Protocol Secure) is an essential protocol for securing data in transit between clients and servers in web applications. It ensures that the data exchanged is encrypted and cannot be intercepted or tampered with by unauthorized parties. In this article, we will dive deep into how to set up HTTPS in a Node.js Express application, providing a secure and robust environment for your users.

## Prerequisites 

To follow this guide, you should have:

- A basic understanding of Node.js and Express
- Node.js (version 10 or later) installed on your system
- A text editor like Visual Studio Code, Sublime Text, or Atom

## Generating SSL Certificates

To enable HTTPS, you need a public and private key pair, which are contained in an SSL certificate. You can either obtain a certificate from a Certificate Authority (CA) like Let's Encrypt, or generate a self-signed certificate for development purposes.

## Self-Signed Certificates

Using OpenSSL, you can create a self-signed certificate for local development:

```
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365
```

This command generates two files:
- key.pem: The private key
- cert.pem: The public certificate

> Note: Self-signed certificates should not be used in production environments, as they will trigger browser security warnings. Instead, use a trusted CA for production certificates.

## Configuring Express to Use HTTPS
Now that you have the SSL certificate, let's configure the Node.js Express application to use HTTPS.

### 1. Create a New Express Application
First, create a new directory for your project and navigate into it:

``` 
mkdir nodejs-express-https
cd nodejs-express-https
```

Initialize the project with the default settings:

```
npm init -y
```

Install Express:

```
npm install express
```

### 2. Set Up the Express Server
Create a new file named app.js in the project directory and add the following code:

```js
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(port, () => {
  console.log(`Example app listening at http://localhost:${port}`);
});
```

This code sets up a basic Express server that listens on port 3000 and responds with "Hello World!" when accessed via the root URL.

### 3. Configure HTTPS
To configure HTTPS, you need to import the https module and use it to create a secure server. Update app.js with the following code:

```js
const fs = require('fs');
const https = require('https');
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello World!');
});

// Read the SSL certificate files
const privateKey = fs.readFileSync('key.pem', 'utf8');
const certificate = fs.readFileSync('cert.pem', 'utf8');

// Create a credentials object
const credentials = { key: privateKey, cert: certificate };

// Create an HTTPS service with the Express app and the credentials
const httpsServer = https.createServer(credentials, app);

// Start the HTTPS server
httpsServer.listen(port, () => {
  console.log(`Example app listening at https://localhost:${port}`);
});
```

This code imports the fs and https modules, reads the SSL certificate files, creates a credentials object, and starts an HTTPS server with the Express app and the credentials.

Now, your Node.js Express application is configured to use HTTPS. When you start the application, it will listen for secure connections on port 3000.

### 4. Test the HTTPS Server
To test your HTTPS server, run the following command:

```
node app.js
```

You should see the following output:

```
Example app listening at https://localhost:3000
```

Open your web browser and navigate to https://localhost:3000. You may encounter a security warning because of the self-signed certificate. Proceed with caution, and you should see the "Hello World!" message displayed.

## Redirecting HTTP Traffic to HTTPS (Optional)

If you want to redirect all HTTP traffic to HTTPS, you can create an additional HTTP server that forwards requests to the HTTPS server. Update app.js with the following code:

```js
const http = require('http');
const fs = require('fs');
const https = require('https');
const express = require('express');
const app = express();
const httpPort = 3001;
const httpsPort = 3000;

app.get('/', (req, res) => {
  res.send('Hello World!');
});

// Read the SSL certificate files
const privateKey = fs.readFileSync('key.pem', 'utf8');
const certificate = fs.readFileSync('cert.pem', 'utf8');

// Create a credentials object
const credentials = { key: privateKey, cert: certificate };

// Create an HTTPS service with the Express app and the credentials
const httpsServer = https.createServer(credentials, app);

// Start the HTTPS server
httpsServer.listen(httpsPort, () => {
  console.log(`Example app listening at https://localhost:${httpsPort}`);
});

// Create an HTTP server that redirects to the HTTPS server
const httpApp = express();
httpApp.use((req, res, next) => {
  res.redirect(`https://${req.headers.host}${req.url}`);
});

const httpServer = http.createServer(httpApp);

// Start the HTTP server
httpServer.listen(httpPort, () => {
  console.log(`HTTP server redirecting to HTTPS at http://localhost:${httpPort}`);
});
```

This code imports the http module, creates an HTTP server that redirects to the HTTPS server, and listens for connections on port 3001. Now, when users access your application via HTTP, they will be redirected to the HTTPS version.