---
title: Django Create Project
description: >-
  Django create a new web project.
author: [shandy]
date: 2026-02-26
categories: [Data Sciences, Django]
tags: [Django Tutorial]
sort_index: 302
# pin: true
# media_subpath: '/posts/01'
---

# First Project

Once you have come up with a suitable name for your Django project, like mine: my_tennis_club, navigate to where in the file system you want to store the code (in the virtual environment), I will navigate to the myworld folder, and run this command in the command prompt:

```
django-admin startproject my_tennis_club
```

Django creates a my_tennis_club folder on my computer, with this content:

```
my_tennis_club
    manage.py
    my_tennis_club/
        __init__.py
        asgi.py
        settings.py
        urls.py
        wsgi.py
```

These are all files and folders with a specific meaning, you will learn about some of them later in this tutorial, but for now, it is more important to know that this is the location of your project, and that you can start building applications in it.

## Run the Django Project

Now that you have a Django project, you can run it, and see what it looks like in a browser.

Navigate to the **/my_tennis_club** folder and execute this command in the command prompt:

```
python manage.py runserver
```

Which will produce this result:

![](/assets/img/2026-02-26-12-37-42.png)

Open a new browser window and type **127.0.0.1:8000** in the address bar.

![](/assets/img/2026-02-26-12-38-03.png)

# Create App

I will name my app members.

Start by navigating to the selected location where you want to store the app, in my case the my_tennis_club folder, and run the command below.

If the server is still running, and you are not able to write commands, press [CTRL] [BREAK], or [CTRL] [C] to stop the server and you should be back in the virtual environment.

```
python manage.py startapp members
```

Django creates a folder named members in my project, with this content:

```
my_tennis_club
    manage.py
    my_tennis_club/
    members/
        migrations/
            __init__.py
        __init__.py
        admin.py
        apps.py
        models.py
        tests.py
        views.py
```

These are all files and folders with a specific meaning. You will learn about most of them later in this tutorial.

First, take a look at the file called views.py.

This is where we gather the information we need to send back a proper response.

## 1. Views

Django views are Python functions that take http requests and return http response, like HTML documents.

A web page that uses Django is full of views with different tasks and missions.

Views are usually put in a file called views.py located on your app's folder.

There is a views.py in your members folder that looks like this:

---
**my_tennis_club/members/views.py:**

```python
from django.shortcuts import render

# Create your views here.
```

Find it and open it, and replace the content with this:

```
from django.shortcuts import render
from django.http import HttpResponse

def members(request):
    return HttpResponse("Hello world!")
```

> Note: The name of the view does **not have to be the same** as the application.
>
> I call it **members** because I think it fits well in this context.

## 2. URLs

Create a file named urls.py in the same folder as the views.py file, and type this code in it:

---
**my_tennis_club/members/urls.py:**

```python
from django.urls import path
from . import views

urlpatterns = [
    path('members/', views.members, name='members'),
]
```

The **urls.py** file you just created is specific for the **members** application. We have to do some **routing** in the root **directory my_tennis_club** as well. This may seem complicated, but for now, just follow the instructions below.

There is a file called **urls.py** on the **my_tennis_club** folder, open that file and add the **include** module in the **import** statement, and also add a **path()** function in the urlpatterns[] list, with arguments that will route users that comes in via 127.0.0.1:8000/.

Then your file will look like this:

```python
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('', include('members.urls')),
    path('admin/', admin.site.urls),
]
```

If the server is not running, navigate to the **/my_tennis_club** folder and execute this command in the command prompt:

```
python manage.py runserver
```

In the browser window, type **127.0.0.1:8000/members/** in the address bar.

![](/assets/img/2026-02-26-12-46-28.png)

## 3. Templates

In the Django Intro page, we learned that the result should be in HTML, and it should be created in a template, so let's do that.

Create a templates folder inside the members folder, and create a HTML file named myfirst.html.

The file structure should be like this:

```
my_tennis_club
    manage.py
    my_tennis_club/
    members/
        templates/
            myfirst.html
```

Open the HTML file and insert the following:

---
**my_tennis_club/members/templates/myfirst.html:**

```html
<!DOCTYPE html>
<html>
<body>

<h1>Hello World!</h1>
<p>Welcome to my first Django project!</p>

</body>
</html>
```

---
**Modify the View**

Open the **my_tennis_club/members/views.py:** file in the members folder, and replace its content with this:

```python
from django.http import HttpResponse
from django.template import loader

def members(request):
  template = loader.get_template('myfirst.html')
  return HttpResponse(template.render())
```

---
**Change Settings**

To be able to work with more complicated stuff than "Hello World!", We have to tell Django that a new app is created.

This is done in the settings.py file in the my_tennis_club folder.

Look up **my_tennis_club/settings.py** the **INSTALLED_APPS[]** list and add the members app like this:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'members'
]
```

Then run this command:

```
python manage.py migrate
```

Which will produce this output:

![](/assets/img/2026-02-26-12-51-34.png)

Start the server by navigating to the /my_tennis_club folder and execute this command:

```
python manage.py runserver
```

In the browser window, type 127.0.0.1:8000/members/ in the address bar.

![](/assets/img/2026-02-26-12-51-59.png)

### 4. Models

Up until now in this tutorial, output has been static data from Python or HTML templates.

Now we will see how Django allows us to work with data, without having to change or upload files in the process.

In Django, data is created in objects, called Models, and is actually tables in a database.

#### 4.1. Create Table (Model)

To create a model, navigate to the models.py file in the /members/ folder.

Open **my_tennis_club/members/models.py:**, and add a Member table by creating a Member class, and describe the table fields in it:

```python
from django.db import models

class Member(models.Model):
  firstname = models.CharField(max_length=255)
  lastname = models.CharField(max_length=255)
```

The first field, firstname, is a Text field, and will contain the first name of the members.

The second field, lastname, is also a Text field, with the member's last name.

Both firstname and lastname is set up to have a maximum of 255 characters.

> SQLite Database
> 
> When we created the Django project, we got an empty SQLite database.
> 
> It was created in the my_tennis_club root folder, and has the filename db.sqlite3.
> 
> By default, all Models created in the Django project will be created as tables in this database.

#### 4.2. Migrate

Now when we have described a Model in the models.py file, we must run a command to actually create the table in the database.

Navigate to the /my_tennis_club/ folder and run this command:

```
python manage.py makemigrations members
```

Output:

```
Migrations for 'members':
  members\migrations\0001_initial.py
    - Create model Member

(myworld) C:\Users\Your Name\myworld\my_tennis_club>
```

Django creates a file describing the changes and stores the file in the **/migrations/** folder:

```python
# Generated by Django 5.1.7 on 2025-03-20 11:39

from django.db import migrations, models


class Migration(migrations.Migration):

    initial = True

    dependencies = [
    ]

    operations = [
        migrations.CreateModel(
            name='Member',
            fields=[
                ('id', models.BigAutoField(auto_created=True, primary_key=True, serialize=False, verbose_name='ID')),
                ('firstname', models.CharField(max_length=255)),
                ('lastname', models.CharField(max_length=255)),
            ],
        ),
    ]
```

Note that Django inserts an id field for your tables, which is an auto increment number (first record gets the value 1, the second record 2 etc.), this is the default behavior of Django, you can override it by describing your own id field.

The table is not created yet, you will have to run one more command, then Django will create and execute an SQL statement, based on the content of the new file in the /migrations/ folder.

Run the migrate command:

```
python manage.py migrate
```

Which will result in this output:

![](/assets/img/2026-02-26-12-55-43.png)

> Now you have a Member table in you database!

#### 4.3 Insert Data

**Add Records by command-line**

The Members table created in the previous chapter is empty.

We will use the Python interpreter (Python shell) to add some members to it.

To open a Python shell, type this command:

```
python manage.py shell
```

Now we are in the shell, the result should be something like this:

```
Python 3.13.2 (tags/v3.13.2:4f8bb39, Feb 4 2025, 15:23:48) [MSC v.1942 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
>>>
```

At the bottom, after the three **>>>** write the following:

```
>>> from members.models import Member
```

Hit [enter] and write this to look at the empty Member table:

```
>>> Member.objects.all()
```

This should give you an empty QuerySet object, like this:

```
<QuerySet []>
```

- A QuerySet is a collection of data from a database.
- Read more about QuerySets in the Django QuerySet chapter.
- Add a record to the table, by executing these two lines:

```
>>> member = Member(firstname='Emil', lastname='Refsnes')
>>> member.save()
```

Execute this command to see if the Member table got a member:

```
>>> Member.objects.all().values()
```

Hopefully, the result will look like this:

```
<QuerySet [{'id': 1, 'firstname': 'Emil', 'lastname': 'Refsnes'}]>
```

---
**Add Multiple Records**

```
>>> member1 = Member(firstname='Tobias', lastname='Refsnes')
>>> member2 = Member(firstname='Linus', lastname='Refsnes')
>>> member3 = Member(firstname='Lene', lastname='Refsnes')
>>> member4 = Member(firstname='Stale', lastname='Refsnes')
>>> member5 = Member(firstname='Jane', lastname='Doe')
>>> members_list = [member1, member2, member3, member4, member5]
>>> for x in members_list:
...   x.save()
...
>>>
```

Hit [enter] one more time at the end to exit the for loop.

Now, if you run this command:

```
>>> Member.objects.all().values()
```

you will see that there are 6 members in the Member table:

```
<QuerySet [{'id': 1, 'firstname': 'Emil', 'lastname': 'Refsnes'},
{'id': 2, 'firstname': 'Tobias', 'lastname': 'Refsnes'},
{'id': 3, 'firstname': 'Linus', 'lastname': 'Refsnes'},
{'id': 4, 'firstname': 'Lene', 'lastname': 'Refsnes'},
{'id': 5, 'firstname': 'Stale', 'lastname': 'Refsnes'},
{'id': 6, 'firstname': 'Jane', 'lastname': 'Doe'}]>
```

#### 4.4 Update Records

To update records that are already in the database, we first have to get the record we want to update:

```
>>> from members.models import Member
>>> x = Member.objects.all()[4]
```

x will now represent the member at index 4, which is "Stale Refsnes", but to make sure, let us see if that is correct:

```
>>> x.firstname
```

This should give you this result:

```
'Stale'
```

Now we can change the values of this record:

```
>>> x.firstname = "Stalikken"
>>> x.save()
```

Execute this command to see if the Member table got updated:

```
>>> Member.objects.all().values()
```

Hopefully, the result will look like this:

```
<QuerySet [{'id': 1, 'firstname': 'Emil', 'lastname': 'Refsnes'},
{'id': 2, 'firstname': 'Tobias', 'lastname': 'Refsnes'},
{'id': 3, 'firstname': 'Linus', 'lastname': 'Refsnes'},
{'id': 4, 'firstname': 'Lene', 'lastname': 'Refsnes'},
{'id': 5, 'firstname': 'Stalikken', 'lastname': 'Refsnes'},
{'id': 6, 'firstname': 'Jane', 'lastname': 'Doe'}]>
```

#### 4.5. Delete Records

To delete a record in a table, start by getting the record you want to delete:

```
>>> from members.models import Member
>>> x = Member.objects.all()[5]
```

x will now represent the member at index 5, which is "Jane Doe", but to make sure, let us see if that is correct:

```
>>> x.firstname
```

This should give you this result:

```
'Jane'
```

Now we can delete the record:

```
>>> x.delete()
```

The result will be:

```
(1, {'members.Member': 1})
```

Which tells us how many items were deleted, and from which Model.

If we look at the Member Model, we can see that 'Jane Doe' is removed from the Model:

```
>>> Member.objects.all().values()
<QuerySet [{'id': 1, 'firstname': 'Emil', 'lastname': 'Refsnes'},
{'id': 2, 'firstname': 'Tobias', 'lastname': 'Refsnes'},
{'id': 3, 'firstname': 'Linus', 'lastname': 'Refsnes'},
{'id': 4, 'firstname': 'Lene', 'lastname': 'Refsnes'},
{'id': 5, 'firstname': 'Stalikken', 'lastname': 'Refsnes'}]>
```

#### 4.6 Update Model

**Add Fields in the Model**

To add a field to a table after it is created, open the my_tennis_club/members/models.py file, and make your changes:

```python
from django.db import models

class Member(models.Model):
  firstname = models.CharField(max_length=255)
  lastname = models.CharField(max_length=255)
  phone = models.IntegerField()
  joined_date = models.DateField()
```

As you can see, we want to add phone and joined_date to our Member Model.

This is a change in the Model's structure, and therefore we have to make a migration to tell Django that it has to update the database:

```
python manage.py makemigrations members
```

The command above will result in a prompt, because we try to add fields that are not allowed to be null, to a table that already contains records.

As you can see, Django asks if we want to provide the fields with a specific value, or if we want to stop the migration and fix it in the model:

![](/assets/img/2026-02-26-13-04-38.png)

I will select option 2, and open the **my_tennis_club/members/models.py** file again and allow NULL values for the two new fields:

```python
from django.db import models

class Member(models.Model):
  firstname = models.CharField(max_length=255)
  lastname = models.CharField(max_length=255)
  phone = models.IntegerField(null=True)
  joined_date = models.DateField(null=True)
```

And make the migration once again:

```
python manage.py makemigrations members
```

Which will result in this:

```
Migrations for 'members':
  members\migrations\0002_member_joined_date_member_phone.py
    - Add field joined_date to member
    - Add field phone to member
```

Run the migrate command:

```
python manage.py migrate
```

Which will result in this output:

![](/assets/img/2026-02-26-13-07-33.png)

### 5. Prepare Template
#### 5.1 Create Member Template

After creating Models, with the fields and data we want in them, it is time to display the data in a web page.

Start by creating an HTML file named all_members.html and place it in the /templates/ folder:

```html
<!DOCTYPE html>
<html>
<body>

<h1>Members</h1>
  
<ul>
  {% for x in mymembers %}
    <li>{{ x.firstname }} {{ x.lastname }}</li>
  {% endfor %}
</ul>

</body>
</html>
```

**Modify View**

Next we need to make the model data available in the template. This is done in the view.

In the view we have to import the Member model, and send it to the **my_tennis_club/members/views.py** like this:

```
from django.http import HttpResponse
from django.template import loader
from .models import Member

def members(request):
  mymembers = Member.objects.all().values()
  template = loader.get_template('all_members.html')
  context = {
    'mymembers': mymembers,
  }
  return HttpResponse(template.render(context, request))
```

The members view does the following:
1. Creates a mymembers object with all the values of the Member model.
2. Loads the all_members.html template.
3. Creates an object containing the mymembers object.
4. Sends the object to the template.
5. Outputs the HTML that is rendered by the template.

If you have followed all the steps on your own computer, you can see the result in your own browser:

Start the server by navigating to the /my_tennis_club/ folder and execute this command:

```
python manage.py runserver
```

In the browser window, type 127.0.0.1:8000/members/ in the address bar.

![](/assets/img/2026-02-26-13-11-32.png)

#### 5.2 Add Link to Member Details

The next step in our web page will be to add a Details page, where we can list more details about a specific member.

Start by creating a new template called **my_tennis_club/members/templates/details.html**:

```html
<!DOCTYPE html>
<html>

<body>

<h1>{{ mymember.firstname }} {{ mymember.lastname }}</h1>
  
<p>Phone: {{ mymember.phone }}</p>
<p>Member since: {{ mymember.joined_date }}</p>

<p>Back to <a href="/members">Members</a></p>

</body>
</html>
```

---
**Add Link in all-members Template**

The list in **my_tennis_club/members/templates/all_members.html** should be clickable, and take you to the details page with the ID of the member you clicked on:

```html
<!DOCTYPE html>
<html>
<body>

<h1>Members</h1>
  
<ul>
  {% for x in mymembers %}
    <li><a href="details/{{ x.id }}">{{ x.firstname }} {{ x.lastname }}</a></li>
  {% endfor %}
</ul>

</body>
</html>
```

---
**Create my_tennis_club/members/views.py**

Then create a new view in the views.py file, that will deal with incoming requests to the /details/ url:

```python
from django.http import HttpResponse
from django.template import loader
from .models import Member

def members(request):
  mymembers = Member.objects.all().values()
  template = loader.get_template('all_members.html')
  context = {
    'mymembers': mymembers,
  }
  return HttpResponse(template.render(context, request))
  
def details(request, id):
  mymember = Member.objects.get(id=id)
  template = loader.get_template('details.html')
  context = {
    'mymember': mymember,
  }
  return HttpResponse(template.render(context, request))
```

The details view does the following:
- Gets the id as an argument.
- Uses the id to locate the correct record in the Member table.
- loads the details.html template.
- Creates an object containing the member.
- Sends the object to the template.
- Outputs the HTML that is rendered by the template.

--- 
**Add /details URLs**

Now we need to make sure that the **/details/** url points to the correct view, with id as a parameter.

Open the urls.py file and add the details view to the urlpatterns list.

my_tennis_club/members/urls.py:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('members/', views.members, name='members'),
    path('members/details/<int:id>', views.details, name='details'),
]
```

If you have followed all the steps on your own computer, you can see the result in your own browser: 127.0.0.1:8000/members/.

If the server is down, you have to start it again with the runserver command:

```
python manage.py runserver
```

![](/assets/img/2026-02-26-13-19-35.png)

![](/assets/img/2026-02-26-13-19-46.png)

#### 5.3 Add Master Template

The extends Tag
In the previous pages we created two templates, one for listing all members, and one for details about a member.

The templates have a set of HTML code that are the same for both templates.

Django provides a way of making a "parent template" that you can include in all pages to do the stuff that is the same in all pages.

Start by creating a template called master.html, with all the necessary HTML elements.

my_tennis_club/members/templates/master.html:

```html
<!DOCTYPE html>
<html>
<head>
  <title>{% block title %}{% endblock %}</title>
</head>
<body>

{% block content %}
{% endblock %}

</body>
</html>
```

Do you see Django block Tag inside the <title> element, and the <body> element?

They are placeholders, telling Django to replace this block with content from other sources.

---
**Modify Templates**

Now the two templates (**all_members.html** and **details.html**) can use this master.html template.

This is done by including the master template with the {% extends %} tag, and inserting a title block and a content block:

- my_tennis_club/members/templates/all_members.html:

```html
{% extends "master.html" %}

{% block title %}
  My Tennis Club - List of all members
{% endblock %}


{% block content %}
  <h1>Members</h1>
  
  <ul>
    {% for x in mymembers %}
      <li><a href="details/{{ x.id }}">{{ x.firstname }} {{ x.lastname }}</a></li>
    {% endfor %}
  </ul>
{% endblock %}
```

- my_tennis_club/members/templates/details.html:

```
{% extends "master.html" %}

{% block title %}
  Details about {{ mymember.firstname }} {{ mymember.lastname }}
{% endblock %}


{% block content %}
  <h1>{{ mymember.firstname }} {{ mymember.lastname }}</h1>
  
  <p>Phone {{ mymember.phone }}</p>
  <p>Member since: {{ mymember.joined_date }}</p>
  
  <p>Back to <a href="/members">Members</a></p>
  
{% endblock %}
```

If you have followed all the steps on your own computer, you can see the result in your own browser: 127.0.0.1:8000/members/.

If the server is down, you have to start it again with the runserver command:

```
python manage.py runserver
```

#### 5.4 Add Main Index Page

**Our project needs a main page.**

The main page will be the landing page when someone visits the root folder of the project.

Now, you get an error when visiting the root folder of your project: **127.0.0.1:8000/**

Start by creating a template called main.html:

- my_tennis_club/members/templates/main.html:

```html
{% extends "master.html" %}

{% block title %}
  My Tennis Club
{% endblock %}


{% block content %}
  <h1>My Tennis Club</h1>

  <h3>Members</h3>
  
  <p>Check out all our <a href="members/">members</a></p>
  
{% endblock %}
```

Then create a new view called main, that will deal with incoming requests to root of the project:

- my_tennis_club/members/views.py:

```python
from django.http import HttpResponse
from django.template import loader
from .models import Member

def members(request):
  mymembers = Member.objects.all().values()
  template = loader.get_template('all_members.html')
  context = {
    'mymembers': mymembers,
  }
  return HttpResponse(template.render(context, request))
  
def details(request, id):
  mymember = Member.objects.get(id=id)
  template = loader.get_template('details.html')
  context = {
    'mymember': mymember,
  }
  return HttpResponse(template.render(context, request))
  
def main(request):
  template = loader.get_template('main.html')
  return HttpResponse(template.render())
```

The main view does the following:
- loads the main.html template.
- Outputs the HTML that is rendered by the template.

---
**Add URL**

Now we need to make sure that the root url points to the correct view.

Open the urls.py file and add the main view to the urlpatterns list:

- my_tennis_club/members/urls.py:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.main, name='main'),
    path('members/', views.members, name='members'),
    path('members/details/<int:id>', views.details, name='details'),
]
```

---
**Add Link Back to Main**

The members page is missing a link back to the main page, so let us add that in the all_members.html template, in the content block:

- my_tennis_club/members/templates/all_members.html:

```html
{% extends "master.html" %}

{% block title %}
  My Tennis Club - List of all members
{% endblock %}


{% block content %}

  <p><a href="/">HOME</a></p>

  <h1>Members</h1>
  
  <ul>
    {% for x in mymembers %}
      <li><a href="details/{{ x.id }}">{{ x.firstname }} {{ x.lastname }}</a></li>
    {% endfor %}
  </ul>
{% endblock %}
```

Run server:

```
python manage.py runserver
```

#### 5.5 Page Not Found 404

If you try to access a page that does not exist (a 404 error), Django directs you to a built-in view that handles 404 errors.

You will learn how to customize this 404 view later in this chapter, but first, just try to request a page that does not exist.

In the browser window, type 127.0.0.1:8000/masfdfg/ in the address bar.

You will get one of two results:

- Case 1:
![](/assets/img/2026-02-26-13-30-19.png)

- Case 2:
![](/assets/img/2026-02-26-13-30-38.png)

If you got the first result, you got directed to the built-in Django 404 template.

If you got the second result, then DEBUG is set to True in your settings, and you must set it to False to get directed to the 404 template.

This is done in the settings.py file, which is located in the project folder, in our case the my_tennis_club folder, where you also have to specify the host name from where your project runs from:

Set the debug property to False, and allow the project to run from your local host:

- my_tennis_club/my_tennis_club/settings.py:

```python
.
.
# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = False

ALLOWED_HOSTS = ['*']
.
.
```

> Important: When DEBUG = False, Django requires you to specify the hosts you will allow this Django project to run from.
> 
> In production, this should be replaced with a proper domain name:
> 
> ALLOWED_HOSTS = ['yourdomain.com']

In the browser window, type **127.0.0.1:8000/masfdfg/** in the address bar, and you will get the built-in 404 template:

![](/assets/img/2026-02-26-13-30-19.png)

---
**Customize the 404 Template**

Django will look for a file named 404.html in the templates folder, and display it when there is a 404 error.

If no such file exists, Django shows the "Not Found" that you saw in the example above.

To customize this message, all you have to do is to create a file in the templates folder and name it 404.html, and fill it with whatever you want:

- my_tennis_club/members/templates/404.html:

```html
<!DOCTYPE html>
<html>
<title>Wrong address</title>
<body>

<h1>Ooops!</h1>

<h2>I cannot find the file you requested!</h2>

</body>
</html>
```

In the browser window, type 127.0.0.1:8000/masfdfg/ in the address bar, and you will get the customized 404 template:

![](/assets/img/2026-02-26-13-35-00.png)

> Reset Debug = True
> You should reset the debug property to True to continue with the tutorial.

- my_tennis_club/my_tennis_club/settings.py:

```python
.
.
# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True

ALLOWED_HOSTS = []
.
.
```