+++ 
date = 2018-10-31T16:22:44-03:00
title = "Testing Django Middlewares"
slug = "testing-django-middlewares" 
tags = ['django']
categories = []
thumbnail = "images/tn.png"
description = ""
+++


I have reached the dilemma that [unit testing](https://medium.com/@adamdonaghy/unit-testing-django-middleware-2e8cb26e06ca) my middlewares in django doesn't seem enough so I came up with a different approach.

What I want is to test the same request object as my django views receives it. 

This is my ugly solution, please forgive me.

### Step 1: Create an Ugly Test Url
```python
if settings.DEBUG:
    urlpatterns += [
        url(r'^test/', test, name='test'),
    ] 
```

### Setp 2: Create an even uglier Test View
You must return an `HttpResponse` object with a `req` attribute containing the actual request. Keep in mind that the attribute `request` already exists and that's why I am using `req`

```python
def test(request):
    response = HttpResponse('Testing')
    response.req = request
    return response
```

### Step 3: Test your middleware
Call the endpoint `/test/` with the django client class and assert the `response.req` object. For example ...

```python
@pytest.mark.django_db
def test_company(client, default_user, company_user, administrator, salesman, site_settings):
    # testing anonymous
    site_settings.price_visibility = SiteSettings.ANONYMOUS
    site_settings.save()
    response = client.get('/test/', follow=True)
    assert response.req.hide_prices is False

    # testing logged user
    site_settings.price_visibility = SiteSettings.LOGGED
    site_settings.save()
    response = client.get('/test/', follow=True)
    assert response.req.hide_prices is True

    client.login(username=default_user.email, password='password')
    response = client.get('/test/', follow=True)
    assert response.req.hide_prices is False
    client.logout()

    # testing user with company
    site_settings.price_visibility = SiteSettings.WITH_CLIENT
    site_settings.save()
    response = client.get('/test/', follow=True)
    
    assert response.req.hide_prices is True
    client.login(username=default_user.email, password='password')
    response = client.get('/test/', follow=True)
    
    assert response.req.hide_prices is True
    client.logout()
    client.login(username=company_user.email, password='password')
    response = client.get('/test/', follow=True)
    assert response.req.hide_prices is False
    client.logout()

    ...
```

## The problem

For example, at work I have to write a middleware with something like this 

```python
class HidePricesMiddleware(object):
    def process_request(self, request):
        request_visibility = SiteSettings.ADMINISTRATOR
        if not request.user.is_authenticated():
            request_visibility = SiteSettings.ANONYMOUS
        elif request.user.is_staff:
            request_visibility = SiteSettings.ADMINISTRATOR
        elif not request.user.company:
            request_visibility = SiteSettings.LOGGED
        elif not request.user.company.show_prices:
            request_visibility = SiteSettings.WITH_CLIENT
        else:
            request_visibility = SiteSettings.APPROVED

        request.hide_prices = request_visibility < request.site_settings.price_visibility
```  

When I try to test this middleware by unit testing it I started mocking all posible cases and after some time it felt very uncomfortable (IMHO) because I end up writing the test to match the code. In the previous example testing was way more natural for me.

Hope it helps.