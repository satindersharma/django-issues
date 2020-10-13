# django-issues


FormView' object has no attribute 'object_list'

the above problem occure whenyou tring CreateView and ListView on a model
But if you import CreateView from edit
```python
from django.views.generic.edit import CreateView
```
and List view from
```python
from django.views.generic import ListView
```

thus above error occur

the simplei solution to this problem is import in the following way
```python
from django.views.generic import CreateView, DeleteView, UpdateView, DetailView, ListView
```

i.e import from generic not generic.edit




# migrate migrations errors
assume your app name is posts

first try:


```bash
python manage.py migrate --fake-initial posts zero
python manage.py migrate posts
```

if above won't work

```bash
delete migrations folder
python manage.py makemigrations posts
python manage.py migrate --fake posts zero
python manage.py migrate posts
```









