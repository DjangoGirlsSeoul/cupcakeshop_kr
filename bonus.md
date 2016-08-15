# Bonus

Django is a powerful and sophisticated backend. Which means it can be used for a backend of apps and services. For example Instagram has the biggest deployment of Django as their backend. 

In this tutorial, we are going to use Django REST Framework to add API endpoints to our model and use that API to make a simple Android app.

### Installing REST framework

```bash
$ pip install djangorestframework
```

Add `rest_framework` to `INSTALLED_APPS` in settings.py

```python
INSTALLED_APPS = (
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'menu',
    'accounts',
    'rest_framework',
)
```

Add default Rest Framework Renderer in settings.py

```python

REST_FRAMEWORK = {
    'DEFAULT_RENDERER_CLASSES': (
        'rest_framework.renderers.JSONRenderer',
    )
}
```

### Creating a Serializer class


> The first thing we need to get started on our Web API is to provide a way of serializing and deserializing the snippet instances into representations such as json

Create a file in menu directory named `serializers.py` and add the following content.

```python

from rest_framework import serializers
from .models import Cupcake

class CupcakeSerializer(serializers.ModelSerializer):
    class Meta:
        model = Cupcake
        fields = ('name', 'rating', 'price', 'image', 'writer','createdAt')

```

### Django Views using Serializer

Add a url for cupcake list in `menu/urls.py`

```python
    url(r'^api/v1/cupcakes/$', views.cupcakes_list),

```

Add the following code in `menu.views.py` import part.

```python
from django.http import HttpResponse
from django.views.decorators.csrf import csrf_exempt
from rest_framework.renderers import JSONRenderer
from rest_framework.parsers import JSONParser
from rest_framework.decorators import api_view
from rest_framework.response import Response
from rest_framework import status
from .serializers import CupcakeSerializer
```

and view function 

```python

@api_view(['GET'])
def cupcakes_list(request):
    """
    REST Api V1
    List all cupcakes
    """
    if request.method == 'GET':
        cakes = Cupcake.objects.all().order_by('-createdAt')
        serializer = CupcakeSerializer(cakes, many=True)
        cake_json = Response(serializer.data)
        return JSONResponse(serializer.data)

```


Start development service and test API either using `curl` or `httpie`. In our case, we use `postman` to test it. You will get response like below

```json
[
  {
    "name": "Another Chocolate",
    "rating": 5,
    "price": "5",
    "image": "/media/images/cakes/chocolate_cupcake_AjwFoHt.jpg",
    "writer": "user2",
    "createdAt": "2016-08-04T04:38:36.193330Z"
  },
  {
    "name": "Green Tea Cupcake",
    "rating": 4,
    "price": "4",
    "image": "/media/images/cakes/cup_cake_2.jpg",
    "writer": "admin",
    "createdAt": "2016-08-01T03:19:29.333117Z"
  },
  {
    "name": "Blueberry Muffin",
    "rating": 4,
    "price": "3",
    "image": "/media/images/cakes/blueberry_muffin.png",
    "writer": "admin",
    "createdAt": "2016-07-29T15:35:40Z"
  },
  {
    "name": "Blueberry Cookie Cupcake",
    "rating": 3,
    "price": "5",
    "image": "/media/images/cakes/blueberry_cupcake.png",
    "writer": "admin",
    "createdAt": "2016-07-28T01:52:28Z"
  }
]
````

## Android Cupcake App

The app with Django REST API [link](https://github.com/hassanabidpk/androidcupcake)

## iOS Cupcake App
Initial version of the iOS app [[link](https://github.com/hassanabidpk/ioscupcake)]