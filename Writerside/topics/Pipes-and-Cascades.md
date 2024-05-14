# Pipes and Cascades
// TODO
## Pipes
You can chain kw messages with pipes  
`1 from: 2 to: 3` is single message with two arguments  
`1 from: 2 |> to: 3` is 2 messages with one argument each  

## Cascades
https://en.wikipedia.org/wiki/Method_cascading  


`a b; c; d` has same effect as
```Scala
a b
a c
a d
a
```

With the help of cascade you can turn any API
into a builder pattern, even if this was not intended!

```Scala
btn = (Button new); 
    label: "Hello Adw";
    hexpand: true;
    vexpand: true;
    onClicked: [
        "clicked $n times!" echo
        n <- n inc
    ]
// same as
btn = Button new
btn label: "Hello Adw"
btn hexpand: true
btn vexpand: true
btn onClicked: [
    "clicked $n times!" echo
    n <- n inc
]
```

## Combine |> with ; together
Sometimes it can be useful, but pls don't do it.
```Scala
String boil -> Nothing!Error =
  Error throwWithMessage: "missing $this var"

String validate -> Unit!Error = [
  URL = "URL"
  SHA = "SHA"
  VERSION = "VERSION"

  this
    contains: URL     |> ifTrue: [URL boil];
    contains: SHA     |> ifTrue: [SHA boil];
    contains: VERSION |> ifTrue: [VERSION boil]
]

recipe_template = "SHA:234234234"
recipe_template validate

```