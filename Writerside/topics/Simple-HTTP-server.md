# Simple HTTP server

In [](Server.md) we used only client, here we will host a server with GET and POST routes.  
We will use same 2 libraries:  
- [http](https://github.com/gavr123456789/bazar/blob/main/Bindings/Net/http.bind.niva)
- [json](https://github.com/gavr123456789/bazar/blob/main/Bindings/JSON/kotlinx.serialization.json.bind.niva)

## Model
We will use simple Person model with 2 fields 
```Scala
@Serializable
type Person name: String age: Int 
```
## Get route
Let's declare `path` with ... path and method kind `GET`.  
And just bind it to codeblock that will be executed.
```Scala
path = "/hi" bind: Method.GET
route = path to: [
    response = Response status: Status.OK
    response body: "Hello, from niva okhttp server"
]
```
## Post route
```Scala
pathPost = "/person" bind: Method.POST
routePost = pathPost to: [
    person = Json::Person decode: it body strPayload 
    response = Response status: Status.OK
    response body: person toString
]
```
Everything is pretty similar here but we're also parse the `person` from `strPayload` of the body, 
and answer with its string representation.  

## Start
Collect all routes and start the server:  
```Scala
routes = Router routes: {route, routePost}
routes asServer: (SunHttp port: 9000) |> start
```

## Checking
You can run  
`curl -v http://localhost:8080/hi`  
or   
```
curl -X POST http://localhost:9000/person \                
                      -H "Content-Type: application/json" \
                      -d '{"name": "Alex", "age": 25}'
```
in your terminal to check that everything is working.  

Or just check it from the code 
```Scala
client = JavaHttpClient new
// printing client prints everything from responce
printingClient::HttpHandler = PrintResponse new |> then: client

request = Request method: Method.GET uri: "http://localhost:9000"
responce = printingClient Request: request // send request, the client will print it
```

## Put everything together
```Scala
@Serializable
type Person name: String age: Int 

// curl -v http://localhost:8080/hi
path = "/hi" bind: Method.GET
route = path to: [
    response = Response status: Status.OK
    response body: "Hello, from niva server"
]

pathPost = "/person" bind: Method.POST
routePost = pathPost to: [
    person = Json::Person decode: it body strPayload 
    response = Response status: Status.OK
    response body: person toString
]
routes = Router routes: {route, routePost}
routes asServer: (SunHttp port: 9000) |> start

// one liner

```

### One-liner
```Scala
Router routes: {(
    "/example" bind: Method.GET |> 
               to: [ Response status: Status.OK |> 
                              body: "Hii! niva server"])} |>
    asServer: (SunHttp port: 9000) |> 
    start
```