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


First and formost:
delete migration folder
and migrate by this command
```bash
python manage.py migrate --run-syncdb erpapp
```

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





<form action="#" can cause error to the NextUrlMixins

as it redirect to /# instead of /


so remove has in the form action of the page.


##### 'str' object is not a mapping
pass name as named argument
i.e
path('abc/',Abc.as_view(),name='abc'),
instead of
path('abc/',Abc.as_view(),'abc'),
