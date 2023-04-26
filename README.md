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
