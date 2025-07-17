---
title: 'Epoch 11: Monitoring, Logging & CI/CD (Basic)'
description: >-
  In this final epoch, students will enhance observability and maintainability of the microservices system. A simple CI/CD pipeline with GitHub Actions to run tests and auto-deploy Docker images
author: [shandy]
date: 2025-07-17
updateDate: 2025-07-17
categories: [(ExpressJS) Server-Side development, (MicroServices) E-Commerce]
tags: [(MicroServices) E-Commerce]
sort_index: 912
# pin: true
# media_subpath: '/posts/02'
---

## Objectives
- Implement structured logging for debugging and auditing
- Add /health endpoints to track service status
- Automate testing and Docker builds using GitHub Actions
- Understand the basics of CI/CD pipelines in microservice projects

## 1. Add Logging with Winston
Install in each Express-based service:

```bash
npm install winston
```

Then configure a simple logger (src/utils/logger.js):
```js
const { createLogger, format, transports } = require('winston');

const logger = createLogger({
  format: format.combine(
    format.timestamp(),
    format.printf(({ timestamp, level, message }) => {
      return `${timestamp} [${level.toUpperCase()}] ${message}`;
    })
  ),
  transports: [new transports.Console()]
});

module.exports = logger;
```

Use in your controllers or middlewares:
```js
const logger = require('../utils/logger');
logger.info('Product retrieved');
logger.error('Something went wrong');
```
## 2. Add Health Check Endpoints
Add this in every service:
```js
router.get('/health', (req, res) => {
  res.status(200).json({ status: 'UP' });
});
```
> Monitor /health with tools like Prometheus, or just test via curl.

## 3. Setup Basic CI/CD with GitHub Actions
Create a file:
- .github/workflows/docker-ci.yml

```yaml
name: Docker CI/CD

on:
  push:
    branches:
      - main

jobs:
  build-and-test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Node
        uses: actions/setup-node@v4
        with:
          node-version: 18

      - name: Install dependencies
        run: npm install
        working-directory: ./user-service

      - name: Run tests
        run: npm test
        working-directory: ./user-service

  build-docker:
    runs-on: ubuntu-latest
    needs: build-and-test

    steps:
      - uses: actions/checkout@v3

      - name: Log in to Docker Hub
        run: echo "${{ secrets.DOCKER_PASSWORD }}" | docker login -u "${{ secrets.DOCKER_USERNAME }}" --password-stdin

      - name: Build & Push Docker Image
        run: |
          docker build -t yourusername/user-service ./user-service
          docker push yourusername/user-service
```

## 4. Use Environment Variables for Logging Level
In .env:
```env
LOG_LEVEL=info
```

In logger config:

```js
level: process.env.LOG_LEVEL || 'info',
```