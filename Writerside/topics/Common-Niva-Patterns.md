# Common Niva Patterns

1) Decompose your task on types
2) Represent all possible states as tagged unions
3) Add method for union root which will match and do something specific for each branch
Here is an example of a json generator
```Scala
// main.niva

// 2) Represent all possible states as tagged unions
union JsonObj =
| JsonArray arr: MutableList::JsonObj
| JsonNumber num: Double
| JsonString str: String
| JsonFields fld: MutableMap(String, JsonObj)

// 3) Add method for union root what to do in every branch
JsonObj toJson -> String = | this 
| JsonArray => this arrToJson
| JsonNumber => num toString
| JsonString => str packForJson 
| JsonFields => this fieldToJson

//Creating specific methods for generation of JsonArray and JsonFields
JsonArray arrToJson -> String = [

  createArr = [
    b = StringBuilder new
    arr forEachIndexed: [ i, it ->
      b append: it toJson 
      i + 1 != arr count => b append: ", "
    ]
    b toString
  ]

  ^createArr do surroundWith: "[ " and: " ]" 
]

JsonFields fieldToJson -> String = [

  createObj = [
    b = StringBuilder new
    mut i = 0
    fld forEach: [ k, v ->

      b append: k packForJson |>
        append: ": " |>
        append: v toJson 
      i + 1 != fld count => b append: ", "

      i <- i inc
    ]
    b toString
  ]

  ^createObj do surroundWith: "{ " and: " }" 
]

jsonNum = JsonNumber num: 5.0
jsonStr = JsonString str: "t'e'x't"
jsonFields = JsonFields fld: #{"a\ng'e" jsonNum}

arr = JsonArray arr: {jsonNum jsonNum jsonStr jsonFields}
arr toJson echo 

```

strUtils.niva
```Scala
String surroundWith::String and::String = 
  surroundWith + this + and

String surroundWith: x::String = 
  x + this + x

String packForJson -> String = 
   this escape surroundWith: "\""

String escape = 
  this replace: "\\" with: "\\\\" |>
       replace: "\"" with: "\\\"" |>
       replace: "\n" with: "\\n" |>
       replace: "\r" with: "\\r" |>
       replace: "\t" with: "\\t" |>
       replace: "\'" with: "\\'" 
```

I recommend you to try writing such json generator yourself(or maybe YAML?)

Some good points in this "Design Patterns in Elixir" talk:  
[](https://youtu.be/agkXUp0hCW8?t=1114)
