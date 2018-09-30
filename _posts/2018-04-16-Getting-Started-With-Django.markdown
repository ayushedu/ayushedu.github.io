---
layout: post
title:  "Getting Started with Django"
date:   2018-04-16
author: Ayush Vatsyayan
categories: Django
tags:	django
cover:  "/assets/instacode.png"
---

It's been a while I've been using Django, but when I started it wasn't a easy journey.
However, I believe Django is a very easy web framework. 

Initially I was concerned about the look and feel of Django pages, but this can be easily overcome by plugging in the <a href="https://getbootstrap.com">Bootstrap</a>, which is again a great toolkit for building responsive, mobile-first projects on the go.

In this post I will show how to integrate Django with Bootstrap. We will be creating a sample site using bootstrap.
Complete code is available on https://github.com/ayushedu/djangobootstrap/

# Prerequisite
* Python 3.6.x
* Django 2.x

# Getting started
## Creating new Django project
```shell
$ django-admin startproject demo
```

This will create a new directory named ```demo```, which further contains:
* File named ```manage.py```
* Sub-Directory named ```demo```

## Starting a new app

```shell
$ cd demo
$ django-admin startapp starter
```

## Register app
* Register app in ```demo/settings.py```

```
INSTALLED_APPS = [
  'django.contrib.admin',
  'django.contrib.auth',
  'django.contrib.contenttypes',
  'django.contrib.sessions',
  'django.contrib.messages',
  'django.contrib.staticfiles',
  'demo',
  'starter',
]
```

## Update URL
* Update ```demo/urls.py```

```python
from django.conf.urls
import url
from django.contrib
import admin
from django.urls
import include
urlpatterns = [
  url(r '^admin/', admin.site.urls),
  url('^starter/', include('starter.urls')),
]
```

* Create new file ```starter/urls.py```

```python
from django.urls
import path
from.import views
from django.urls
import include
urlpatterns = [
  path('', views.index, name = 'index'),
]
```

## Create HTML template
### Create new directory:
* Directory for base html i.e. html containing common code such as Navbar, etc.
```shell
$ mkdir -p demo/templates/demo/
```

* Directory for starter app template
```shell
$ mkdir -p starter/templates/starter
```

### Creating base HTML file
* The base file ```demo/templates/demo/base.html``` will contain common code
{% highlight django %}
{% raw %}
{% block head %}
{% block title %}My Site{% endblock %}
{% endblock %}
{% block extrahead %}{% endblock %}
{% block onload %}
{% endblock %}
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
   <a class="navbar-brand mb-0 h1">
   <img class="d-inline-block align-top" src="https://getbootstrap.com/assets/brand/bootstrap-solid.svg" alt="" width="30" height="30" />
   My Site</a><button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
   </button>
   <div id="navbarSupportedContent" class="collapse navbar-collapse justify-content-center">
      <ul class="navbar-nav mr-auto">
         <li class="nav-item dropdown">
            <a id="navbarDropdown1" class="nav-link dropdown-toggle" role="button" href="#">
            Profile
            </a>
            <div class="dropdown-menu"><a class="dropdown-item" href="#">Personal Information</a>
               <a class="dropdown-item" href="#">Address</a>
            </div>
         </li>
         <li class="nav-item dropdown">
            <a id="navbarDropdown2" class="nav-link dropdown-toggle" role="button" href="#" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
            Financial
            </a>
            <div class="dropdown-menu"><a class="dropdown-item" href="#">Income</a>
               <a class="dropdown-item" href="#">Online Payment</a>
            </div>
         </li>
         <li class="nav-item dropdown">
            <a id="navbarDropdown" class="nav-link dropdown-toggle" role="button" href="#" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
            Admin
            </a>
            <div class="dropdown-menu">
               <a class="dropdown-item" href="#">Messages</a>
               <a class="dropdown-item" href="#">Events</a>
               <div class="dropdown-divider"></div>
               <a class="dropdown-item" href="#">Settings</a>
            </div>
         </li>
      </ul>
   </div>
   <ul class="navbar-nav px-3">
      <li class="nav-item text-nowrap"><a class="nav-link" href="#">Sign out</a></li>
   </ul>
</nav>
<div>{% block content %}{% endblock %}</div>
{% block js %}
{% endblock js %}
{% endraw %}
{% endhighlight %}

### Creating HTML for app
This file will be exending the base HTML.

{% highlight django %}
{% raw %}
{% extends "demo/base.html" %}
{% block content %}
<h1>Hello, world!</h1>
{% endblock %}
{% endraw %}
{% endhighlight %}


## Create view

Create view for starter app in ```starter/view.py```

```python
from __future__
import unicode_literals
from django.shortcuts
import render

# Create your views here.
def index(request): #Doing nothing here
return render(request, "starter/index.html", context = {}, )
```

## Running the application
```linux
$ python manage.py runserver
```
