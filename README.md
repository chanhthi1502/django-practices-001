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
