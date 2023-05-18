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



- To implement this functionality, you can create a new table called contacts with the following columns:

contact_id: A unique identifier for each contact.
user_id: The ID of the user who added the contact.
contact_name: The name of the contact.
contact_email: The email address of the contact.
contact_phone: The phone number of the contact.



```sql

CREATE TABLE contacts (
  contact_id INT NOT NULL PRIMARY KEY,
  user_id INT NOT NULL,
  contact_name VARCHAR(255) NOT NULL,
  contact_email VARCHAR(255) NOT NULL,
  contact_phone VARCHAR(255) NOT NULL,
  FOREIGN KEY (user_id) REFERENCES users(user_id)
);

```





## What is persist reducer and persistStore
```js
import { persistReducer, persistStore } from "redux-persist";
```

## what is this why we required?

The code you provided is importing two functions, persistReducer and persistStore, from the "redux-persist" library. These functions are commonly used in Redux applications that require data persistence.

Redux is a state management library for JavaScript applications. It helps manage the state of your application in a predictable and centralized manner. However, by default, Redux state is reset whenever the page is refreshed or the application is restarted.

In some cases, you may need to persist the Redux state across page refreshes or app restarts, so that the user's data and application state are preserved. This is where the "redux-persist" library comes into play.

The persistReducer function is used to create a new reducer that wraps your existing Redux reducer. It adds the capability to persist the state managed by your reducer. It takes a configuration object as an argument, which specifies the storage engine to use (e.g., local storage, session storage, AsyncStorage, etc.) and other options for data serialization and migration.

The persistStore function is used to create a persisted Redux store. It takes the Redux store as an argument and returns a persisted version of the store. This persisted store will automatically load the previously saved state from the storage engine specified in the persistReducer configuration. It also enables auto-rehydration, which means that whenever the application is loaded, it will rehydrate the persisted state into the Redux store.

Here's an example of how these functions can be used:


```js
import { createStore } from "redux";
import { persistReducer, persistStore } from "redux-persist";
import storage from "redux-persist/lib/storage";

// Create your Redux reducer
const rootReducer = (state, action) => {
  // Your reducer logic
};

// Configure Redux persist
const persistConfig = {
  key: "root",
  storage: storage,
  // Add any additional configuration options here
};

// Create the persisted reducer
const persistedReducer = persistReducer(persistConfig, rootReducer);

// Create the Redux store
const store = createStore(persistedReducer);

// Create the persisted store
const persistedStore = persistStore(store);

export { store, persistedStore };


```


In this example, the persistConfig object defines the configuration options for Redux persist, including the storage engine to use (in this case, storage refers to the local storage).

The persistReducer function is then called with the persistConfig and the root reducer (rootReducer in this example) to create a new persisted reducer.

Finally, the Redux store is created with the persisted reducer, and the persistStore function is used to create the persisted store.

By using persistReducer and persistStore, you can ensure that the Redux state is automatically persisted and rehydrated across page refreshes or app restarts, providing a seamless user experience.


## Sample api response for location and location claouse:-
```
{
  "location_id": "your-location-id",
  "clauses": [
    {
      "clause_id": 1,
      "clause_uuid": "clause-uuid-1",
      "clause_section": 1,
      "clause_page": 3,
      "clause_name": "Clause A",
      "clause_details": "Details of A",
      "clause_notes": "Notes of A"
    },
    {
      "clause_id": 2,
      "clause_uuid": "clause-uuid-2",
      "clause_section": 1,
      "clause_page": 3,
      "clause_name": "Clause B",
      "clause_details": "Details of B",
      "clause_notes": "Notes of B"
    },
    ...
  ]
}


```
# Invitation url can be like this:-
so that the user can be registered against a company for a specific role and for a specific location also

```js
https://yourapp.com/register?invite_token=xxxxxxxx&role=role_type&company_id=company_id&location_id=locationid
```


## group events by months
``` js
import React from 'react';

function EventTable({ events }) {
  // Parse event due date to Date objects
  const parsedEvents = events.map((event) => ({
    ...event,
    event_due_date: new Date(event.event_due_date),
  }));

  // Group events by month
  const groupEventsByMonth = (events) => {
    return events.reduce((grouped, event) => {
      const monthYear = event.event_due_date.toLocaleString('default', {
        month: 'long',
        year: 'numeric',
      });

      if (!grouped[monthYear]) {
        grouped[monthYear] = [];
      }

      grouped[monthYear].push(event);
      return grouped;
    }, {});
  };

  const groupedEvents = groupEventsByMonth(parsedEvents);

  return (
    <div>
      {Object.keys(groupedEvents).map((monthYear) => (
        <div key={monthYear}>
          <h2>{monthYear}</h2>
          <table>
            <thead>
              <tr>
                <th>Event Name</th>
                <th>Due Date</th>
                {/* Add more table headers as needed */}
              </tr>
            </thead>
            <tbody>
              {groupedEvents[monthYear].map((event) => (
                <tr key={event.event_id}>
                  <td>{event.event_name}</td>
                  <td>{event.event_due_date.toLocaleDateString()}</td>
                  {/* Add more table cells with event details as needed */}
                </tr>
              ))}
            </tbody>
          </table>
        </div>
      ))}
    </div>
  );
}

export default EventTable;

```

