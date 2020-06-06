# Django

## Features

- Object-relational mapping (Use object-oriented language to "substitute" SQL language)

---

## Basic file structure

```

```

---

## Models

A model is a single definitive source of information (quite similar to model in MVC model)

### Basics

- Each model is a Python class that subclass `django.db.models.Model`.
- Each attribute of model represents a database field.
- Django will then give an auto-generated API to database.

### Useful related command

`python manage.py makemigration`: It is like "commit" of git. Make a commit of the database model.
`python manage.py migrate`: It is like "push" of git. Actually change the database schema based on the changes.

### Database API

Afterwards, Django would provide a useful python API that allows easy access/modification of the database, e.g.:

```Python
i = SomeItem(field1="xxx",...)
i.save() # Save the object to the database
```

### Representing relationships

#### Many-to-One relation

Consider a case where one reporter has many article (and one article has one reporter).

```python
from django.db import models

class Reporter(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharFiled(max_length=30)
    email = models.EmailField()

class Article(models.Model):
    headline = models.CharField(max_length=100)
    pub_date = models.DateField()
    reporter = models.ForeignKey(Reporter, on_delete=models.CASCADE)

    class Meta:
        ordering = ['headliine']
```

In this case, as `Article` has defined a foreign key, instance of the foreign key model (`Reporter`) will automatically have access to a `Manager` that return all instance of the first model (`Article`), and it named as `FIRSTMODEL_set`, where `FIRSTMODEL` is the name of the first model in **lowercase**.

### Meta

Defines metadata options, e.g.

`ordering`: The default ordering of the model. 
---

## Admin site

It is a automatic admin interface, that reads metadata from the models defined, and allows users to manage content on the site. (It is recommended only for internal use.)

To use this function, simply register is at `admin.py`.

## URL scheme (or routing)

How django works:

1. Django determines the root URLconf to use. Ordinarily, it would be the value of `ROOT_URLCONF`.
2. Django load this mmodule and look for variable `urlpatterns`.
3. Django runs through the url patterns to look for matchs.
4. Once matched, Django imports and calls the given view, which is a Python function. The view get passed the following argument:
    - An instance of `HttpRequest`
    - Positional arguemnt for regex in the match
    - keyword arguments
5. Error handling view if error occurs.

### Path converters

Used to match a more complex pattern.

---

## Views

It is used to serve pages when a particular URL is called. The view file is located is `views.py`. It usually returns Objects from ` django.http`.

### Template

Django also support template. By providing a template in another file, e.g. `template.html`, we can parse it in `views.py` with `django.shortcuts.render`.

## Signals

Django has a "signal dispatcher" what allow decoupled applications to get notified when actions occur elsewhere in the framework.

Layman: `Senders` can notify a set of `receivers`.

### Built-in Django signals

`save()`: 
- `django.db.models.signals.pre_save`
- `django.db.models.signals.post_save`

`delete()`:
- `django.db.models.signals.pre_delete`
- `django.db.models.signals.post_delete`

`django.db.models.signals.m2m_changed`

HTTP request:
- `django.core.signals.request_started`
- `django.core.signals.request_finished`

etc.

### Connecting signals

Suppose there is a signal:
```python
import django.dispatch

pizza_done = django.dispatch.Signal(providing_args=["..."])
```

We can assign receiver function by: 

### With decorative

```python
@receiver(pizza_done)
def my_callback(sender, **kwargs):
    print("Time for delivery!")
```

