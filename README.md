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


### DB DESIGNS:-


- To achieve the functionality you described, you can create the following tables:

 - users table - This table will store information about each user, including their user ID, username, password, user type, and company ID.
 
 - events table - This table will store information about each event, including the event ID, event name, start date, end date, location, and the user who created the event.

 - assigned_events table - This table will store information about which events are assigned to which users. It will have a foreign key that references the users table and another foreign key that references the events table.

```sql
CREATE TABLE users (
  user_id INT NOT NULL PRIMARY KEY,
  username VARCHAR(50) NOT NULL UNIQUE,
  password VARCHAR(255) NOT NULL,
  user_type VARCHAR(20) NOT NULL,
  company_id INT NOT NULL
);

CREATE TABLE events (
  event_id INT NOT NULL PRIMARY KEY,
  event_name VARCHAR(255) NOT NULL,
  start_date DATE NOT NULL,
  end_date DATE NOT NULL,
  location VARCHAR(255) NOT NULL,
  creator_user_id INT NOT NULL,
  FOREIGN KEY (creator_user_id) REFERENCES users(user_id)
);

CREATE TABLE assigned_events (
  user_id INT NOT NULL,
  event_id INT NOT NULL,
  FOREIGN KEY (user_id) REFERENCES users(user_id),
  FOREIGN KEY (event_id) REFERENCES events(event_id),
  PRIMARY KEY (user_id, event_id)
);

```

