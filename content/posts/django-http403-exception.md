+++ 
date = 2012-08-09T15:02:28-03:00
title = "Django Http403 Exception"
slug = "django-http403-exception" 
tags = ["django"]
categories = []
description = ""
+++

I really don’t know if what I’m about to write is so obvious that noone needs to look for it on internet; but Django’s (1.4) new [403 handle documentation](https://docs.djangoproject.com/en/dev/topics/http/views/#the-403-http-forbidden-view), doesn’t specify what exception must I raise.

» “If a view results in a 403 exception …”

Just to be clear, I looked for the Http403 exception but I couldn’t find it

So, for all the clueless programmers like me; when they talk about a 403 exception they are actually talking about a ‘PermissionDenied’ exception that you can import from ‘django.core.exceptions’.

UPDATE: Now the documentation has all the information needed!! :D
