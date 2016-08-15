# 데모
## Step 1. 설치하기

파이썬 3.5.x, Git, 코드에디터(atom,sublime text, visual code 중 하나)가 반드시 설치되어야 합니다.
프로젝트 시작 전 가상환경을 설치하고 실행시키세요. 그리고 아래 명령어를 입력해 django를 설치합니다.

 ```bash
 $ pip install django
 ```

장고나 다른 패키지를 설치할 때는 `requirements.txt` 파일을 생성해 설치 버전을 저장해 놓는 것이 좋습니다.

 ```bash
 $ pip freeze > requirements.txt
 ```

## Step 2. 프로젝트 시작하기
이제 `djangocupcakeshop` 장고 프로젝트를 시작해봅시다.

 ```bash
 $ django-admin startproject djangocupcakeshop
 ```

## Step 3. 설정 변경하기
프로젝트를 생성하고나서 `settings.py`에 있는 `TIME_ZONE`을 변경해야 합니다. `djangocupcakeshop/djangocupcakeshop/settings.py`에서 `TIME_ZONE`을 찾아 변경하세요. 웹 사이트가 어디서 운영(host)되고 있는지 알려주는 거랍니다. 서울이라면, 아래처럼 변경해주세요.


 ```bash
 $ pip install django
 ```

 Once we install django and any other required package, it is good idea to save that in a `requirements.txt` file.

 ```bash
 $ pip freeze > requirements.txt
 ```

## Step 2 Start Project
 Start Django project

 ```bash
 $ django-admin startproject djangocupcakeshop
 ```

 Django 프로젝트 시작하기

 ```bash
 $ django-admin startproject djangocupcakeshop
 ```

## Step 3 Change Settings
After creating the project, we have to change `TIME_ZONE` in `settings.py` file. You can find it under `djangocupcakeshop/djangocupcakeshop/settings.py` folder. It depends on where your site is hosted. For Seoul, we will change it the following.

```python
TIME_ZONE = "Asia/Seoul"
```
`STATIC_URL` 부분 아래에 동적파일과 미디어파일(images, css, javascript) 경로도 아래처럼 설정해주세요.

```python
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
MEDIA_URL = '/media/'
```

>MEDIA_ROOT와 MEDIA_URL설정 경로는 이미지와 파일들이 저장되는 경로를 말합니다.

테이터베이스 테이블을 생성했고 브라우저에 프로젝트가 보이는지 확인합시다. 터미널/프롬트프에서 아래 두 명령어를 실행하세요.

``` bash
$ python manage.py migrate

$ python manage.py runserver
```

브라우저를 열고 다음 링크로 접속하세요. : [http://127.0.0.1:8000](http://127.0.0.1:8000)


![](step_3_runserver.png)


## Step 4. Django App/Model
> Relevant git branch `model`

a. `menu`앱을 만들고 `settings.py`의 `INSTALLED_APPS`에 새로운 앱을 추가하세요.

```bash
$ manage.py startapp menu
```

`settings.py`

```python
INSTALLED_APPS = (
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'menu',
)

```

#### Demo. 여기서부터 시작하세요.

b. 메뉴 앱 모델를 생성해봅시다. 여기서부터는 튜토리얼 그대로 따라하세요. `Cupcake`에 필요한 것은 `이름(name), 평가(rating), 가격(price), 이미지(image), 작성자(writer), 작성일(createdAt)`의 정보입니다. 이미지필드(`ImageField`)에서 필요한 것은 `Pillow`패키지가 설치되어야합니다. 아래 명령어를 실행해 설치합시다.

```bash
(myvenv) $ pip install Pillow
```
and then update requirements file by

```bash
(myvenv) $ pip freeze > requirements.txt
```

> 윈도우에서 Pillow가 설치 시 에러가 발생하면, `pip install Pillow==3.0.0`를 사용하세요.

먼저 import 합시다.

```python

from django.contrib.auth.models import User
from django.utils import timezone

```

아래 Cupcake 클래스를 그대로 따라해 봅시다.

```python

class Cupcake(models.Model):
    name = models.CharField(max_length=200)
    price = models.CharField(max_length=20)
    rating = models.FloatField()
    image = models.ImageField(upload_to='images/cakes')
    writer = models.ForeignKey(User)
    createdAt = models.DateTimeField(default=timezone.now)

    def __str__(self):
        return self.name
```

 c. 모델을 생성한 후에, 아래 명령어를 실행해 실제 데이터 베이스를 생성합니다.


 ```bash

 $ python manage.py makemigrations menu
 $ python manage.py migrate


 ```

 > makemigrations을 실행하면, 장고에게 모델에 수정할 것이 있는지 확인하라고 말해주는 것입니다. (이 경우에, 새로운 모델이 생성이 되었다는 것을 알려줍니다) 그리고 수정 내용을 migration하게 됩니다.


 ```

 > By running makemigrations, you’re telling Django that you’ve made some changes to your models (in this case, you’ve made new ones) and that you’d like the changes to be stored as a migration.
>>>>>>> 0d638dce3cef986977c116ae84a0d7be7979cb8f
 > At this point after `makemigrations` you can test your model by executing `python manage.py test`. We have provided a test case already for your convenience!
 > In Summary `makemigrations` command make migrations for changes in model and `migrate` actually applies those changes in database


## Step 5. Django 관리자
> Relevant git branch `admin`

a. 관리자에 모델을 추가해 관리자 사이트에서 컵케익을 등록할 수 있게 만듭시다. `menu/admin.py`파일을 열고 아래 내용을 추가하세요.

```python
from django.contrib import admin
from .models import Cupcake

admin.site.register(Cupcake)
```
b. Now its time to add few cupcakes from the site. But in order to do so, you would need admin account. Create it by executing following command in terminal/command prompt and follow the instructions.

```bash
python manage.py createsuperuser
```

Start developement server again `python manage.py runserver`. Visit [http://127.0.0.1:8000/admin](http://127.0.0.1:8000/admin) to login and add cupcakes!

![](admin_1_main.png)

![](admin_2_add.png)


## Step 6. Django Urls
> Relevant git branch `django-urls`

a. We have to point a url towards our home page. Firstly, we will point the home page URL to our menu app. Add following to `djangocupcakeshop/urls.py`.

```python
urlpatterns = [
    url(r'^admin/', include(admin.site.urls)),
    url(r'', include('menu.urls')),
]

```


b. Secondly, we will point URL to corresponding `view` function in our menu app. Create a file in `menu` directory and name it `urls.py`. Add following code

```python
from django.conf.urls import url
from . import views

urlpatterns = [
    url(r'^$',views.cupcake_list,name="cupcake_list"),
]
```
For now if you run your server and visit the home page, It will show an error. If you pay attention to the error, it is about absence of views function `cupcake_list`. Don't worry, we will create this function in the next step.

![](urls_error.png)


## Step 7 Django Views
In Django, web pages and other content are delivered by views. Each view is represented by a simple Python function. Django will choose a view by examining the URL that’s requested. We have already added `url` for home page in previous step. We are going to add our first view function `cupcakes_list` in `menu/views.py` file.

```python
from django.shortcuts import render
from . import models

def cupcake_list(request):
    return render(request,"menu/list.html",{})
```

Start your server and visit the home page [http://127.0.0.1:8000](http://127.0.0.1:8000) Oops! You will come across following error!. Let's fix this in next step.

![](views_error.png)


## Step 8 Django Templates

a. If you look at the error in previous step, you will notice that is complaining about missing template `menu/list.html`. Django template is an html page where you show your data to user which was stored in database. First, create a directory called templates in your menu directory. Django will look for templates in there.
Within the templates directory you have just created, create another directory called menu, and within that create a file called list.html. In other words, your template should be at menu/templates/menu/list.html. We will use bootstrap for creating html page. You can find some example templates [here](http://getbootstrap.com/getting-started/). To customize our template we created a `style.css` file and added few nice images! You can find free cupcake images from [here](https://www.pexels.com/search/cupcake/). For adding static files, create a folder `static` in menu directory. Then create a menu folder. In menu folder, create two folders to hold our css and image files. So your directory sturcture for css will be `menu/static/menu/css` and images `menu/static/menu/images`

`list.html`

```html
{% load staticfiles %}
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">
	<title>Django Cupcake Shop</title>

<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
<!-- Optional theme -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap-theme.min.css">
<link rel="stylesheet" href="{% static 'menu/css/style.css' %}">
</head>
<body>
  <!-- Fixed navbar -->
  <nav class="navbar navbar-default navbar-fixed-top">
    <div class="container">
      <div class="navbar-header">
        <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar" aria-expanded="false" aria-controls="navbar">
          <span class="sr-only">Toggle navigation</span>
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
        </button>
        <a class="navbar-brand" href="/">Django Cupcake Shop</a>
      </div>
      <div id="navbar" class="navbar-collapse collapse">
        <ul class="nav navbar-nav navbar-right">
          <li class="dropdown">
            <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">Sort by <span class="caret"></span></a>
            <ul class="dropdown-menu">
              <li><a href="#">Highest</a></li>
              <li><a href="#">Lowest</a></li>
            </ul>
          </li>
        </ul>
      </div><!--/.nav-collapse -->
    </div>
  </nav>


  <div class="container" >

    <!-- Main component for a primary marketing message or call to action -->
    <div class="jumbotron title text-center" style="background-image: url({% static 'menu/images/cupcake_cover.jpg' %});">
      <h1 class="title">Cupcakes and High Fives!</h1>
      <p>Django Girls Seoul welcomes you!</p>
      <p>Lets build an awesome Django site together</p>
      <p>
        <a class="btn btn-lg btn-primary" href="https://github.com/DjangoGirlsSeoul/djangocupcakeshop" role="button">Source Code &raquo;</a>
      </p>
    </div>

  </div> <!-- /container -->

  <div class="container">
    <h2 class="text-center">Choose your favorite Cupcake!</h2>
    <div class="row">
      <div class="col-xs-12 col-sm-6 col-md-4 col-lg-4">
        <div class="card">
        <div class="card-img-top">
          <div class="image" style="background-image: url({% static 'menu/images/chocolate_cupcake.jpg' %});"></div>
        </div>
        <div class="card-block">
          <h4 class="card-title text-center">Chocolate Cupcake</h4>
            <p class="card-text text-center">
              <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
              <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
              <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
              <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
              <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
            </p>
          <a href="#" class="btn btn-default btn-lg btn-block">View</a>
        </div>
        </div>
      </div>
      <div class="col-xs-12 col-sm-6 col-md-4 col-lg-4">
        <div class="card">
          <div class="card-img-top">
          <div class="image" style="background-image: url({% static 'menu/images/vanilla_cupcake.jpeg' %});"></div>
          </div>
        <div class="card-block">
          <h4 class="card-title text-center">Vanilla Cupcake</h4>
          <p class="card-text text-center">
            <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
            <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
            <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
            <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
          </p>
          <a href="#" class="btn btn-default btn-lg btn-block">View</a>
        </div>
        </div>
      </div>
      <div class="col-xs-12 col-sm-6 col-md-4 col-lg-4">
        <div class="card">
          <div class="card-img-top">
          <div class="image" style="background-image: url({% static 'menu/images/blueberry_cupcake.png' %});"></div>
          </div>
        <div class="card-block">
          <h4 class="card-title text-center">Blueberry Cookie Cupcake</h4>
          <p class="card-text text-center">
            <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
            <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
            <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
          </p>
          <a href="#" class="btn btn-default btn-lg btn-block">View</a>
        </div>
        </div>
      </div>
    </div>
  </div>

  <footer class="footer">
      <div class="container">
        <p class="text-muted">Pycon 2016 Tutorial.</p>
      </div>
    </footer>
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"></script>
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js" ></script>

</body>
</html>

```
We also need a css file for minor adjustments in the template. Create  file `style.css` in `menu/static/menu/css` folder. Add following contents to css file
`style.css`

```css
 @import url(http://fonts.googleapis.com/css?family=Raleway:400,800|Roboto+Slab:300);


body {
  font-family: "Roboto Slab","Helvetica", "Arial", sans-serif;;
  padding-top: 70px;
}

.card {
  margin: 2rem auto;
}
.image {
  width: 100%;
  height: 250px;
  padding-bottom: 50%;
  transition: 0.1s linear;
  background-image: url(http://www.freeallimages.com/wp-content/uploads/2014/09/space-cat-wallpaper-5.jpg);
  background-size: cover;
  background-position: center center;
}

.card-title {
  margin-bottom: 1rem;
  font-weight: 900;
}

.jumbotron {
  height: 450px;
}

.title {
  color: white !important;

}

.footer {
  position: relative;
  bottom: 0;
  width: 100%;
  margin-top: 150px;
  text-align: center;
  /* Set the fixed height of the footer here */
  height: 60px;
  background-color: #f5f5f5;
}
.footer > .container {
  padding-right: 15px;
  padding-left: 15px;
}
.text-muted {
  margin-top: 20px;
}

.glyphicon
{
  color:#FF5252;
}

/* navbar */
.navbar-default {
    border-color: #FF5252;
}

a {
  color: #FF5252;
}


```
In `menu/static/images` folder, add three cupcakes images and make sure that names are same as the ones in `list.html` file.

b. We have to create a detail page for our `cupcake` where we can show more information such as price and user who uploaded it. Let's create a `detail.html` in the same folder as `list.html`

```html
{% load staticfiles %}
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">
	<title>Django Cupcake Shop</title>

<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
<!-- Optional theme -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap-theme.min.css">
<link rel="stylesheet" href="{% static 'menu/css/style.css' %}">
</head>
<body>
  <!-- Fixed navbar -->
  <nav class="navbar navbar-default navbar-fixed-top">
    <div class="container">
      <div class="navbar-header">
        <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar" aria-expanded="false" aria-controls="navbar">
          <span class="sr-only">Toggle navigation</span>
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
        </button>
        <a class="navbar-brand" href="/">Django Cupcake Shop</a>
      </div>
      <div id="navbar" class="navbar-collapse collapse">
        <ul class="nav navbar-nav navbar-right">
          <li class="dropdown">
            <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">Sort by <span class="caret"></span></a>
            <ul class="dropdown-menu">
              <li><a href="#">Highest</a></li>
              <li><a href="#">Lowest</a></li>
            </ul>
          </li>
        </ul>
      </div><!--/.nav-collapse -->
    </div>
  </nav>

  <div class="col-xs-12 col-sm-6 col-md-3 col-lg-3">
          <div class="card">
        <ul class="list-group">
          <li class="list-group-item"><span class="glyphicon glyphicon-tag"></span>  <strong>Chocolate Cupcake</strong></li>
          <li class="list-group-item"><span class="glyphicon glyphicon-usd"></span> 3.00</li>
          <li class="list-group-item"><span class="glyphicon glyphicon-pencil"></span> John</li>
          <li class="list-group-item"><span class="glyphicon glyphicon-calendar"></span> 3rd June, 2015</li>
          <li class="list-group-item">
            <span class="glyphicon glyphicon-star"></span>
            <span class="glyphicon glyphicon-star"></span>
            <span class="glyphicon glyphicon-star"></span>
            <span class="glyphicon glyphicon-star"></span>
            <span class="glyphicon glyphicon-star"></span>
          </li>
          <li class="list-group-item">
            <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#myModal">
              Order
            </button>
             </li>
        </ul>
      </div>
    </div>
    </div>
  </div>
  <!-- Modal -->
<div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
        <h4 class="modal-title" id="myModalLabel">Chocolate Cupcake</h4>
      </div>
      <div class="modal-body">
        Complete this website to get your Cupcake!
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
      </div>
    </div>
  </div>
</div>
  <footer class="footer">
      <div class="container">
        <p class="text-muted">Pycon 2016 Tutorial.</p>
      </div>
    </footer>
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"></script>
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js" ></script>

</body>
</html>

```

C. We can observe that both `list.html` and `base.html` share lots of common html such as header and footer. Django allows us to create `base` template and extend other templates from it. Create a `base.html` in same folder with `list.html`. And put the `list.html` contents in it. Delete the part from `<div class="container">` until just before the `footer` and replace with

```python
  {% block content %}
    {% endblock %}
```

After replacing, your `base.html` will look like as follows :

```html

{% load staticfiles %}
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">
	<title>Django Cupcake Shop</title>

<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
<!-- Optional theme -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap-theme.min.css">
<link rel="stylesheet" href="{% static 'menu/css/style.css' %}">
</head>
<body>
  <!-- Fixed navbar -->
  <nav class="navbar navbar-default navbar-fixed-top">
    <div class="container">
      <div class="navbar-header">
        <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar" aria-expanded="false" aria-controls="navbar">
          <span class="sr-only">Toggle navigation</span>
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
        </button>
        <a class="navbar-brand" href="/">Django Cupcake Shop</a>
      </div>
      <div id="navbar" class="navbar-collapse collapse">
        <ul class="nav navbar-nav navbar-right">
          <li class="dropdown">
            <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">Sort by <span class="caret"></span></a>
            <ul class="dropdown-menu">
              <li><a href="#">Highest</a></li>
              <li><a href="#">Lowest</a></li>
            </ul>
          </li>
        </ul>
      </div><!--/.nav-collapse -->
    </div>
  </nav>

    {% block content %}
    {% endblock %}

  <footer class="footer">
      <div class="container">
        <p class="text-muted">Pycon 2016 Tutorial.</p>
      </div>
    </footer>
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"></script>
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js" ></script>
</body>
</html>



```

Now we have to connect base template with list and detail template.

``` list.html ```

```html

{% extends 'menu/base.html' %}
{% load staticfiles %}
{% block content %}
  <div class="container">
    <!-- Main component for a primary marketing message or call to action -->
    <div class="jumbotron title text-center" style="background-image: url({% static 'menu/images/cupcake_cover.jpg' %});">
      <h1 class="title">Cupcakes and High Fives!</h1>
      <p>Django Girls Seoul welcomes you!</p>
      <p>Lets build an awesome Django site together</p>
      <p>
        <a class="btn btn-lg btn-primary" href="https://github.com/DjangoGirlsSeoul/djangocupcakeshop" role="button">Source Code &raquo;</a>
      </p>
    </div>

  </div> <!-- /container -->

  <div class="container">
    <h2 class="text-center">Choose your favorite Cupcake!</h2>
    <div class="row">
      <div class="col-xs-12 col-sm-6 col-md-4 col-lg-4">
        <div class="card">
        <div class="card-img-top">
          <div class="image" style="background-image: url({% static 'menu/images/chocolate_cupcake.jpg' %});"></div>
        </div>
        <div class="card-block">
          <h4 class="card-title text-center">Chocolate Cupcake</h4>
            <p class="card-text text-center">
              <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
              <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
              <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
              <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
              <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
            </p>
          <a href="#" class="btn btn-default btn-lg btn-block">View</a>
        </div>
        </div>
      </div>
      <div class="col-xs-12 col-sm-6 col-md-4 col-lg-4">
        <div class="card">
          <div class="card-img-top">
          <div class="image" style="background-image: url({% static 'menu/images/vanilla_cupcake.jpeg' %});"></div>
          </div>
        <div class="card-block">
          <h4 class="card-title text-center">Vanilla Cupcake</h4>
          <p class="card-text text-center">
            <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
            <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
            <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
            <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
          </p>
          <a href="#" class="btn btn-default btn-lg btn-block">View</a>
        </div>
        </div>
      </div>
      <div class="col-xs-12 col-sm-6 col-md-4 col-lg-4">
        <div class="card">
          <div class="card-img-top">
          <div class="image" style="background-image: url({% static 'menu/images/blueberry_cupcake.png' %});"></div>
          </div>
        <div class="card-block">
          <h4 class="card-title text-center">Blueberry Cookie Cupcake</h4>
          <p class="card-text text-center">
            <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
            <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
            <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
          </p>
          <a href="#" class="btn btn-default btn-lg btn-block">View</a>
        </div>
        </div>
      </div>
    </div>
  </div>
{% endblock %}


```

`detail.html`

```html

{% extends 'menu/base.html' %}
{% load staticfiles %}

{% block content %}
  <div class="container">
    <h2 class="text-center">Order Cupcake</h2>
    <div class="row">
      <div class="col-xs-12 col-sm-6 col-md-4 col-lg-4 col-md-offset-2 col-md-lg-2">
        <div class="card">
        <div class="card-img-top">
          <div class="image" style="background-image: url({% static 'menu/images/chocolate_cupcake.jpg' %});"></div>
        </div>
        </div>
      </div>
      <div class="col-xs-12 col-sm-6 col-md-3 col-lg-3">
          <div class="card">
        <ul class="list-group">
          <li class="list-group-item"><span class="glyphicon glyphicon-tag"></span>  <strong>Chocolate Cupcake</strong></li>
          <li class="list-group-item"><span class="glyphicon glyphicon-usd"></span> 3.00</li>
          <li class="list-group-item"><span class="glyphicon glyphicon-pencil"></span> John</li>
          <li class="list-group-item"><span class="glyphicon glyphicon-calendar"></span> 3rd June, 2015</li>
          <li class="list-group-item">
            <span class="glyphicon glyphicon-star"></span>
            <span class="glyphicon glyphicon-star"></span>
            <span class="glyphicon glyphicon-star"></span>
            <span class="glyphicon glyphicon-star"></span>
            <span class="glyphicon glyphicon-star"></span>
          </li>
          <li class="list-group-item">
            <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#myModal">
              Order
            </button>
             </li>
        </ul>
      </div>
    </div>
    </div>
  </div>
  <!-- Modal -->
<div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
        <h4 class="modal-title" id="myModalLabel">Chocolate Cupcake</h4>
      </div>
      <div class="modal-body">
        Complete this website to get your Cupcake!
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
      </div>
    </div>
  </div>
</div>
{% endblock %}

```

Start developement server go to the link : [http://127.0.0.1:8000](http://127.0.0.1:8000) to see the list template!

**Move to next part of [Demo](demo_part_2.md)**
