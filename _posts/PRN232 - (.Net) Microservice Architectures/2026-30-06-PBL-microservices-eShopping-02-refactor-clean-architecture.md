---
title: "Epoch 2: Refactor to Clean Architecture"
description: >-
  Mermaid demo
author: [shandy]
date: 2025-10-08
categories: [(.Net) Microservice Architectures, E Shopping]
tags: [CNN, (PBL) Dog and cat classification]
sort_index: 102
# pin: true
# media_subpath: '/posts/01'
---

## Epoch 2 – Refactor to Clean Architecture

### Business Story

The initial Product Service has been successfully created as a single ASP.NET Core Web API project.

As the project grows, placing all source code inside a single project will make it difficult to maintain, test, and scale.

The architecture team has decided to refactor the service into a **Clean Architecture**, separating responsibilities into multiple projects.

---

## Learning Objectives

After completing this epoch, students will be able to:

- Understand the principles of Clean Architecture.
- Create multiple class library projects using the .NET CLI.
- Organize a solution into layers.
- Configure project references.
- Apply the Dependency Rule.
- Build a scalable foundation for future development.

---

## Target Architecture

```
ProductService

                +----------------------+
                |   ProductService.API |
                +----------+-----------+
                           |
                           v
                +----------------------+
                | ProductService.Application |
                +----------+-----------+
                           |
                           v
                +----------------------+
                | ProductService.Domain |
                +----------------------+

                           ^
                           |
                +----------------------+
                | ProductService.Infrastructure |
                +----------------------+
```

---

## Responsibilities

| Project | Responsibility |
|----------|----------------|
| API | HTTP endpoints, middleware, dependency injection |
| Application | Use cases, DTOs, interfaces, validation |
| Domain | Business entities, business rules |
| Infrastructure | Database, repositories, external services |

---

## Step 1. Remove the Default Web API Project

Move to the **src** folder.

```bash
cd src
```

Rename the existing project.

```bash
mv ProductService ProductService.API
```

Rename the project file.

```bash
mv ProductService.API/ProductService.csproj ProductService.API/ProductService.API.csproj
```

---

## Step 2. Remove the Old Project from the Solution

Return to the solution folder.

```bash
cd ..
```

Remove the old project.

```bash
dotnet sln remove src/ProductService.API/ProductService.API.csproj
```

---

## Step 3. Create the Remaining Projects

Move into **src**.

```bash
cd src
```

Create the Application project.

```bash
dotnet new classlib -n ProductService.Application
```

Create the Domain project.

```bash
dotnet new classlib -n ProductService.Domain
```

Create the Infrastructure project.

```bash
dotnet new classlib -n ProductService.Infrastructure
```

---

## Step 4. Add All Projects to the Solution

Return to the solution root.

```bash
cd ..
```

```bash
dotnet sln add src/ProductService.API/ProductService.API.csproj

dotnet sln add src/ProductService.Application/ProductService.Application.csproj

dotnet sln add src/ProductService.Domain/ProductService.Domain.csproj

dotnet sln add src/ProductService.Infrastructure/ProductService.Infrastructure.csproj
```

Verify:

```bash
dotnet sln list
```

Expected output:

```
src/ProductService.API/ProductService.API.csproj
src/ProductService.Application/ProductService.Application.csproj
src/ProductService.Domain/ProductService.Domain.csproj
src/ProductService.Infrastructure/ProductService.Infrastructure.csproj
```

---

## Step 5. Configure Project References

### API

```bash
dotnet add src/ProductService.API/ProductService.API.csproj reference src/ProductService.Application/ProductService.Application.csproj
```

```bash
dotnet add src/ProductService.API/ProductService.API.csproj reference src/ProductService.Infrastructure/ProductService.Infrastructure.csproj
```

---

### Application

```bash
dotnet add src/ProductService.Application/ProductService.Application.csproj reference src/ProductService.Domain/ProductService.Domain.csproj
```

---

### Infrastructure

```bash
dotnet add src/ProductService.Infrastructure/ProductService.Infrastructure.csproj reference src/ProductService.Application/ProductService.Application.csproj
```

```bash
dotnet add src/ProductService.Infrastructure/ProductService.Infrastructure.csproj reference src/ProductService.Domain/ProductService.Domain.csproj
```

---

## Dependency Graph

```
               API
              /   \
             /     \
    Application   Infrastructure
           \          /
            \        /
              Domain
```

The Domain project does **not** reference any other project.

---

## Step 6. Build the Solution

```bash
dotnet build
```

Expected output:

```
Build succeeded.
```

---

## Step 7. Verify the Folder Structure

```
src

├── ProductService.API
├── ProductService.Application
├── ProductService.Domain
└── ProductService.Infrastructure
```

---

## Why Use Clean Architecture?

### Without Clean Architecture

```
Controllers
Database
Business Logic
Validation
Repositories
DTOs

↓

Everything inside one project
```

Problems:

- Hard to maintain
- Difficult to test
- Strong coupling
- Low reusability

---

### With Clean Architecture

```
Presentation

↓

Application

↓

Domain

↑

Infrastructure
```

Benefits:

- Clear separation of responsibilities
- Easier unit testing
- Better scalability
- Easier maintenance
- Suitable for enterprise applications

---

## Hands-on Exercise

Complete the following tasks:

- Rename the Web API project to `ProductService.API`
- Create three class library projects
- Add all projects to the solution
- Configure project references
- Build the solution successfully

---

## Definition of Done

By the end of this epoch, students should have:

- ✅ Four independent projects
- ✅ Correct project references
- ✅ Successfully built solution
- ✅ Understanding of Clean Architecture
- ✅ Ready for Domain modeling