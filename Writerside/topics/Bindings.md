# Bindings

- [](Files.md)
- [](Server.md)
- [](GTK-UI.md)


## @rename:

`@rename` is useful when signature of niva method doesn't match with target 
matches: `1 from: 2 to: 3` == `1.fromTo(2, 3)`  
  
not: `HttpClient send::Request handler::Handlers` !=  
       `fun HttpClient.send(x: Request,y: Handlers){}` 
`send:handler:` will be transformed to `sendHandler` function, but we need just `send`.  
  In such cases we can use `@rename`:
```Scala
Bind package: "java.net" content: [
    type URI
    constructor URI new::String -> URI
]

Bind package: "java.net.http" content: [
    type HttpRequest value: String
    constructor HttpRequest newBuilder -> HttpRequest
    HttpRequest uri::URI -> HttpRequest
    HttpRequest build -> HttpRequest

    type BodyHandlers

    type HttpResponse
    HttpResponse body -> String
    @rename: "BodyHandlers.ofString"
    constructor HttpResponse ofString -> BodyHandlers

    type HttpClient value: String
    constructor HttpClient newHttpClient -> HttpClient
    @rename: "send"
    HttpClient send::HttpRequest handler::BodyHandlers -> HttpResponse
]

client = HttpClient newHttpClient
request = HttpRequest newBuilder uri: (URI new: "https://postman-echo.com/get" |> build)
response = client send: request handler: (HttpResponse ofString)
response body echo
```
## @emit:
Since everything in niva should have a receiver, here is a common approach on binding functions without receiver:
1) create fake type
2) create the constructor
3) add @emit pragma

```Scala
Bind package: "kotlin.io" content: [
    type Console
    
    @emit: "readln()"
    constructor Console readln -> String
    // from niva: Console readln
]
```