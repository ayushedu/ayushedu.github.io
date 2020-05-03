---
layout: post
title:  "Quick Steps to Analytics Dashboard"
date:   2019-02-25 11:04:04
author: Ayush Vatsyayan
categories: Python
tags:	    python
cover:  "/assets/analyst-analytics-blur-106344.jpg"
---

This is an all in one tutorial for creating an analytics dashboard using <a target="_blank" href="https://www.djangoproject.com/">Django</a>, which is a python based web-framework.

We have used <a target="_blank" href="https://getbootstrap.com/">Bootstrap</a> (CSS framework) for frontend, <a target="_blank" href="https://www.highcharts.com/">HighCharts</a> (Javascript charting library) for charts, and <a target="_blank" href="https://www.djangoproject.com/">Django</a> as web-framework

# Create Skeleton Website
#### #1. Install Django

```shell
pip3 install django
```

#### #2. Create project

```shell
django-admin startproject sample_dashboard
```

The `django-admin` tool creates a folder structure as shown below:
```
sample_dashboard/
    manage.py
    sample_dashboard/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```

The sample_dashboard project sub-folder is the entry point for the website:

* **\_\_init\_\_.py** is an empty file that instructs Python to treat this directory as a Python package.
* **settings.py** contains all the website settings. This is where we register any applications we create, the location of our static files, database configuration details, etc.
* **urls.py** defines the site url-to-view mappings. While this could contain all the url mapping code, it is more common to delegate some of the mapping to particular applications, as you'll see later.
* **wsgi.py** is used to help your Django application communicate with the web server. You can treat this as boilerplate.

The manage.py script is used to create applications, work with databases, and start the development web server. 

#### #3. Create application

```shell
python3 manage.py startapp sales 
```
The tool creates a new folder and populates it with files for the different parts of the application. Most of the files contain some minimal boilerplate code for working with the associated objects.

The updated project directory should now look like this:
```
sample_dashboard/
    manage.py
    sample_dashboard/
    sales/
        admin.py
        apps.py
        models.py
        tests.py
        views.py
        __init__.py
        migrations/
```

#### #4. Register the application
Open the project settings file sample_dashboard/sample_dashboard/settings.py and find the definition for the `INSTALLED_APPS` list. Then add a new line at the end of the list, 

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'sales',
]
```

#### #5. Add  url mapper

Open sample_dashboard/sample_dashboard/urls.py and add map the url, along with the static folder.

```python
from django.contrib import admin
from django.urls import include
from django.urls import path
from django.views.generic import RedirectView
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('sales/', include("sales.urls")),
    path('', RedirectView.as_view(url='/sales/', permanent=True)),
] + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

#### #6. Define view in urls.py

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

# Read Data

For the data we have used the [Kaggle's car sales dataset](https://www.kaggle.com/hsinha53/car-sales)
```
Manufacturer,Model,Sales in thousands,4-year resale value,Vehicle type,Price in thousands,Engine size,Horsepower,Wheelbase,Width,Length,Curb weight,Fuel capacity,Fuel efficiency,Latest Launch
Acura        ,Integra          ,16.919,16.36,Passenger,21.5,1.8,140,101.2,67.3,172.4,2.639,13.2,28,2-Feb-14
Acura        ,TL               ,39.384,19.875,Passenger,28.4,3.2,225,108.1,70.3,192.9,3.517,17.2,25,6-Mar-15
Acura        ,CL               ,14.114,18.225,Passenger,.,3.2,225,106.9,70.6,192,3.47,17.2,26,1-Apr-14
Acura        ,RL               ,8.588,29.725,Passenger,42,3.5,210,114.6,71.4,196.6,3.85,18,22,3-Oct-15
Audi         ,A4               ,20.397,22.255,Passenger,23.99,1.8,150,102.6,68.2,178,2.998,16.4,27,10-Aug-15
Audi         ,A6               ,18.78,23.555,Passenger,33.95,2.8,200,108.7,76.1,192,3.561,18.5,22,8-Sep-15
```



#### #7. Define view and read data in it
```
from django.shortcuts import render
import pandas as pd

def index(request):
    """ view function for sales app """

    # read data                                                                                                  
	
    df = pd.read_csv("data/car_sales.csv")
    rs = df.groupby("Engine size")["Sales in thousands"].agg("sum")
    categories = list(rs.index)
    values = list(rs.values)
	
	table_content = df.to_html(index=None)
    table_content = table_content.replace("<thead>","<thead class='thead-dark'>")
    table_content = table_content.replace('class="dataframe"',"class='table table-striped'")
    table_content = table_content.replace('border="1"',"")
	
    context = {"categories": categories, 'values': values, 'table_data':table_content}
    return render(request, 'index.html', context=context)
```

# Design the Dashboard

#### #8. Define the html with charting engine
This is the part where we design the dashboard using Bootstrap and Highcharts.
Bootstrap will design the page skeleton including the chart container, while HighCharts will create the chart using javascript.

After defining the view, we will now be defining the code that presents the information to users. This is how the data flows:
* URL mappers forward the supported URLs (and any information encoded in the URLs) to the appropriate view functions.
* View functions get the requested data from the models, create HTML pages that display the data, and return the pages to the user to view in the browser
* Templates are used when rendering data in the views.


Django will automatically look for templates in a directory named 'templates' in your application. Hence, let's create a new folder named `templates` in sales and create a new file `index.html` in it. The complete index.html is posted below, but before that here are the snippets:
First step is adding the libraries.
* Add Bootstrap css
```html
<!-- Bootstrap core CSS -->                                                                                                                              
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/css/bootstrap.min.css" integrity="sha384-GJzZqFGwb1QTTN6wy59ffF1BuGJpLSa\
9DkKMp0DgiMDm4iYMj70gZWKYbI706tWS" crossorigin="anonymous">
```

* Add Highchart libraries
```html
<script src="https://code.highcharts.com/highcharts.js"></script>
<script src="https://code.highcharts.com/modules/exporting.js"></script>
<script src="https://code.highcharts.com/modules/export-data.js"></script>
```

* Add Bootstrap and other required libraries
```html
<script src="https://code.highcharts.com/highcharts.js"></script>
<script src="https://code.highcharts.com/modules/exporting.js"></script>
<script src="https://code.highcharts.com/modules/export-data.js"></script>
```

Next step is defining the container for chart. This is where charts will be added from the javascript.
```html
<div id="container" style="min-width: 310px; height: 400px; margin: 0 auto" class="border"></div>
```

Finally creating charts in javascript, wherein we are assigning chart values from Django objects `{% raw %}{{categories|safe}}{% endraw %}` and `{% raw %}{{values|safe}}{% endraw %}`. We are also defining chart title, yaxis title, tooltips, and series label.
```javascript
{% raw %}_categories = {{categories|safe}};
_values = {{values|safe}};{% endraw %}
Highcharts.chart('container', {
	chart: {
		type: 'column'
	},
	title: {
		text: 'Sales in Thousand per Engine Capacity'
	},
	subtitle: {
		text: ''
	},
	xAxis: {
		categories:_categories,
		crosshair: true
	},
	yAxis: {
		min: 0,
		title: {
			text: 'Sales in thousands'
		}
	},
	tooltip: {
		headerFormat: '<span style="font-size:10px">{point.key}</span><table>',
		pointFormat: '<tr><td style="color:{series.color};padding:0">{series.name}: </td>' +
			'<td style="padding:0"><b>{point.y:.1f} mm</b></td></tr>',
			footerFormat: '</table>',
		shared: true,
		useHTML: true
	},
	plotOptions: {
		column: {
			pointPadding: 0.2,
			borderWidth: 0
		}
	},
	series: [{
		name: 'Engine Capacity',
		data: _values
		
	}]
});

```

This is how the final HTML will look

```html
<!doctype html>
<html lang="en">
  <head>
    <title>Big Analytics</title>

    <!-- Bootstrap core CSS -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/css/bootstrap.min.css" integrity="sha384-GJzZqFGwb1QTTN6wy59ffF1BuGJpLSa9DkKMp0DgiMDm4iYMj70gZWKYbI706tWS" crossorigin="anonymous">

  </head>
  <body>
    <!-- Top Bar -->
    <nav class="navbar navbar-dark fixed-top bg-dark flex-md-nowrap p-0 shadow">
      <a class="navbar-brand col-sm-3 col-md-2 mr-0" href="#">Big Analytics</a>
      <input class="form-control form-control-dark w-100" type="text" placeholder="Search" aria-label="Search">
      <ul class="navbar-nav px-3">
	<li class="nav-item text-nowrap">
	  <a class="nav-link" href="#">Sign out</a>
	</li>
      </ul>
    </nav>

    <div class="container-fluid">
      <div class="row">

	<!-- Left Pane -->
	<nav class="col-md-2 d-none d-md-block bg-light sidebar">
	  <div class="sidebar-sticky">
            <ul class="nav flex-column">
              <li class="nav-item">
		<a class="nav-link active" href="#">
		  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-home"><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path><polyline points="9 22 9 12 15 12 15 22"></polyline></svg>
		  Dashboard <span class="sr-only">(current)</span>
		</a>
              </li>
              <li class="nav-item">
		<a class="nav-link" href="#">
		  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-users"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path><circle cx="9" cy="7" r="4"></circle><path d="M23 21v-2a4 4 0 0 0-3-3.87"></path><path d="M16 3.13a4 4 0 0 1 0 7.75"></path></svg>
		  Customers
		</a>
              </li>
              <li class="nav-item">
		<a class="nav-link" href="#">
		  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-bar-chart-2"><line x1="18" y1="20" x2="18" y2="10"></line><line x1="12" y1="20" x2="12" y2="4"></line><line x1="6" y1="20" x2="6" y2="14"></line></svg>
		  Reports
		</a>
              </li>
            </ul>
	    
            <h6 class="sidebar-heading d-flex justify-content-between align-items-center px-3 mt-4 mb-1 text-muted">
              <span>Saved reports</span>
              <a class="d-flex align-items-center text-muted" href="#">
		<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-plus-circle"><circle cx="12" cy="12" r="10"></circle><line x1="12" y1="8" x2="12" y2="16"></line><line x1="8" y1="12" x2="16" y2="12"></line></svg>
		  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-home"><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path><polyline points="9 22 9 12 15 12 15 22"></polyline></svg>
		  Dashboard <span class="sr-only">(current)</span>
		</a>
              </li>
              <li class="nav-item">
		<a class="nav-link" href="#">
		  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-users"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path><circle cx="9" cy="7" r="4"></circle><path d="M23 21v-2a4 4 0 0 0-3-3.87"></path><path d="M16 3.13a4 4 0 0 1 0 7.75"></path></svg>
		  Customers
		</a>
              </li>
              <li class="nav-item">
		<a class="nav-link" href="#">
		  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-bar-chart-2"><line x1="18" y1="20" x2="18" y2="10"></line><line x1="12" y1="20" x2="12" y2="4"></line><line x1="6" y1="20" x2="6" y2="14"></line></svg>
		  Reports
		</a>
              </li>
            </ul>
	    
            <h6 class="sidebar-heading d-flex justify-content-between align-items-center px-3 mt-4 mb-1 text-muted">
              <span>Saved reports</span>
              <a class="d-flex align-items-center text-muted" href="#">
		<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-plus-circle"><circle cx="12" cy="12" r="10"></circle><line x1="12" y1="8" x2="12" y2="16"></line><line x1="8" y1="12" x2="16" y2="12"></line></svg>
              </a>
            </h6>
            <ul class="nav flex-column mb-2">
              <li class="nav-item">
		<a class="nav-link" href="#">
		  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file-text"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path><polyline points="14 2 14 8 20 8"></polyline><line x1="16" y1="13" x2="8" y2="13"></line><line x1="16" y1="17" x2="8" y2="17"></line><polyline points="10 9 9 9 8 9"></polyline></svg>
		  Current month
		</a>
              </li>
              <li class="nav-item">
		<a class="nav-link" href="#">
		  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file-text"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path><polyline points="14 2 14 8 20 8"></polyline><line x1="16" y1="13" x2="8" y2="13"></line><line x1="16" y1="17" x2="8" y2="17"></line><polyline points="10 9 9 9 8 9"></polyline></svg>
		  Last quarter
		</a>
              </li>
              <li class="nav-item">
		<a class="nav-link" href="#">
		  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-file-text"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path><polyline points="14 2 14 8 20 8"></polyline><line x1="16" y1="13" x2="8" y2="13"></line><line x1="16" y1="17" x2="8" y2="17"></line><polyline points="10 9 9 9 8 9"></polyline></svg>
		  Year-end sale
<h1 class="h2">Dashboard</h1>
            <div class="btn-toolbar mb-2 mb-md-0">
			              <div class="btn-group mr-2">
<button type="button" class="btn btn-sm btn-outline-secondary">Share</button>
<button type="button" class="btn btn-sm btn-outline-secondary">Export</button>
              </div>
			                <button type="button" class="btn btn-sm btn-outline-secondary dropdown-toggle">
<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-calendar"><rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect><line x1="16" y1="2" x2="16" y2="6"></line><line x1="8" y1="2" x2="8" y2="6"></line><line x1="3" y1="10" x2="21" y2="10"></line></svg>
		This week
              </button>
            </div>
	  </div>

	  <!-- Highcharts display -->
	  <div class="border" id="container" style="min-width: 310px; height: 400px; margin: 0 auto"></div>

	  <!-- Table data -->
	  <h2 class="pt-4">Section Details</h2>
	  <p class="text-danger">This table will be upated by Django objects, hence leaving it as it is.</p>
	  <div class="table-responsive">
	    {% raw %}{{table_data|safe}}{% endraw %}
	  </div>
	</main>
      </div>
    </div>
	
    <!-- Chartjs libraries -->
    <script src="https://code.highcharts.com/highcharts.js"></script>
    <script src="https://code.highcharts.com/modules/exporting.js"></script>
    <script src="https://code.highcharts.com/modules/export-data.js"></script>
    
    <script>
      _categories = {% raw %}{{categories|safe}};
      _values = {{values|safe}};{% endraw %}
      
      Highcharts.chart('container', {
	  chart: {
              type: 'column'
	  },
	  title: {
              text: 'Sales in Thousand per Engine Capacity'
	  },
	  subtitle: {
              text: ')'
	  },
	  xAxis: {
              categories:_categories,
              crosshair: true
	  },
	  yAxis: {
              min: 0,
              title: {
		  text: 'Sales in thousands'
              }
	  },
	  tooltip: {
              headerFormat: '<span style="font-size:10px">{point.key}</span><table>',
              pointFormat: '<tr><td style="color:{series.color};padding:0">{series.name}: </td>' +
		  '<td style="padding:0"><b>{point.y:.1f} mm</b></td></tr>',
              footerFormat: '</table>',
              shared: true,
              useHTML: true
	  },
	  plotOptions: {
              column: {
		  pointPadding: 0.2,
		  borderWidth: 0
        }
	  },
	  series: [{
              name: 'Engine Capacity',
              data: _values
	      
	  }]
      });
      </script>
    
    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.6/umd/popper.min.js" integrity="sha384-wHAiFfRlMFy6i5SRaxvfOCifBUQy1xHdJ/yoi7FRNXMRBu5WHdZYu1hA6ZOblgut" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/js/bootstrap.min.js" integrity="sha384-B0UglyR+jN6CkvvICOB2joaf5I4l3gm9GU6Hc1og6Ls7i6U/mkkaduKaBhlAXv9k" crossorigin="anonymous"></script>


    
  </body>
</html>
```

# Running the Application
```shell
python3 manage.py runserver
```

# Open the link
<a href="http://localhost:8000/sales" target="_blank">http://localhost:8000/sales</a>

![](/assets/django_dashboard.png?raw=true)
