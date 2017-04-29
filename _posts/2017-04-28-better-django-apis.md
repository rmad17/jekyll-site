---
layout: post
title: Better API designs in Django
---

This post is of beginner level but requires you to know the basics of django and python. 
Here I am going to talk about how we(our team at my work place) faced some issues and how we resolved it.

# Basic Django API

In django as we all know there is a view(which is in other worlds `Controller`), a model and may or may not be a template to render. There are two types of views, a function based or a class based. However, here we will only talk about class based views as again in my opinion class based views are cleaner. Of course if you are making a view for a small work building class based views are extra effort. Our use case targets building APIs for a medium or large or if you like any size of project :).

For those who are not familiar with class based views, here is a pretty simple example.

```python
from django.http import HttpResponse
from django.views import View
import datetime

def current_datetime(request):
    if not request.method == 'POST':
        return HttpResponse ('Invalid Request')
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


