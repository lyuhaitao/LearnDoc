# Django Learn Summarization

# Environment Configuration

- create virtual environment. For example `env_django` 

- Add `xxx\env_django\Library\bin`, `xxx\env_django\scripts`, and `xxx\env_django` to system variable `PATH`

![](Django%20Learn%20Summarization.assets/9b0f68bda07d36feef26c3fe1f4c2b842d366fef.jpg)

# Add Conda interpreter Failure

when adding `conda interpreter` fail, we can directly add `system interpreter`. 

select the `python.exe` in the environment we create.

![](Django%20Learn%20Summarization.assets/6a9c0f8931a77c7a7db42c0b0db59cfb3386e98f.jpg)

# Functions

## DateTimeField

https://www.geeksforgeeks.org/datetimefield-django-models/

```python
from django.db import models
class UserInfo(models.Model):
    createTime = models.DateTimeField(auto_now_add=True)
    updateTime = models.DateTimeField(auto_now=True)
```

- DateTimeField.auto_now

Automatically set the field to now every time the object is saved. Useful for “last-modified” timestamps. Note that the current date is always used; it’s not just a default value that you can override.

- DateTimeField.auto_now_add

Automatically set the field to now when the object is first created. Useful for creation of timestamps. Note that the current date is always used; it’s not just a default value that you can override. So even if you set a value for this field when creating the object, it will be ignored. If you want to be able to modify this field, set the following instead of auto_now_add=True:
