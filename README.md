## Different ways of authentication handling in frontend 


- Local storage,cookies, session storage

## Why not to use tokens inside local storage 


Using localStorage to store tokens for authentication can be a reasonable option in some scenarios, but it has both advantages and disadvantages:

Advantages:

Persistence: Data stored in localStorage persists even after the user closes the browser or navigates away from the website. This can be useful for maintaining a user's login session across browser sessions.

Simple Implementation: It's straightforward to use localStorage in JavaScript, making it an accessible option for storing tokens.

Disadvantages:

Security Risks: localStorage is accessible via JavaScript, which means any JavaScript running on your page can access it. If there is a security vulnerability in your application (e.g., Cross-Site Scripting or XSS), an attacker could potentially steal the token.

No Expiry: Tokens stored in localStorage do not have a built-in expiration mechanism. You need to handle token expiration and renewal manually, which can be error-prone.

Limited to Same-Origin Policy: localStorage data is subject to the same-origin policy, meaning it can only be accessed by pages from the same origin (protocol, domain, and port). This can cause issues if you have multiple subdomains or if you want to share tokens with different parts of your application.

Limited Storage: localStorage has a limited storage capacity (usually around 5-10 MB per domain). If you store a lot of data or have multiple tokens to manage, you might encounter storage limitations.




### Why and how to use httpsCookie ?
Here are some advantages of using HttpOnly cookies for token storage:

Enhanced Security: HttpOnly cookies cannot be accessed via JavaScript. This provides an additional layer of security against common attacks like Cross-Site Scripting (XSS), where malicious scripts attempt to steal tokens from client-side storage.

Same-Origin Policy: Cookies are subject to the same-origin policy, which means they are automatically sent with every HTTP request to the domain they belong to. This simplifies the process of including the token in each request to your API, and it works seamlessly across subdomains.

Automatic Expiration: You can set an expiration time for cookies, and the browser will automatically handle their removal after that time. This simplifies token expiration management.

Secure Flag: You can set the "Secure" flag on cookies, ensuring they are only sent over HTTPS connections, adding an extra layer of security.




```js

const express = require('express');
const cookieParser = require('cookie-parser');
const jwt = require('jsonwebtoken');

const app = express();
app.use(express.json());
app.use(cookieParser());

// Secret key for JWT signing
const secretKey = 'your-secret-key';

// Middleware to check if a user is authenticated
const authenticate = (req, res, next) => {
  const token = req.cookies.token;

  if (!token) {
    return res.status(401).json({ message: 'Unauthorized' });
  }

  try {
    const decoded = jwt.verify(token, secretKey);
    req.user = decoded.user;
    next();
  } catch (err) {
    return res.status(401).json({ message: 'Invalid token' });
  }
};

// Route to login and set HttpOnly cookie
app.post('/login', (req, res) => {
  // Authenticate user (replace with your authentication logic)
  const user = { id: 1, username: 'exampleuser' };

  // Generate JWT and set it as an HttpOnly cookie
  const token = jwt.sign({ user }, secretKey, { expiresIn: '1h' });
  res.cookie('token', token, { httpOnly: true });
  res.json({ message: 'Login successful' });
});

// Protected route
app.get('/protected', authenticate, (req, res) => {
  res.json({ message: 'Protected route accessed' });
});

// Logout route
app.post('/logout', (req, res) => {
  res.clearCookie('token');
  res.json({ message: 'Logout successful' });
});

app.listen(3001, () => {
  console.log('Server is running on port 3001');
});

```

```js


import React, { useState } from 'react';
import axios from 'axios';

const App = () => {
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');

  const handleLogin = async () => {
    try {
      const response = await axios.post('http://localhost:3001/login', {
        username,
        password,
      });

      if (response.data.message === 'Login successful') {
        // Redirect to a protected route or update UI
      }
    } catch (error) {
      console.error('Login failed', error);
    }
  };

  const handleLogout = async () => {
    try {
      await axios.post('http://localhost:3001/logout');
      // Redirect to a login page or update UI
    } catch (error) {
      console.error('Logout failed', error);
    }
  };

  const handleProtectedAccess = async () => {
    try {
      const response = await axios.get('http://localhost:3001/protected');
      console.log(response.data);
    } catch (error) {
      console.error('Access to protected route failed', error);
    }
  };

  return (
    <div>
      <h1>Authentication Example</h1>
      <input
        type="text"
        placeholder="Username"
        onChange={(e) => setUsername(e.target.value)}
      />
      <input
        type="password"
        placeholder="Password"
        onChange={(e) => setPassword(e.target.value)}
      />
      <button onClick={handleLogin}>Login</button>
      <button onClick={handleLogout}>Logout</button>
      <button onClick={handleProtectedAccess}>Access Protected Route</button>
    </div>
  );
};

export default App;


```






