---
title: "Gradients and optimization"
description: >-
  Mermaid demo
author: [shandy]
date: 2025-09-05
categories: [Machine Learning, Linear Algebra for Machine Learning]
tags: [Machine Learning, Linear Algebra for Machine Learning]
sort_index: 111
# pin: true
# media_subpath: '/posts/01'
---

## Update Soon

```python
import numpy as np
import matplotlib.pyplot as plt

# ---------------------------
# 1. Tạo dữ liệu giả lập: 2 lớp (chó = 0, mèo = 1)
# ---------------------------
np.random.seed(42)
n_samples = 100

# mèo (label = 1)
x_cat = np.random.normal(loc=[2, 2], scale=0.5, size=(n_samples, 2))
y_cat = np.ones((n_samples, 1))

# chó (label = 0)
x_dog = np.random.normal(loc=[0, 0], scale=0.5, size=(n_samples, 2))
y_dog = np.zeros((n_samples, 1))

# Gộp dữ liệu
X = np.vstack((x_cat, x_dog))
y = np.vstack((y_cat, y_dog))

# ---------------------------
# 2. Hàm sigmoid & loss
# ---------------------------
def sigmoid(z):
    return 1 / (1 + np.exp(-z))

def loss(y_true, y_pred):
    eps = 1e-8
    return -np.mean(y_true * np.log(y_pred + eps) + (1 - y_true) * np.log(1 - y_pred + eps))

# ---------------------------
# 3. Huấn luyện Logistic Regression bằng Gradient Descent
# ---------------------------
lr = 0.1      # learning rate
epochs = 50   # số vòng lặp

# Khởi tạo tham số ngẫu nhiên
w = np.random.randn(2, 1)
b = 0
losses = []

for epoch in range(epochs):
    # Forward
    z = X @ w + b
    y_pred = sigmoid(z)
    L = loss(y, y_pred)
    losses.append(L)
    
    # Gradient
    dz = y_pred - y
    dw = (X.T @ dz) / len(y)
    db = np.mean(dz)
    
    # Update tham số
    w -= lr * dw
    b -= lr * db

# ---------------------------
# 4. Vẽ Loss theo Epoch
# ---------------------------
plt.figure(figsize=(7,5))
plt.plot(losses, marker='o')
plt.title("Loss giảm dần theo Epoch (Logistic Regression chó vs mèo)")
plt.xlabel("Epoch")
plt.ylabel("Loss")
plt.grid(True)
plt.show()

# ---------------------------
# 5. Vẽ Decision Boundary
# ---------------------------
plt.figure(figsize=(7,5))

# Vẽ dữ liệu
plt.scatter(x_cat[:,0], x_cat[:,1], color="blue", label="Mèo (1)")
plt.scatter(x_dog[:,0], x_dog[:,1], color="red", label="Chó (0)")

# Vẽ đường phân cách
x_min, x_max = X[:,0].min()-1, X[:,0].max()+1
y_min, y_max = X[:,1].min()-1, X[:,1].max()+1
xx, yy = np.meshgrid(np.linspace(x_min, x_max, 200),
                     np.linspace(y_min, y_max, 200))

Z = sigmoid(np.c_[xx.ravel(), yy.ravel()] @ w + b)
Z = Z.reshape(xx.shape)

# contour: đường boundary tại ngưỡng 0.5
plt.contour(xx, yy, Z, levels=[0.5], colors="green", linewidths=2)

plt.title("Decision Boundary của Logistic Regression")
plt.xlabel("Feature 1")
plt.ylabel("Feature 2")
plt.legend()
plt.grid(True)
plt.show()
```