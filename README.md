## Why To use Express.js?
- Node.js is a runtime environment not a framework in which we have to write a lot of code to even build a simple server
- Therefore express.js is used to reduce the number of lines of code.
- It is good in routing


## Difference between nodejs, koa, express?

## Route Parameters.


## What is middleware?
- A function that is placed in between request and response object
- Request ---->  Middleware ----> Response <br>
  <---------------------------------------
  
```js
app.use((req,res,next)=>{

next() // execute next middle ware functions
})


```          
      
### what is the use of app.use(express.json())

- app.use(express.json()) is a middleware function in the Express.js framework that parses incoming JSON payloads.

- When a client sends data to a server in the form of a JSON payload, the server needs to be able to parse that data and convert it into a format that can be used by the application. The express.json() middleware function does just that by parsing the incoming JSON data and making it available on the request object as req.body.

- By using this middleware, we can easily parse and handle JSON data in our routes or controllers without having to manually parse it ourselves. This can save us a lot of time and make our code more maintainable.



### what is origin 
- the combination of domain name and port where the application is running but browser by default blocks all the cross origin request as when app in one origin requests for the data from other origins (the application running in different origins ) is blocked


### How to make build folder of react app inside the server files

-  create a script in frontend react 
```js
   {
    "build":"BUILD_PATH=../server/public react-scripts build"
   }
```

- Then run npm run build it will create the build folder inside server


```js
const date = new Date("January 30,2030")
console.log(isNaN(date))
// return false
// because date is always do date.valueOf() that returns a number and if it is a date then only it returns a number otherwise if it is invalid date then it doesn't returns numbers
```


```js
const express = require('express');
const mongoose = require('mongoose');
const bcrypt = require('bcrypt');

// Create express app
const app = express();

// Connect to MongoDB
mongoose.connect('mongodb://localhost/social_media_app', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
})
  .then(() => {
    console.log('Connected to MongoDB');
  })
  .catch((error) => {
    console.error('Error connecting to MongoDB:', error);
  });

// Define user schema
const userSchema = new mongoose.Schema({
  username: {
    type: String,
    required: [true, 'Username is required'],
    unique: true,
  },
  email: {
    type: String,
    required: [true, 'Email is required'],
    unique: true,
    validate: {
      validator: (value) => {
        // Simple email format validation
        const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        return emailRegex.test(value);
      },
      message: 'Invalid email address',
    },
  },
  password: {
    type: String,
    required: [true, 'Password is required'],
    minlength: [8, 'Password must have at least 8 characters'],
  },
  profilePicture: String,
  bio: String,
  following: [{ type: mongoose.Schema.Types.ObjectId, ref: 'User' }],
  followers: [{ type: mongoose.Schema.Types.ObjectId, ref: 'User' }],
});

// Define post schema
const postSchema = new mongoose.Schema({
  author: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
  content: String,
  image: String,
  tags: [{ type: mongoose.Schema.Types.ObjectId, ref: 'User' }],
  likes: [{ type: mongoose.Schema.Types.ObjectId, ref: 'User' }],
  comments: [{
    user: { type: mongoose.Schema.Types.ObjectId, ref: 'User' },
    content: String,
    createdAt: { type: Date, default: Date.now },
  }],
  createdAt: { type: Date, default: Date.now },
});

// Define models
const User = mongoose.model('User', userSchema);
const Post = mongoose.model('Post', postSchema);

// Middleware for parsing JSON requests
app.use(express.json());

// User registration
app.post('/register', async (req, res) => {
  const { username, email, password } = req.body;
  const hashedPassword = await bcrypt.hash(password, 10);

  try {
    const user = new User({ username, email, password: hashedPassword });
    await user.save();
    res.status(200).json({ message: 'User registered successfully' });
  } catch (error) {
    res.status(500).json({ error: 'Error registering user' });
  }
});

// User login
app.post('/login', async (req, res) => {
  const { email, password } = req.body;

  try {
    const user = await User.findOne({ email });
    if (!user) {
      res.status(404).json({ error: 'User not found' });
      return;
    }

    const passwordMatch = await bcrypt.compare(password, user.password);
    if (!passwordMatch) {
      res.status(401).json({ error: 'Invalid password' });
      return;
    }

    res.status(200).json({ message: 'User logged in successfully' });
  } catch (error) {
    res.status(500).json({ error: 'Error logging in' });
  }
});

// Start the server
app.listen(300


```
