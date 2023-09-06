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

## How cookie sent from frontend to backend

In the frontend, when you make a request to the backend, the browser automatically includes the HttpOnly cookie in the request headers. You don't need to explicitly send the cookie data from the frontend; the browser takes care of this for you. Here's how it works:

Login Request: When a user logs in on the frontend, and you set the HttpOnly cookie on the server (as shown in the previous example), the cookie is stored by the browser.


## How to check cookie 

```js


import React from 'react';
import { Route, Redirect } from 'react-router-dom';

const ProtectedRoute = ({ component: Component, ...rest }) => {
  // Check for the presence of the authentication token (e.g., 'myToken') in cookies
  const token = document.cookie.split('; ').find(row => row.startsWith('myToken='));

  return (
    <Route
      {...rest}
      render={(props) =>
        token ? (
          <Component {...props} />
        ) : (
          <Redirect to="/login" />
        )
      }
    />
  );
};

export default ProtectedRoute;


```



Subsequent Requests: When the user makes subsequent requests to the backend (e.g., accessing protected routes), the browser automatically includes the cookie in the request headers. You can see this in the Network tab of your browser's developer tools.

### How HttpsOnly cookie anhance security ?

Here's how HttpOnly cookies enhance security:

Inaccessible from JavaScript: HttpOnly cookies cannot be accessed or modified by JavaScript running in the browser. The browser enforces this restriction. As a result, attackers cannot use common techniques like Cross-Site Scripting (XSS) to steal tokens from client-side storage.

Same-Origin Policy: Cookies, including HttpOnly cookies, are subject to the same-origin policy, which means they can only be accessed by pages from the same origin (i.e., same protocol, domain, and port). This further restricts the attack surface.

Secure Transmission: HttpOnly cookies are typically transmitted securely over HTTPS, making them resistant to network-based attacks.

However, it's essential to keep in mind that while HttpOnly cookies provide strong protection against client-side attacks, they do not protect against other security threats, such as:

Server-side vulnerabilities: If your server-side code is vulnerable to attacks like SQL injection or remote code execution, attackers might gain access to authentication tokens directly from your server's storage.

Cross-Site Request Forgery (CSRF): CSRF attacks involve tricking a user into making unwanted requests to your server using their authenticated session. To protect against CSRF attacks, you should implement anti-CSRF measures like using CSRF tokens.

Phishing: Attackers may use phishing techniques to trick users into revealing their credentials or tokens. Educate users about safe browsing practices to reduce the risk of falling victim to phishing attacks.





## how to use cookie properly from frontend to backend

- Backend in order to recevive cookie from different domains such as below domain it should have following parameter setup in cookie-parser middleware
- This will parse the cookie that backend receive under "req.cookies".
```js
app.use(cors({ origin: "http://localhost:3000", credentials: true }));
```
- To send cookies like httpOnly cookie to a response we can send it like below

```js
res.cookie("secret_token", secret_token, { httpOnly: true }); // send http cookie to frontend for further communication
    return res
      .status(200)
      .json({ status: true, data: { authData, secret_token } });
```

### Please not in frontend to receive properly the cookies and set it to the cookies table of browser it  is necesary to do withCredentials as true to receivei and set cookie properly like below


- Setting withCreds as true to get and set cookie properly 
```js
import axios from "axios";
import { BASE_URL } from "../../constants/staticurls";
const API = axios.create({ baseURL: BASE_URL, withCredentials: true });

export const loginApi = (secret_token) =>
  API.post(`/auth/login/${secret_token}`);
```


- In subsequent request as withCredentials as true will sent back the cookie to server which is available under req.cookies
```js
import axios from "axios";
import { BASE_URL } from "../../constants/staticurls";

const API = axios.create({ baseURL: BASE_URL, withCredentials: true });

const config = {
  headers: {
    "Content-Type": "multipart/form-data",
  },
};
export const getAllLiveClassesApi = () =>
  API.get("/schedule-live-class/get-all");
export const createLiveClassApi = (data) =>
  API.post("/schedule-live-class/create", data, config);
export const getLiveClassDetailsApi = (roomId) =>
  API.get(`/schedule-live-class/get-details/${roomId}`);
export const getUpcomingClassApi = (roomId) =>
  API.get(`/schedule-live-class/get-upcoming-class/${roomId}`);

```




