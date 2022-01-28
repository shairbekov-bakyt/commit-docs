# Before create any Django project



## Django Web Framework

Django (/ˈdʒæŋɡoʊ/ JANG-goh) is a free and open-source web framework, written in Python, which follows the model-view-template (MVT) architectural pattern. It is maintained by the Django Software Foundation (DSF), an independent organization established as a 501(c)(3) non-profit.

Django's primary goal is to ease the creation of complex, database-driven websites. Django emphasizes reusability and "pluggability" of components, less code, low coupling, rapid development, and the principle of don't repeat yourself. Python is used throughout, even for settings files and data models. Django also provides an optional administrative create, read, update and delete interface that is generated dynamically through introspection and configured via admin models.


# Coding Style

* Put imports in these groups: future, standard library, third-party libraries, other Django components, local Django component, try/excepts. Sort lines in each group alphabetically by the full module name. Place all import module statements before from module import objects in each section. Use absolute imports for other Django components and relative imports for local components.

* On each line, alphabetize the items with the upper case items grouped before the lower case items.

* Break long lines using parentheses and indent continuation lines by 4 spaces. Include a trailing comma after the last import and put the closing parenthesis on its own line.

Use a single blank line between the last import and any module level code, and use two blank lines above the first function or class.

For example (comments are for explanatory purposes only):

```python

# future
from __future__ import unicode_literals

# standard library
import json
from itertools import chain

# third-party
import bcrypt

# Django
from django.http import Http404
from django.http.response import (
    Http404, HttpResponse, HttpResponseNotAllowed, StreamingHttpResponse,
    cookie,
)

# local Django
from .models import LogEntry

# try/except
try:
    import yaml
except ImportError:
    yaml = None

CONSTANT = 'foo'


class Example:
    # ...

```


* Use convenience imports whenever available. For example, do this:

```python

from django.views import View

```

instead of:

```python

from django.views.generic.base import View

```

# Template style

* In Django template code, put one (and only one) space between the curly brackets and the tag contents.

Do this:

```html

{{ foo }}

```

Don’t do this:

```html

{{foo}}

```

# View style

* In Django views, the first parameter in a view function should be called request.


Do this:

```python

def my_view(request, foo):
    # ...

```

Don’t do this:

```python

def my_view(req, foo):
    # ...

```


# Model Style

* Field names should be all lowercase, using underscores instead of camelCase.

Do this:

```python

class Person(models.Model):
    first_name = models.CharField(max_length=20)
    last_name = models.CharField(max_length=40)

```

Don’t do this:

```python

class Person(models.Model):
    FirstName = models.CharField(max_length=20)
    Last_Name = models.CharField(max_length=40)

```

* The class Meta should appear after the fields are defined, with a single blank line separating the fields and the class definition.

Do this:

```python

class Person(models.Model):
    first_name = models.CharField(max_length=20)
    last_name = models.CharField(max_length=40)

    class Meta:
        verbose_name_plural = 'people'

```

Don’t do this:

```python

class Person(models.Model):
    first_name = models.CharField(max_length=20)
    last_name = models.CharField(max_length=40)
    class Meta:
        verbose_name_plural = 'people'

```

Don’t do this, either:

```python

class Person(models.Model):
    class Meta:
        verbose_name_plural = 'people'

    first_name = models.CharField(max_length=20)
    last_name = models.CharField(max_length=40)

```

* The order of model inner classes and standard methods should be as follows (noting that these are not all required):
    * All database fields
    * All database fields
    * Custom manager attributes
    * class Meta
    * def __str__()
    * def save()
    * def get_absolute_url()
    * Any custom methods
    
# Use of django.conf.settings

Modules should not in general use settings stored in django.conf.settings at the top level (i.e. evaluated when the module is imported). The explanation for this is as follows:

Manual configuration of settings (i.e. not relying on the DJANGO_SETTINGS_MODULE environment variable) is allowed and possible as follows:


```python

from django.conf import settings

settings.configure({}, SOME_SETTING='foo')

```   

However, if any setting is accessed before the settings.configure line, this will not work. (Internally, settings is a LazyObject which configures itself automatically when the settings are accessed if it has not already been configured).

So, if there is a module containing some code as follows:

```python

from django.conf import settings
from django.urls import get_callable

default_foo_view = get_callable(settings.FOO_VIEW)

``` 

…then importing this module will cause the settings object to be configured. That means that the ability for third parties to import the module at the top level is incompatible with the ability to configure the settings object manually, or makes it very difficult in some circumstances.

Instead of the above code, a level of laziness or indirection must be used, such as django.utils.functional.LazyObject, django.utils.functional.lazy() or lambda.


