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

