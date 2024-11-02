# Files

Let's write a little program which will read a file.  
First of all we need to bind a function which reads the file since niva std 
doesn't contain any IO besides echo.  
This was done for a purpose, since other backends are possible in the future, for example imagine having js browser 
platform, but with readline or readfile function in std, it would be strange and error-prone.  

## Bind file API

For a tutorial we will bind java.io.File since it has very easy to use api.  
But we have a complete binding of great library [okio](https://square.github.io/okio/), it's [here](https://github.com/gavr123456789/bazar/blob/main/Bindings/okio.bind.niva)

So here is a snippet we need to bind:
```Kotlin
import java.io.File
fun foo() {
    val fileContent = File("path/to/file.txt").readText()
    println(fileContent)
}
```
Let's create a `javaio.bind.niva` file, I prefer to add `.bind` for binding files, I think its a nice convention.

The bindings would look like: 
```Scala
Bind package: "java.io" content: [
  type File path: String
  File readText -> String
]
```
Pretty straight forward, I even not sure what to explain here.  
To bing external lib from any jvm lang we need to create a message to `Bind` with 2 params:
1) package - simply the name of a package that will be imported in every file where content of bind is used
2) content - the main part, signatures of what we bind

## Main
Lets create a `main.niva` file 

TODO :(