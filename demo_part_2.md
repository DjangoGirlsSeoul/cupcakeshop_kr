# Demo Part 2
## Step 9 [템플릿의 동적 데이터](http://tutorial.djangogirls.org/en/dynamic_data_in_templates/#dynamic-data-in-templates)
> `orm` branch 관련 잇어요

a. queryset을 이용해서 데이터베이스에 동적데이터(cupcake 정보)를 가져옵니다! `menu/views.py`안에 `cupcake_list` function에 다음과 같은 코드를 추가합니다.

```python
from django.shortcuts import render
from .models import Cupcake

def cupcake_list(request):
    cakes = Cupcake.objects.all().order_by('-createdAt')
    context = {"cakes": cakes}
    return render(request,"menu/list.html",context)

```

위의 `Cupcake.objects.all().order_by('-createdAt')` 부분의 query는 데이터베이스에서 `createdAt`기준으로 내림차순으로 모든 컵케이크 정보들을 가져오는 거에요!

> 이 코든는 `menu/list.html`라는 템플릿에서 로드 되고 context에 전달 될 거에요. `context`는 Python 객체들에 dictionary mapping되는 템플릿 변수 이름입니다.
> That code loads the template called menu/list.html and passes it a context. The `context` is a dictionary mapping template variable names to Python objects.

만약 홈페이지에 방문하면 데이터베이스에서 가져온 데이터들이 템플릿에서 볼수 없을 거에요! 이제 템플릿에서 돌아가서 QuerySet를 보이게 해 볼까요?

b. 템플릿에 queryset으로 가져온 데이터를 추가하기 위해 Django Template Tags를 사용할 거에요. 하드코딩된 컵케이크 코드를 제거 하고 다음과 같이 만들어 보죠!


`list.html`

``` html

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
    {% if cakes %}
    <h2 class="text-center">Choose your favorite Cupcake!</h2>
    <div class="row">
      {% for cake in cakes %}
      <div class="col-xs-12 col-sm-6 col-md-4 col-lg-4">
        <div class="card">
        <div class="card-img-top">
          <div class="image" style="background-image: url({{ cake.image.url }});"></div>
        </div>
        <div class="card-block">

          <h4 class="card-title text-center">{{ cake.name }}</h4>
          <p class="card-text text-center">
            <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
          </p>
          <a href="#" class="btn btn-default btn-lg btn-block">View</a>
        </div>
        </div>
      </div>
      {% endfor %}
    </div>
    {% else %}
    <h2 class="text-center">No Cupcakes added yet -:(</h2>
    {% endif %}
  </div>
{% endblock %}

```
개발 서버를 다시 시작하고 템플릿에 데이터베이스에서 가져온 컵케이크가 잘 보이는지 홈페이지에서 확인해 보세요! :)

c. 일단 여기에 들어가 보면 한가지 문제가 있어요!. 순서를 매기는데 `오직` 별 한개 밖에 안보이는 거죠. Django Template에서는 Template 안에서 `range`같은 복잡한 파이썬 함수들의 사용을 허용하지 않아요. 그래서 우리는 rating count로 looping도 하고 별 갯수도 추가 하기 위해서 커스텀한  Django Template filter를 추가 할 거에요.
c. We have one problem that we have to address here. As you can see that `only` one star appears for rating. Django Template doesn't allow usage of complex Python functions in template such as `range`. We have to make a custom Django Template filter for looping over rating count and add number of stars based on it. We are going to create a custom `Django Template Filter`. You can read more about it [here](https://docs.djangoproject.com/en/1.9/howto/custom-template-tags/).

그래서 `models.py` and `views.py` 파일이 있는 폴더에 `templatetags` 폴더를 만들거에요. 그 디렉토리안에 `__init__.py`라는 빈 파일을 추가 해야 된 다는 것을 잊지 마세요!. 그리고 `templatetags` 폴더안에 `menu_extras.py` 라는 파일을 만들어 보아요.

디렉토리 구조는 다음과 같아야 됩니다.

```bash

menu/
    __init__.py
    models.py
    templatetags/
        __init__.py
        menu_extras.py
    views.py

```
`menu_extras.py`안에 다음과 같은 내용을 추가합니다.

```python
from django import template

register = template.Library()

@register.filter
def get_range(value):
    """
     Filter - returns a list containing range made from given value
     Usage (in template):
    """
    return range(int(value))


```
`list.html`에는 `{% load staticfiles %}`의 뒷 부분에 다음과 같이 추가 하세요!

```html
{% load menu_extras %}
```

커스텀한 filter를 이용해서 반복문을 돌리려면 마지막으로 추가해야 되는 게 있습니다. html에 `rating`이라는 부분을 다음과 같이 바꿔주세요.

`list.html`

```html
<p class="card-text text-center">
   {% for i in cake.rating|get_range %}
    <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
   {% endfor %}
</p>

```

![](orm_cupcakes.png)

개발 서버를 다시 시작하고 홈페이지에 들어가 데이터베이스에서 가져온 컵케이크들이 잘 보이는지 확인해 보세요! :)


## Step 10 프로그램 어플리케이션 확장하기
> `extend-app` branch와 관련 있어요!

a. 지금까지 `list.html` 템플릿을 적절하게 바꾸는 것을 했는데요. 홈페이지에서 어드민에서 추가한 post들을 볼수 있으면 잘한 거에요! 잘했어요! 이제 사용자들이 버튼을 클릭해서 `Cupcake`에 대한 자세한 정보를 보는 기능을 만들고 싶어요! 기본으로 돌아가서 `list.html` 템플릿을 구성하던 단계를 다시 반복할 거에요.

우선 컵케이크 하나를 가리키는 `url`를 추가하고 `menu/urls.py`파일에 `url(r'^$',views.cupcake_list,name="cupcake_list")` 아래에 `menu/urls.py` 이 부분을 추가해 볼거에요!

```python
url(r'^cupcake/(?P<pk>\d+)/$',views.cupcake_detail,name="cupcake_detail")
```

This part `^cupcake/(?P<pk>\d+)/$` looks scary, but no worries - You can read explanation about it here [[Eng](http://tutorial.djangogirls.org/en/extend_your_application/#create-a-url-to-a-posts-detail), [Kor](https://djangogirlsseoul.gitbooks.io/tutorial/content/extend_your_application/)].

Then add a function `cupcake_detail` in `menu/views.py` to render the template we created earlier. Any url like `cupcake/1` will be sent to view function `cupcake_detail`.

```python
from django.shortcuts import render, get_object_or_404

def cupcake_detail(request,pk):
    cake = get_object_or_404(Cupcake,pk=pk)
    context = {"cake": cake}
    return render(request,"menu/detail.html",context)

```

b. There are two more things we have to do before we can see the cupcake detail page. Firstly, add a link to `list.html` template which can take us to detail page. Replace existing `<a href="#"...` with following code.

```html
<a href="{% url 'menu:cupcake_detail' pk=cake.pk %}" class="btn btn-default btn-lg btn-block">View</a>

```

The mysterious `{% url 'menu:cupcake_detail' pk=cake.pk %}` will take us to the view function `cupcake_detail` which in turn will show us the detail page!

Seondly, we are going to add template tags and custom filter in `detail.html` for showing the cupcake from database.

```html
{% extends 'menu/base.html' %}
{% load staticfiles %}
{% load menu_extras %}
{% block content %}
  <div class="container">
    {% if cake %}
    <h2 class="text-center">Order Cupcake</h2>
    <div class="row">
      <div class="col-xs-12 col-sm-6 col-md-4 col-lg-4 col-md-offset-2 col-md-lg-2">
        <div class="card">
        <div class="card-img-top">
          <div class="image" style="background-image: url({{ cake.image.url }});"></div>
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
            {% for i in cake.rating|get_range %}
            <span class="glyphicon glyphicon-star" aria-hidden="true"></span>
            {% endfor %}
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
    {% else %}
    <h2 class="text-center">No Cupcake found :(</h2>
    {% endif %}
  </div>
  <!-- Modal -->
<div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
        <h4 class="modal-title" id="myModalLabel">{{ cake.name }}</h4>
      </div>
      <div class="modal-body">
        <p>Order completed 주문 완료됬었습니다!</p>
        <p>{% now "jS F Y H:i" %}</p>
        <p>Price : {{ cake.price }}</p>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
      </div>
    </div>
  </div>
</div>
{% endblock %}

```
Start development server again, and click on `view` button in home page to see detail page. Here is one example below

![](extend_detail.png)

a. menu에 템플릿 링크 만들기 그리고 menu 상세 페이지에 뷰 추가하기


## Step 11 (Django Forms 폼)
> Relevant branch `forms`

The final thing we want to do on our website is create a nice way to add cupcakes by registered users. Django's admin is cool, but it is rather hard to customize and make pretty. With forms we will have absolute power over our interface - we can do almost anything we can imagine!

a. Create a new file `forms.py` in menu directory. We are going to use `ModelForm` which allows us to create a form from already created model. Add following code to your `forms.py`

```python

from django import forms
from .models import Cupcake


class CupcakeForm(forms.ModelForm):

    class Meta:
        model = Cupcake
        fields = ('name','rating','price','image')

```
>Note that we have excluded couple of fields like `createdAt` and `writer`. Actually we can set them up while saving the form. We will get back it shortly.

Let's add a new url for our form in `menu/urls.py`. Add below code after `    url(r'^cupcake/(?P<pk>\d+)/$',views.cupcake_detail,name="cupcake_detail"),
`

```python
    url(r'^cupcake/new/$', views.cupcake_new, name='cupcake_new'),

```

b. Now we have the form and added it to the url. All we need to do is to create a template and add function in view. Start by adding a `+` navigation button to your `base.html` template. Add following code just before `<li class="dropdown">`.

```html
    {% if user.is_authenticated %}
      <li><a href="{% url 'menu:cupcake_new' %}"><span class="glyphicon glyphicon-plus"></span></a></li>
     {% endif %}
```
> This `user.is_authenticated` will cause the link to only be sent to the browser if the user requesting the page is logged in. This doesn't protect the creation of new posts completely, but it's a good first step.

Create a new html file `cupcake_new.html` in `menu/templates/menu` directory. Add following content to it.

`cupcake_new.html`
```html
{% extends 'menu/base.html' %}
{% load staticfiles %}
{% block content %}
  <div class="container">
    <!-- Main component for a primary marketing message or call to action -->
    <div class="jumbotron title text-center" style="height: 200px;">
      <h1 style="color:black;">Add new Cupcake!</h1>
    </div>

  </div> <!-- /container -->

  <div class="container">
    <div class="row">
      <div class="col-xs-12 col-sm-12 col-md-offset-2 col-lg-offset-3 col-md-4 col-lg-4">
        <h2 class="text-center">Fill in details and submit</h2>
      <form method="POST" class="post-form" enctype="multipart/form-data">{% csrf_token %}
          {{ form.non_field_errors }}
          <div class="form-group">
          <label for="{{ form.name.id_for_label }}">Name</label>
          <input type="text" class="form-control" id="{{ form.name.id_for_label }}" name="{{ form.name.html_name }}" placeholder="blueberry Cupcake etc.">
          {{ form.name.errors }}
        </div>
        <div class="form-group">
          <label for="{{ form.rating.id_for_label }}">Rating</label>
          <input type="text" class="form-control" id="{ form.rating.id_for_label }}" name="{{ form.rating.html_name }}" placeholder="1-5">
          {{ form.rating.errors }}
        </div>
        <div class="form-group">
          <label for="{{ form.price.id_for_label }}">Price</label>
          <input type="text" class="form-control" id="{{ form.price.id_for_label }}" name="{{ form.price.html_name }}" placeholder="$ 2.00">
          {{ form.price.errors }}
        </div>
        <div class="form-group">
          <label for="{{ form.image.id_for_label }}">Image</label>
          <input type="file" id="{{ form.image.id_for_label }}" name="{{ form.image.html_name }}">
          <p class="help-block">Attach an image of size of atleast 360w x 250h</p>
          {{ form.image.errors }}
        </div>
        <button type="submit" class="btn btn-default">Submit</button>
      </form>
    </div>
    </div>
  </div>
{% endblock %}

```
There is a simple way to add form in template too. Just add ` {{ form.as_p }}` in between `<form></form>`. However, we can also add manually (as above template) if we like.

c. If you try to start development server, it will give an error. That's because we haven't added a view yet. Open `menu/views.py` file and add following lines with rest of the `from` rows.

```python
from django.shortcuts import redirect
from .forms import CupcakeForm
from django.utils import timezone
from django.contrib.auth.decorators import login_required

```

and view function.

```python
@login_required
def cupcake_new(request):
    if request.method == "POST":
        form = CupcakeForm(request.POST,request.FILES)
        if form.is_valid():
            cake = form.save(commit=False)
            cake.createdAt = timezone.now()
            cake.writer = request.user
            cake.save()
            return redirect('cupcake_detail',pk=cake.pk)
    else:
        form = CupcakeForm()
    context = {'form':form}
    return render(request,"menu/cupcake_new.html",context)
```

> `@login_required`will make sure that only logged in user can save the new cupcake.

Let's see if it works. Go to the page [http://127.0.0.1:8000/cupcake/new/](http://127.0.0.1:8000/cupcake/new/), add name,rating, price and image, submit it... and voilà! The new cupcake is added and we are redirected to cupcake_detail page!

![](forms_1.png)

Congrats :) We are almost done with development of our site!

Finally, it's time to have cupcake. There is one more thing that we have to do. So hang in there...

## Step 12 Deploy your site on PythonAnywhere [배포하기](http://tutorial.djangogirls.org/ko/deploy/#github에서-pythonanywhere로-코드-가져오기)

> Relevant branch `deploy`

Good work, if you have reached this far! Before deploying our site on PythonAnywhere. We have to commit all the changes and push them to `Github`.
Make sure that your `.gitignore` file has following content.

```bash
*.pyc
__pycache__
myvenv
db.sqlite3
/static
.DS_Store
media/
```
### Security for Production
> Follow this [link](https://github.com/espern/espern.github.io-source/blob/master/content/home/en-securing-your-django-settings-on-github.md)

#### Django secret key
Open your `settings.py` file. You will find `secret key`

```python
# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = '(+73cf=j*8!=r$#2à^@ibgpw8yn9pm#wa42bk&amp;v(@*%m7nx1sg'
```
We are hosting our website on Github. So everyone can see the secret key. One way to avoid this is by making two separate `settings.py` file for development and production server. But it will really be a pain to maintain both.

We will use `Environment Variables` on PythonAnywhere to store our `database password` and `SECRET_KEY`.  Before doing this, we have to change our `settings.py` file to replace `secret_key` with some dummy value. We will also make sure that `DEBUG=False` for production version. Replace the `DEBUG=True`, `SECRET_KEY` and `ALLOWED_HOSTS` code with following.

```python
DEBUG = os.getenv('DJANGO_DEBUG') != 'FALSE'

# SECURITY WARNING: keep the secret key used in production secret!
if DEBUG:
    SECRET_KEY = 'Hell@World!'
else:
    SECRET_KEY = os.getenv('SECRET_KEY')

# SECURITY WARNING: don't run with debug turned on in production!

if DEBUG:
    ALLOWED_HOSTS = ['*']
else:
    ALLOWED_HOSTS = ['djangocupcakeshop.pythonanywhere.com']

```

`os.getenv('DJANGO_DEBUG')` is an environment variable which we will add on PythonAnywhere later.

> We are making sure that our secret key is not Github! We are also making `DEBUG=False` for your production site.PythonAnywhere related code is in the next section.


#### Publish on Github

Do `git status` to check the current status. We will add all the files and save our changes

```bash
$ git add --all
$ git commit -m "finished tutorial until Step 10"
```

It's time to publish your `awesome` work to Github.

```bash
$ git push -u origin master
```

#### PythonAnywhere

PythonAnywhere has free tier which can you use to deploy your site. Go to [PythonAnywhere.com](https://www.pythonanywhere.com) and log in.
> If you haven't signed up then sign up for a free "Beginner" account on PythonAnywhere.

When you've logged in to PythonAnywhere, you'll be taken to your dashboard or "Consoles" page. Choose the option to start a "Bash" console -- that's the PythonAnywhere version of a console, just like the one on your computer.

We will fetch our code from Github to Pythonanywhere using `git clone`.

```bash
$ git clone https://github.com/<your_github_user_name>/djangocupcakeshop.git
```

> Don't forget to replace `<your_github_user_name>` with your github username. Make sure that you are not using `DjangoGirlsSeoul` username :)

Now its time to setup virtual environment on PythonAnywhere.

```bash
$ cd djangocupcakeshop
$ virtualenv --python=python3.5 myvenv
$ source myvenv/bin/activate
(myvenv) $ pip install -r requirements.txt
```

One things to note that we put `db.sqlite3` file in `.gitignore`. The main reason of it was to avoid sharing our database on github. We have to create a new database on PythonAnywhere and setup a superuser.

```bash
(myvenv) $ python manage.py migrate
(myvenv) $ python manage.py createsuperuser
```

Since we are deplpoying production version of our site, we have to do one more step. Run the following command in console

```bash
(myvenv) $ python manage.py collectstatic
```

Type `yes` when prompted. Django will move all static files (images,css, javascript) to a single directory (the STATIC_ROOT).

That's all for the console commands -:)
Click back to the PythonAnywhere dashboard by clicking on its logo, and go click on the Web tab. Finally, hit Add a new web app.

After confirming your domain name, choose manual configuration (not the "Django" option) in the dialog. Next choose Python 3.5, and click Next to finish the wizard.

![](deploy_create_app_1.png)


#### Setting the virtualenv

In the "Virtualenv" section, click the red text that says "Enter the path to a virtualenv", and enter: `/home/<your-PythonAnywhere-username>/djangocupcakeshop/myvenv/`. Click the blue box with the check mark to save the path before moving on.

![](deploy_venv_2.png)

Also Add the path to your source code `/home/<your-PythonAnywhere-username>/djangocupcakeshop`.


#### Configuring the WSGI file
Click on `wsgi configuration file` link and paste the following contents. Replace path with your project path.

`wsgi`

```python
import os
import sys

path = '/home/<your_pythonanywhere_username>/djangocupcakeshop'  # use your own PythonAnywhere username here
if path not in sys.path:
    sys.path.append(path)

os.environ['DJANGO_SETTINGS_MODULE'] = 'djanocupcakeshop.settings'
os.environ['DEBUG'] = 'FALSE'
os.environ['SECRET_KEY'] = 'MY_SECRET_KEY'

from django.core.wsgi import get_wsgi_application
from django.contrib.staticfiles.handlers import StaticFilesHandler
application = StaticFilesHandler(get_wsgi_application())
```
> Replace `MY_SECRET_KEY` with your secret key!

We're all done! Hit the big green Reload button and you'll be able to go view your application. You'll find a link to it at the top of the page.

![](deploy_refresh_3.png)

	a. PythonAnywhere에서 무료 계정인 "초보자(Beginner)"로 회원가입 하세요. GitHub에서 PythonAnywhere로 코드 가져오기
	b. PythonAnywhere에서 가상환경(virtualenv) 생성하기. 콘솔창에서 `virtualenv --python=python3.4 myvenv` 그리고 `pip install -r requirements.txt` 실행하세요.정적 파일 모으기 `python manage.py collectstatic`
	c. PythonAnywhere에서 데이터베이스 생성하기 `python manage.py migrate`
	d. web app으로 DjangoCupcakeshop 배포하기 - 가상환경(virtualenv) 설정하기 그리고 WSGI 파일 설정하기


## Step 13 Homework (숙제)

a. Sort cupcakes by `highest` and `lowest` price

> Hint : Make a url like `cupcakes/price/hightolow`, add a corresponding view function. Get the data from Database, convert price `string` to int and sort using python. You can use the same template `list.html` for this homework.

b. Sort cupcakes by `highest` rating

