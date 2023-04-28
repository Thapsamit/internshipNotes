


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
MEDIA_URL = ""
```





























