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
