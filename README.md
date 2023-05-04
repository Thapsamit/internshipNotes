### install dotenv 
- pip install python-dotenv



```python 
import os
from dotenv import load_dotenv

# Load environment variables from the appropriate file
if os.environ.get('DJANGO_ENV') == 'production':
    load_dotenv('.env.prod')
else:
    load_dotenv('.env.dev')

# Use environment variables in your Django settings
DEBUG = os.getenv('DEBUG') == 'True'
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.getenv('DATABASE_NAME'),
        'USER': os.getenv('DATABASE_USER'),
        'PASSWORD': os.getenv('DATABASE_PASSWORD'),
        'HOST': 'localhost',
        'PORT': '5432',
    }
}


```


### Databases 👎
```
\l # to show databas
\c databasename # to change databse
\dt # to show data tables
```
