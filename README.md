# django-issues


FormView' object has no attribute 'object_list'

the above problem occure whenyou tring CreateView and ListView on a model
But if you import CreateView from edit
from django.views.generic.edit import CreateView
and List view from
from django.views.generic import ListView

thus above error occur

the simplei solution to this problem is import in the following way
from django.views.generic import CreateView, DeleteView, UpdateView, DetailView, ListView

i.e import from generic not generic.edit
