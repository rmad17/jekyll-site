---
layout: post
title: Better API designs in Django
---

This post is of beginner level but requires you to know the basics of django and python. 
Here I am going to talk about how we(our team at my work place) faced some issues and how we resolved it.

### Basic Django API

In django as we all know there is a view(which is in other worlds `Controller`), a model and may or may not be a template to render. There are two types of views, a function based or a class based. However, here we will only talk about class based views as again in my opinion class based views are cleaner. Of course if you are making a view for a small work building class based views are extra effort. Our use case targets building APIs for a medium or large or if you like any size of project :).

For those who are not familiar with class based views, here is a pretty simple example.

```python
from django.http import HttpResponse
from django.views import View
import datetime

def current_datetime(request):
    if not request.method == 'POST':
        return HttpResponse ('invalid request')
    now = datetime.datetime.now()
    html = "<html><body>It is now %s.</body></html>" % now
    return HttpResponse(html)


class CurrentDateTime(View):

    def post(self, request):
        now = datetime.datetime.now()
        html = "<html><body>It is now %s.</body></html>" % now
        return HttpResponse(html)
```
These to views do the same thing. When we declare this in urls.py for class based view we call it like `CurrentDateTime.as_view()`.

### API validations

Now as your code grows more practical, you might be receiving some variables. And with that will come validations.

```python
class Addition(View):

    def post(self, request):
        if not request.POST.get('first_number') and not request.POST.get('second_number'):
            return HttpResponse('invalid request')
        try:
            first_number = request.POST.get('first_number')
            second_number = request.POST.get('second_number')
        except ValueError:
            return HttpResponse('invalid data')
        return HttpResponse(str(first_number + second_number))
```
Here the actual code is just the last line. However for every such API we need to do such repetative lines of code. Now lets say we have to store the data as well

```python
from django.db import IntegrityError
from base.exceptions import ModelOperationError
from .models import Addition

class AddDbIO:

    def create_addition(self, first_number, second_number, result):
        try:
            addition = Addition.get_or_create(
                first_number=first_number, 
                second_number=second_number, result=result)
            addition.save()
            return addition
        except IntegrityError as e:
            raise ModelOperationError(e.args[0])

    def get_addition(self, first_number, second_number, result):
        try:
            addition = Addition.objects.get(
                first_number=first_number, 
                second_number=second_number, result=result)
            return addition
        except Addition.DoesNotExist:
            raise ModelOperationError(e.args[0])


class AddHandler:

    def process_addition(self, first_number, second_number):
        result = first_number + second_number
        return AddDbIO().create_addition(first_number, second_number, result)


class AddView(View):

    def get(self, request):
        if not request.POST.get('first_number') and not request.POST.get('second_number'):
            return HttpResponse('invalid request')
        try:
            first_number = int(request.POST.get('first_number'))
            second_number = int(request.POST.get('second_number'))
        except ValueError:
            return HttpResponse('invalid data')
        addition = AddDbIO().get_addition(first_number, second_number, result)
        return HttpResponse(str(addition.result))

    def post(self, request):
        if not request.POST.get('first_number') and not request.POST.get('second_number'):
            return HttpResponse('invalid request')
        try:
            first_number = int(request.POST.get('first_number'))
            second_number = int(request.POST.get('second_number'))
        except ValueError:
            return HttpResponse('invalid data')
        add_obj = AddHandler().process_addition(first_number, second_number)
        return HttpResponse(add_obj.result)
```
This solves our problem with organization of code.
