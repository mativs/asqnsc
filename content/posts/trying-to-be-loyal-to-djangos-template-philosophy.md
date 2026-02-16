+++ 
date = 2012-05-24T14:50:55-03:00
title = "Trying to be loyal to django's template philosophy"
slug = "trying-to-be-loyal-to-djangos-template-philosophy" 
tags = ['django']
categories = []
description = ""
+++

I’ve reached the following dilemma.

“Model instance with methods that depends on logged user” VS “Django <a href="https://docs.djangoproject.com/en/1.3/topics/templates/#accessing-method-calls" target="_blank">dislike</a> to call a method with parameters in it’s template framework”

I agree with them (while wondering who cares)

So, what to do?

- Messing with the view (as suggested). The problem is that sometimes I have to access the elements of a query and that would mean iterating through all the elements to update it’s status.
- Messing with the model. Add a ‘user’ variable to each model. (prediction: same problem from above)
- Messing with the template. This is the [best solution](http://od-eon.com/blogs/liviu/calling-instance-methods-inside-django-templates/) I’ve have found but it’s not django’s way.

I’ve found [MY solution](https://gist.github.com/2781616), although I’m not so sure about its elegance.

Usage:

    {{ instance|with_user:user|call_method:"my_method_name" }}