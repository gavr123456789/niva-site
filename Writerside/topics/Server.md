# Get currency rates via HTTP

We will use bindings to http4k and kotlinx.serialization lib, which you can get here  
- [http](https://github.com/gavr123456789/bazar/blob/main/Bindings/Net/http.bind.niva)  
- [json](https://github.com/gavr123456789/bazar/blob/main/Bindings/JSON/kotlinx.serialization.json.bind.niva)  
Just add it to the same folder as your main.niva file.  

## Client: GET currency

Let's use free API from https://api.frankfurter.dev/v1/latest to get current currency list.  

### Model

```Scala
@Serializable
type CurrencyResponce 
    amount: Double
    base: String
    date: String
    rates: Map(String, Double)
```

### Main 
I like creating a Program which has all deps for talking with outside world.  
Here we need only client, so lets create custom constructor that adds it, kinda dependency inject.
```Scala
type Program client: JavaHttpClient
constructor Program new = Program client: JavaHttpClient new 

Program new
```
(new is not a keyword, I'm using Scala syntax highlighting here)

### httpGet

Let's define the method for getting info. I will use [](Message-Declaration.md#extend) syntax here:

```Scala
extend Program [
    on httpGet: uri::String -> Response = [
        request = Request method: Method.GET uri: uri
        ^ client Request: request
    ]
]
```
This method is just a little helper.  
1) We are creating a request with `uri` from arg and `GET` kind
2) Then executing the `request` on the `client` will return Response

### getCurrency
`getCurrency` method will use our `httpGet` to get the json from [API](https://api.frankfurter.dev/v1/latest), 
parse it to our model and print it.
```Scala
    on getCurrency = [
        responce = .httpGet: "https://api.frankfurter.dev/v1/latest"
        responce status echo; bodyString echo
        curResponce = Json::CurrencyResponce decode: responce bodyString
        ^ curResponce
    ]
```
Here on the line `responce status echo; bodyString echo` I use [cascade](Pipes-and-Cascades.md#cascades) to send many 
messages to the same receiver.  
Then decode currency via [Json](https://github.com/gavr123456789/bazar/tree/main/Bindings/JSON) and printing its amount

### Put everything together
```Scala
type Program client: JavaHttpClient
constructor Program new = Program client: JavaHttpClient new 

@Serializable
type CurrencyResponce 
    amount: Double
    base: String
    date: String
    rates: Map(String, Double)


extend Program [
    on httpGet: uri::String -> Response = [
        request = Request method: Method.GET uri: uri
        ^ client Request: request
    ]
    on getCurrency = [
        responce = .httpGet: "https://api.frankfurter.dev/v1/latest"
        curResponce = Json::CurrencyResponce decode: responce bodyString
        ^ curResponce
    ]
]
// run
Program new getCurrency base echo
```
### One-liner
Here single expression version, that looks kinda Clojure

```Scala
URI = "https://api.frankfurter.dev/v1/latest"
cur = Json::CurrencyResponce 
    decode: ((JavaHttpClient new) 
                Request: (Request 
                            method: Method.GET 
                            uri: URI)
            ) bodyString
cur rates at: "USD" |> 
    echo
```

