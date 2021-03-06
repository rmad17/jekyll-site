---
layout: post
title: How I Automated My Django Settings Configuration
---


Past couple of months I had to create a lot of django projects. I generally add a .gitignore, local\_settings.py and local\_settings\_sample.py like many of you. 
For those not familiar with the `local_settings.py` concept, to tell you briefly, I move out my secret keys and settings that I do not want people to find in my git repo to a file called local\_settings.py and gitignore it. I also add a local\_settings\_sample.py to add the key names but without the actual key(adding empty strings). This file gets commited and whoever downloads the repo knows what keys he/she requires to recreate local\_settings.py locally.

My `.gitignore` generally looks something like this:

```viml
*.pyc
*.swp
*.sqlite3
local_settings.py
*_settings.py
```
The first one is straightforward. The second one is for my files opened in vim. The third one is something I do not generally need very often but still as a precaution to accidentally commiting someday I add it. Git ignoring local\_settings.py is something many of you do. The last one is one of my own ideas. The idea is say you have two django apps. Both are using some API services for which you need some kind of key which is not required globally. I then tend to create a file called `<app_name>_settings.py`. We can always keep everything in `local_settings.py` but this is how I prefer doing it.

Coming to my `local_settings.py`, I create a very basic version initially. 

```python
#SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = ')k@=hw&9_iywf#st7xg)k@=hw&nlj#6pxu!2f759nlj#6pxu!2f75'
#SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True
#Database
#https://docs.djangoproject.com/en/1.10/ref/settings/#databases
DATABASES = {
   'default': {
       'ENGINE': 'django.db.backends.postgresql_psycopg2',
       'NAME': 'generic',
       'USER': 'postgresuser',
       'PASSWORD': 'password',
       'HOST': '127.0.0.1',
       'PORT': '5432',
       'TEST': {
           'NAME': 'generictest',
       },
   }
}
```

So as I was creating a new django project every few week, I grew irritated with the fact I have to create these files everytime. I did a quick lookup for any such tool that existed to automate these simple steps. I didn't find any and decided to create my own. 

I wrote a python script that created a .gitignore with the following:

```python
# Django #
*.log
*.pot
*.pyc
__pycache__/
local_settings.py
db.sqlite3
media
# Ignore all local_settings #
*_settings.py
# Linux #
*.swp
```
It created a local\_settings.py and a local\_settings\_sample.py with database settings. It also copied the SECRET\_KEY from settings.py to local\_settings.py. And finally it added this to the end of settings.py

```python
try:
    from .local_settings import SECRET_KEY, DATABASES, DEBUG  # noqa
except ImportError as e:
    print('Error:', e.msg)
```

The `noqa` removes flake-8 warnings in case the line is greater than 79 chars.

I decided to make it more like a cli tool with arguments rather than just a script so that other people facing simmilar issues can take advantage of this tool as well as probably add some more stuff to it to make it even better. I used the really awesome [click](http://click.pocoo.org/5/) library and bingo! 
I decided to call it `django-initiate`. 
So now all I needed to do was `pip install django-initiate` and then run `initiate <awesome_project_name>` from my project directory. And I have a basic settings figured out. It is to be noted however that to set the database settings with preferred config you have to provide optional arguments everytime. Probably we could add a config.json sometime. Or maybe you have a better idea. Create issues at [https://github.com/rmad17/djanjo-initiate]() or mail to `souravbasu17[at]gmail[dot]com`.
