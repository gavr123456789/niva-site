# Binding

## Simple example

```Scala
Bind package: "java.math" content: [
    type BigDecimal value: String
    BigDecimal + BigDecimal -> BigDecimal
    BigDecimal - BigDecimal -> BigDecimal
    BigDecimal * BigDecimal -> BigDecimal
    BigDecimal / BigDecimal -> BigDecimal
]

Bind package: "kotlin.text" content: [
    String split::String -> List::String = []
]

"hi Mark" split: " " // {}
```

## Load package from maven central
`Project loadPackages: List::String`  
```Scala
Project loadPackages: {"org.jetbrains.kotlinx:kotlinx-datetime:0.4.1"}
Bind package: "kotlinx.datetime" content: [
    type Clock
    type Instant
    type LocalDateTime hour: Int
    type TimeZone id: String
        constructor TimeZone currentSystemDefault
        constructor TimeZone of::String = []
    type FixedOffsetTimeZone
    Clock now -> Instant
    Instant toLocalDateTime::TimeZone -> LocalDateTime
    // ...
] 
```

## @rename:

`@rename` is useful when signature of niva method doesn't match with target  
- matches: `1 from: 2 to: 3` == `1.fromTo(2, 3)`  
- not: `HttpClient send::Request handler::Handlers` != `fun HttpClient.send(x: Request,y: Handlers){}`  
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

### Change the order of the arguments
Sometimes you need arguments in different order, or even receiver, you can do it with $N  
where $0 is the receiver, and #1-N is the message arguments

```Scala
Project loadPackages: {"io.exoquery:pprint-kotlin:2.0.2"}
Bind package: "io.exoquery" content: [
  @emit: "pprint($0)"
  Any pprint -> String
]

1 pprint
"sas" pprint
Person name: "Alice" age: 24 |> pprint
```
