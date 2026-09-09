---
title: "Epoch 1: Building Project"
description: >-
  Mermaid demo
author: [shandy]
date: 2025-10-08
categories: [(.Net) Microservice Architectures, E Shopping]
tags: [CNN, (PBL) Dog and cat classification]
sort_index: 101
# pin: true
# media_subpath: '/posts/01'
---

## Epoch 1 – Create the First Microservice with .NET CLI

### Business Story

ABC Company is building a new **EShopping** platform using a **Microservices Architecture**.

The Backend team has been assigned to develop the first microservice: **Product Service**, which is responsible for managing product catalogs and inventory.

The Tech Lead defines the following requirements:

- Use **.NET 8**
- Create the project using **ASP.NET Core Web API**
- Initialize everything using the **.NET CLI**
- The project must build and run successfully
- Database integration is **not required** in this epoch

---

### Learning Objectives

After completing this epoch, students will be able to:

- Install and verify the .NET SDK
- Use the .NET CLI
- Create a Solution
- Create an ASP.NET Core Web API project
- Add projects to a Solution
- Build and run applications
- Understand the basic structure of an ASP.NET Core project

---

### What is the .NET CLI?

The **.NET CLI (Command Line Interface)** is the official command-line tool provided by Microsoft for developing .NET applications.

It allows developers to:

- Create projects
- Build applications
- Run applications
- Publish applications
- Execute tests
- Install NuGet packages
- Generate Entity Framework migrations

> In professional software development, projects are often initialized using the .NET CLI instead of Visual Studio.

---

### Step 1. Verify the Installed .NET SDK

Check the installed SDK version.

```bash
dotnet --version
```

Example output:

```text
8.0.412
```

List all installed SDKs.

```bash
dotnet --list-sdks
```

---

### Step 2. Create the Project Workspace

```bash
mkdir EShopping
cd EShopping
```

Verify the current working directory.

```bash
pwd
```

---

### Step 3. Create the Solution

```bash
dotnet new sln -n EShopping
```

Expected output:

```text
EShopping.sln
```

---

### Step 4. Create the Project Structure

```
EShopping
├── src
├── tests
├── docs
└── docker
```

Create the directories.

```bash
mkdir src
mkdir tests
mkdir docs
mkdir docker
```

---

### Step 5. Create the Product Service

Move into the **src** directory.

```bash
cd src
```

Create an ASP.NET Core Web API project.

```bash
dotnet new webapi -n ProductService
```

The resulting structure:

```
src
└── ProductService
```

---

### Step 6. Return to the Solution Root

```bash
cd ..
```

---

### Step 7. Add the Project to the Solution

```bash
dotnet sln add src/ProductService/ProductService.csproj
```

Verify the solution.

```bash
dotnet sln list
```

Expected output:

```text
Project(s)
----------
src/ProductService/ProductService.csproj
```

---

### Step 8. Build the Solution

```bash
dotnet build
```

Expected output:

```text
Build succeeded.
```

---

### Step 9. Run the Application

#### Option 1

```bash
cd src/ProductService

dotnet run
```

#### Option 2

```bash
dotnet run --project src/ProductService
```

Example output:

```text
Now listening on:

http://localhost:5000

https://localhost:5001
```

---

### Step 10. Open Swagger

Open one of the following URLs in your browser:

```
https://localhost:5001/swagger
```

or

```
http://localhost:5000/swagger
```

If the Swagger UI appears successfully, the project has been created correctly.

---

### Step 11. Understand the Project Structure

```
ProductService
│
├── Program.cs
├── appsettings.json
├── appsettings.Development.json
├── ProductService.csproj
├── Properties
└── obj
```

| File/Folder | Description |
|-------------|-------------|
| Program.cs | The application's entry point. Configures dependency injection, middleware, and routing. |
| appsettings.json | Stores application configuration such as logging, connection strings, and other settings. |
| appsettings.Development.json | Overrides configuration when running in the Development environment. |
| ProductService.csproj | Defines the project, target framework, package references, and build configuration. |
| Properties/launchSettings.json | Contains launch profiles for local development. |
| obj | Auto-generated build artifacts. Do not modify manually. |

---

### Step 12. Enable Hot Reload

Run the application with Hot Reload enabled.

```bash
dotnet watch
```

or

```bash
dotnet watch run
```

The application will automatically rebuild whenever a source file is saved.

---

### Project Structure After Epoch 1

```
EShopping
│
├── docker
├── docs
├── tests
├── src
│   └── ProductService
│       ├── Program.cs
│       ├── appsettings.json
│       ├── ProductService.csproj
│       └── Properties
│
└── EShopping.sln
```

---

### Hands-on Exercise

Complete the following tasks:

1. Create an `EShopping` solution.
2. Create the folders:
   - `src`
   - `tests`
   - `docs`
   - `docker`
3. Create a `ProductService` project using the .NET CLI.
4. Add the project to the solution.
5. Build the solution successfully.
6. Run the application.
7. Open Swagger and verify that the default endpoint is accessible.

---
### Next Epoch Preview

In **Epoch 2**, we will refactor the generated Web API project into a **Clean Architecture** by separating it into multiple projects:

- ProductService.API
- ProductService.Application
- ProductService.Domain
- ProductService.Infrastructure

Students will learn why this layered architecture is widely adopted in enterprise-grade Microservices systems.