# DJANGO RAZORPAY INTEGRATION
Here, we will see how to integrate a payment gateway using the Razorpay API in your Django Project

## STEP1 - SETUP
_Copy-paste_ the following commands in your _command prompt_ to get the initial project setup.
```
mkdir django-razorpay
cd  django-razorpay
django-admin startproject razorpay
cd razorpay
python manage.py startapp base
```

Add the _base app_ in the _installed apps_ of the project’s settings.py file. And then scroll down and _add the ‘templates’_ directory also.
```
INSTALLED_APPS = [
	‘base’, //Add this line 
	 . . .
]

TEMPLATES = [
    {
         . . .
        'DIRS': ['templates'], //Add this line
```
Run the pip command in your command prompt for _installing_ the _Razorpay_ module which is really needed!
```
pip install razorpay
```

_Create a views.py_ file in the base app directory and add the following code-
```
from django.shortcuts import render
import razorpay
from django.views.decorators.csrf import csrf_exempt


def home(request):
    if request.method == "POST":
        name = request.POST.get('name')
        amount = 50000

        client = razorpay.Client(
auth=("TEST_KEY", "SECRET_KEY"))
        payment = client.order.create({'amount': amount, 'currency': 'INR',
'payment_capture': '1'})

    return render(request, 'index.html')

@csrf_exempt
def success(request):
    return render(request, "success.html")
```
Now _add the views in the urlpatterns_ for both the project and the base app.

Replace the code with your app’s urls.py file-
```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('base.urls'))
]
```
And similarly replace the following code with the base app’s urls.py file-
```
from django.urls import path
from .views import home, success

urlpatterns = [
    path('', home, name='home'),
     path('success' , success , name='success')
]
```
Now the last thing to setup will be the templates! _So create the templates folder_ in your razorpay directory. Create three files named _index_.html, _success_.html, and _base_.html.

Now add the following code in the index.html template-
```
{% extends 'base.html' %}
{% block content %}

<head>
    <style>
        .center {
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
        }
    </style>
</head>

<div class="center">
<form class="text-center border border-light p-5" action="success" method="POST">

{% csrf_token %}

    <p class="h3 mb-3">
    <i class="fas fa-coffee"></i> BUY ME A COFFEE!!
    </p>

    <p class="mb-3">
    Help us create more videos by donating 500 INR
    </p>

<input type="name" name="name" id="name" required class="form-control mb-4" placeholder="Name">

<!-- RAZORPAY INTEGRATION HERE -->

<script src="https://checkout.razorpay.com/v1/checkout.js"
    data-key="TEST_KEY"
    data-amount="50000"
    data-currency="INR"
    data-order_id="{{payment.id}}
    data-buttontext="Pay with Razorpay"
    data-name="Professional Cipher"
    data-description="Django and Data Science"
    data-image="https://example.com/your_logo.jpg"
    data-prefill.name="PC"
    data-prefill.email="pc@studygyaan.com"
    data-theme.color="#F37254">
</script>
</form>
</div>
</div> 
{% endblock %}
```
Similarly add the following code in your success.html file-
```
{% extends 'base.html' %}
{% block content %}
<head>
   <style>
      .center {
      position: absolute;
      text-align: center;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
      }
   </style>
</head>
<body>
   <div class="center">
      <h3>
         Thanks, You are aweseome!<i class="fas fa-check-circle"></i>
      </h3>
      <h5>
         <a href="http://127.0.0.1:8000/">
         Again?
         </a>
      </h5>
   </div>
</body>
{% endblock content %}
```
And finally this code in your in base.html file to finish our templates.
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <script src="https://kit.fontawesome.com/47101d2035.js" crossorigin="anonymous"></script>

    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css"
        integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">

<title>Buy me a Coffee</title>
</head>
<body>
    {% block content %}

    {% endblock content %} 
</body>
</html>
```
## STEP 2 – ADDING RAZORPAY KEYS
So, we are done with the initial set up. Let’s move on to the main stuff. Open this link https://dashboard.razorpay.com. Create an account or log in if you own one already. Open this link now https://dashboard.razorpay.com/app/keys to get your keys. We will need to copy the keys, both the secret key and the public key.

Navigate to the views.py file and paste both the keys in the index view where specified (replace **TEST_KEY** and **SECRET_KEY** with your keys).

Similarly copy your test key and now open the _index.html_ file. There, in the data-key variable, replace the **TEST_KEY** with your copied test key value.





