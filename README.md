# Next js 

- Has server side rendering support means html page is prebuild at server side and then show it to the client side
- React js has client side rendering means the data is rendering after gettting from server
- Next js uses file based routing and has in built router.
- Good for SEO optimization
- Automatic code splitting
- Render automatically
- Automate several functions

## Fundamentals

- To use client side render for a component
- within app all component are react server component, they rendered on server side
- to turn a componet into client rendered we need to use a directive at top of that component "use client"


## when to use server componnet and client component?
### Nested routes
- create nested folder within app to achieve nested routes
### Dynamic routes


## Data fetching in Next js
#### 1 - Server side rendering:-
- Dynamic data from server every time

#### 2 - static side rendering:-
- by default uses this , it fetches data and caches it maybe this data not changes quite often

#### 3 - Incremental static generation:-
- feture of above two rendering using {next:{revalidate:10}}
- Data fetched after certain amount of times

## Next js provides serverless function also
- we can't have route and page together
- don't create route within folder instead put inside api/foldernane/route.js


## Managing Metadata
- Static metadata:-
  - 
- Dynamic metadata






