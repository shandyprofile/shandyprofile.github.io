---
title: CRUD Project
description: >-
  This project provides a comprehensive system for managing recipe data, including the ability to create, view, edit, and delete recipes. It demonstrates the fundamental operations of a CRUD application for recipe management using Django, typically used in web applications for organizing and maintaining recipe collections.
author: [shandy]
date: 2026-02-26
categories: [Data Sciences, Django]
tags: [Django Tutorial]
sort_index: 302
# pin: true
# media_subpath: '/posts/01'
---

## CRUD Operations In Django

the "Recipe Update" project is a part of a CRUD (Create, Read, Update, Delete) application for managing recipes. Here's a summary of its key functionality:
- **Create**: The project enables users to create new recipes by providing a name, description, and an image upload.
- **Read**: Users can view a list of recipes with details, including names, descriptions, and images. They can also search for recipes using a search form.
- **Update**: Users can update existing recipes by editing their names, descriptions, or images. This functionality is provided through a form that populates with the recipe's current details.
- **Delete**: The project allows users to delete recipes by clicking a "Delete" button associated with each recipe entry in the list.

## Create Django Project and App

- Create the project:

```
django-admin startproject core
cd core
```

- Create the app inside the project:

```
python manage.py startapp recipe
```

- Configure settings.py

  - setting.py: After creating the app we need to register it in 
  - settings.py in the installed_apps section like below

```python
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "recipe",
]
```

### Create Recipe Model

**recipe/models.py**: The code defines a Django model named Recipe that represents recipes. It includes fields for the user who created the recipe, the recipe's name, description, image, and the number of times the recipe has been viewed. The user field is linked to the built-in User model and can be null, allowing for recipes without a specific user.

```python
from django.db import models
from django.contrib.auth.models import User

class Recipe(models.Model):
    user = models.ForeignKey(User, on_delete=models.SET_NULL, null=True, blank=True)
    recipe_name = models.CharField(max_length=100)
    recipe_description = models.TextField()
    recipe_image = models.ImageField(upload_to="recipe")
    recipe_view_count = models.PositiveIntegerField(default=1)

    def __str__(self):
        return self.recipe_name
```

### Create Views

**recipe/views.py:** The code is part of a Django web application for managing recipes. It includes functions for:

1. Displaying and creating recipes, with the ability to filter recipes by name.
2. Deleting a specific recipe.
3. Updating an existing recipe, including the option to change the recipe's image.

These functions are responsible for various recipe-related actions in the application.

```python
from django.shortcuts import render, redirect, get_object_or_404
from .models import Recipe

def recipes(request):
    if request.method == 'POST':
        data = request.POST
        recipe_image = request.FILES.get('recipe_image')
        recipe_name = data.get('recipe_name')
        recipe_description = data.get('recipe_description')

        Recipe.objects.create(
            recipe_image=recipe_image,
            recipe_name=recipe_name,
            recipe_description=recipe_description,
        )
        return redirect('/')

    queryset = Recipe.objects.all()
    if request.GET.get('search'):
        queryset = queryset.filter(recipe_name__icontains=request.GET.get('search'))

    context = {'recipes': queryset}
    return render(request, 'recipes.html', context)


def delete_recipe(request, id):
    recipe = get_object_or_404(Recipe, id=id)
    recipe.delete()
    return redirect('/')


def update_recipe(request, id):
    recipe = get_object_or_404(Recipe, id=id)
    if request.method == 'POST':
        data = request.POST
        recipe_name = data.get('recipe_name')
        recipe_description = data.get('recipe_description')
        recipe_image = request.FILES.get('recipe_image')

        recipe.recipe_name = recipe_name
        recipe.recipe_description = recipe_description
        if recipe_image:
            recipe.recipe_image = recipe_image
        recipe.save()
        return redirect('/')

    context = {'recipe': recipe}
    return render(request, 'update_recipe.html', context)
```

### Register Model in Admin

**recipe/admin.py:** We register the models in admin.py file

```python
from django.contrib import admin
from .models import Recipe

admin.site.register(Recipe)
```

**recipes.html:** First we created the receipes.html file this Django template code is a part of a web page for adding, searching, displaying, updating, and deleting recipes. Here's a simplified explanation.

```html
{% extends "base.html" %}
{% block start %}
 
<link href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" rel="stylesheet">
<style>
  .text{
    color: green;
    font-weight: bold;
  }
</style>
 
<div class="container mt-5">
    
    <form class="col-6 mx-auto card p-3 shadow-lg" method="post" enctype="multipart/form-data">
        {% csrf_token %}
        <h2 class="text  text-center"> ADY201 - CRUD Application </h2>
        <br>
        <h3>Add Recipe</h3>
        <hr>
        <div class="form-group">
          <label for="exampleInputEmail1">Recipe name</label>
          <input name="recipe_name" type="text" class="form-control"  required>
         </div>
        <div class="form-group">
          <label for="exampleInputPassword1"  >Recipe description</label>
          <textarea name="recipe_description" class="form-control" required ></textarea>
        </div>
        <div class="form-group">
            <label for="exampleInputPassword1">Recipe Image</label>
            <input name="recipe_image" type="file" class="form-control" >
          </div>
       
        <button type="submit" class="btn btn-success">Add Recipe</button>
      </form>
 
      <hr>
      <div class="class mt-5">
        <form action="">
 
          <div class="max-auto col-6">
            <div class="form-group">
              <label for="exampleInputEmail1">Search Food</label>
              <input name="search" type="text" class="form-control">
            </div>
            <button type="submit" class="btn btn-primary "> Search</button>
          </form>
        </div>
      </div>
      <table class="table mt-5">
        <thead>
 
          <tr>
            <th scope="col">#</th>
            <th scope="col">Recipe name</th>
            <th scope="col">Recipe Desc</th>
            <th scope="col">Image</th>
            <th scope="col">Actions</th>
          </tr>
        </thead>
        <tbody>
            {% for recipe in recipes %}
          <tr>
            <th scope="row">{{forloop.counter}}</th>
            <td>{{recipe.recipe_name}}</td>
            <td>{{recipe.recipe_description}}</td>
            <td>
                <img src="/media/{{recipe.recipe_image}}" style="height: 100px;"> </td>
                <td>
                    <a href="/delete_recipe/{{recipe.id }}" class="btn btn-danger m-2">Delete</a>
                    <a href="/update_recipe/{{recipe.id }}" class="btn btn-success">Update</a>
                </td>
          </tr>
          {% endfor %}
        </tbody>
      </table>
       
</div>
 
{% endblock %}
```


**update_recipe.html:** This template provides a user-friendly interface for editing the details of a recipe, including its name, description, and image. When a user submits the form, it likely sends the updated data to a Django view for processing and updating the database record.

```
{% extends "base.html" %}
{% block start %}
 
<link href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" rel="stylesheet">
<style>
  .text{
    color: green;
    font-weight: bold;
  }
</style>
 
<div class="container mt-5">
 
  <form class="col-6 mx-auto card p-3 shadow-lg" method="post" enctype="multipart/form-data">
    {% csrf_token %}
    <h2 class="text  text-center"> ADY201 - CRUD Application </h2>
     
    <h3>Update Recipe</h3>
    <hr>
    <div class="form-group">
      <label for="exampleInputEmail1">Recipe name</label>
      <input name="recipe_name" value="{{recipe.recipe_name}}" type="text" class="form-control" required>
    </div>
    <div class="form-group">
      <label for="exampleInputPassword1">Recipe description</label>
      <textarea name="recipe_description" class="form-control" required>{{recipe.recipe_description}}</textarea>
    </div>
    <div class="form-group">
      <label for="exampleInputPassword1">Recipe Image</label>
      <input name="recipe_image" type="file" class="form-control">
    </div>
 
    <button type="submit" class="btn btn-success">Update Recipe</button>
  </form>
 
 
</div>
 
{% endblock %}
```

**base.html:** It is the base HTML file which is extended by all other HTML files.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <title>{{ page|default:"Recipe CRUD" }}</title>
    <link href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" rel="stylesheet" />
    <style>
        table {
            width: 80%;
            margin: 20px auto;
            border-collapse: collapse;
        }
        th, td {
            padding: 10px;
            border: 1px solid #ccc;
            text-align: left;
        }
        th {
            background-color: #f2f2f2;
        }
        tr:nth-child(even) {
            background-color: #f9f9f9;
        }
        tr:hover {
            background-color: #ddd;
        }
    </style>
</head>
<body>
    {% block start %}{% endblock %}
</body>
</html>
```

- **urls.py:** Setting up all the paths for our function.

```python
from django.contrib import admin
from django.urls import path
from recipe import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.recipes),
    path('update_recipe/<id>', views.update_receipe, name='update_recipe'),
   path('delete_recipe/<id>', views.delete_receipe, name='delete_recipe'),
]
```

### Create and Run Migrations

Run these commands to apply the migrations:

```
python manage.py makemigrations
python manage.py migrate
```

Run the server with the help of following command:

```
python manage.py runserver
```