# django-practices-001

## Section 4: Templates & Static Files

### Templates

- Create sub-folder under challenges calls templates/challenges
- Create challenge.html file

<b>How to import html file into views.py</b>

- Using library `django.template.loader import render_to_string`

```python
from django.template.loader import render_to_string
def monthly_challenge(request, month):
    try:
        challenge_text = monthly_challenges[month]
        response_data = render_to_string("challenges/challenge.html")
        return HttpResponse(response_data)
    except:
        return HttpResponseNotFound(response_data)
```

- Register app folder in `settings.py` to make project understand app path

```python
INSTALLED_APPS = [
    "challenges",
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
]
```

- Add folder "challenges" under INSTALLED_APPS, from now project will register challenges folder as app in project.

<b>Noted:</b>
Instead of using library `django.template.loader import render_to_string` we can use shorter way `from django.shortcuts import render`. Example

```python
def monthly_challenge(request, month):
    try:
        challenge_text = monthly_challenges[month]
        return render(request, "challenges/challenge.html")
    except:
        return HttpResponseNotFound("<h1>This is not supported</h1>")
```

- Make dynamic variable display into html

In render method, we add third argument is one dictionary with key (variable to send in html) and value. For example, we make dictionary with 2 keys `text` and `month`

```python
def monthly_challenge(request, month):
    try:
        challenge_text = monthly_challenges[month]
        return render(
            request,
            "challenges/challenge.html",
            {"text": challenge_text, "month": month},
        )
    except:
        return HttpResponseNotFound("<h1>This is not supported</h1>")

```

in HTML we use {{<key_name>}} to call that value

```html
<head>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>{{ month }} Challenge</title>
</head>
<body>
  <h1>This {{ month }}'s Challenge</h1>
  <h2>{{ text }}</h2>
</body>
```

- Filters

Make title is capitalize. We can change it in html file. By using

```html
<title>{{ month | title}} Challenge</title>
```

More information about Filters: https://docs.djangoproject.com/en/4.1/ref/templates/builtins/

- Beautify template (html code) in Django

Install extension Django (from Baptiste Darthenay)

- "For" loop tag in HTML

When we have to send list of value to html display. For example send list of months

In `views.py` we send months list in render method

```python
def index(request):
    list_items = ""
    months = list(monthly_challenges.keys())

    return render(request, "challenges/index.html", {"months": months})
```

In `index.html` we use "for" tag loop to generate list of month in html

```html
<body>
  <ul>
    {% for month in months %}
    <li><a href="/challenges/{{ month }}">{{month| title}}</a></li>
    {% endfor %}
  </ul>
</body>
```

- Using URL tag

Rather than hard code URL `href = "/challenges"` we can use URL tag to call it

```html
  <body>
    <ul>
      {% for month in months %}
      <li><a href="{% url "month-challenge" month %}">{{month| title}}</a></li>
      {% endfor %}
    </ul>
  </body>
```

`month-challenge` we get it from urls.py file

```python
urlpatterns = [
    path("", views.index),  # /challenges?
    path("<int:month>", views.monthly_challenge_by_number),
    path("<str:month>", views.monthly_challenge, name="month-challenge"),
]
```

`if-else` tag is using like this

```html
<body>
  <h1>This {{ month | title}}'s Challenge</h1>
  {% if text is not None %}
  <h2>{{ text }}</h2>
  {% else %}
  <p>There is no challenge for this month yet!</p>
  {% endif %}
</body>
```

- Template Inheritance

Make folder in root project calls `templates` so every app in project can inherit. Create one template call `base.html` which is every page will reuse it

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>{% block page_title %}My Challenge{% endblock %}</title>
  </head>
  <body>
    {% block content %}{% endblock %}
  </body>
</html>
```

Any html want to reuse this `base.html` need to add extends

```html
{% extends 'base.html' %}
```

to use that we need to add blocks in html

```html
  {% block page_title %}
  All Challenges
  {% endblock%}

  {% block content %}
   <ul>
    {% for month in months %}
    <li><a href="{% url "month-challenge" month %}">{{month| title}}</a></li>
    {% endfor %}
   </ul>
  {% endblock %}
```

- Including Partial Template Snippets

Create one html we want to reuse

```html
<header>
  <nav><a href="">All Challenges</a></nav>
</header>
```

To add inside another template we will use `<% include %>`

```html
 {% block content %}
    {% include "challenges/includes/header.html" %}
   <ul>
    {% for month in months %}
    <li><a href="{% url "month-challenge" month %}">{{month| title}}</a></li>
    {% endfor %}
   </ul>
 {% endblock %}
```

- Adding static files

Create subfolder call "static/<app_name>/"

In html file, we can add static file to reuse by

```html
{% block css_files %} <link rel="stylesheet" href="{% static
"challenges/challenges.css" %}"> {% endblock %}
```

Note that if want to apply static file, we should rerun server to apply new code
