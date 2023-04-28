


# DJANGO SETUP MVT:-


### Steps for installation

- first create a virtual env 
- activate the virtual env
- install django 
- run command for creating a project in django using start-project and creating an app using start-app
- for using templates :-
  - create templates folder 
  - in settings.py file inside TEMPLATES in dirs  write below 
  ```python 
   os.path.join(BASE_DIR,'templates')
  ```


### Django model forms:-

- This allows to create forms based on the models


## How to setup static files in Django 
- create a static folder in root directory
- create styles,images,js and any other folder inside static folder
- inside styles we can create styles.css or anything else

- inside settings.py write this
```python
STATICFILES_DIRS = [BASEDIR / "static"]
```

## Where to upload user generated media files:-

- for example if user has put an image in the form then where to upload this media
- in Settings.py write this
```python 
MEDIA_ROOT = os.path.join(BASE_DIR,'static/images')

```
- also write this 
```python
MEDIA_URL = "/images/"
```

- Inside urls.py of main project
```python 
from django.conf import settings
from django.conf.urls.static import static


urlpatterns = [
    path("admin/", admin.site.urls),
    path('',include('projects.urls'))  
] + static(settings.MEDIA_URL,document_root=settings.MEDIA_ROOT) # add this static behind urlpatterns
```

## what if we debug = False

- The ALLOWED_HOSTS setting is enforced: When DEBUG = False, Django will only serve requests with the Host header that matches a value in the ALLOWED_HOSTS setting. This is a security measure to prevent HTTP Host header attacks.

- Static files are not served: When DEBUG = False, Django will not serve static files (CSS, JavaScript, and images) in development mode. This is because serving static files in production is typically done using a web server such as Nginx or Apache.

- Template errors are not displayed: When DEBUG = False, Django will not display template syntax errors to the end-users. Instead, a generic error page will be displayed.

- E-mail is sent on errors: When DEBUG = False, Django will send an email to the site administrators with details of the error, including a full stack trace. This is a helpful feature for debugging errors in production.
- In order to serve static files in production we need to write below line in settings.py file and then run the next command following the below command
```python 
STATIC_ROOT = os.path.join(BASE_DIR,'staticfiles')
```
```python
python manage.py collectstatic
```
- After doing above things and let suppose we did DEBUG = False
- then we need to do following
- ALLOWED_HOSTS in settings.py file
```python
ALLOWED_HOSTS = ['localhost:8000','127.0.0.1',mywebsite.com]
```
- In urls.py write this 
```python

urlpatterns+=static(settings.STATIC_URL,document_root=settings.STATIC_ROOT)

```

- After this we need to install whitenoise to serve image files
- then in settings.py in INSTALLED_APPS add the middleware whitenoise






















