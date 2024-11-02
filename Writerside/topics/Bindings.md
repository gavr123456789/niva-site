# Bindings

Here we will look at some more real world examples of writing code in niva.
https://github.com/gavr123456789/bazar/tree/main/Bindings  

## Simple example
To bind some external types and methods we use internal `Bind package:content:` method.  
Let's imagine we need to bind a type Person from Java.  
```Java
package org.person

public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public void sayHello() {
        System.out.println("Hello, my name is " + name + 
            " and I am " + age + " years old.");
    }

    public void changeAge(int newAge) {
        this.age = newAge;
        System.out.println(name + " is now " + age + " years old.");
    }

    public void introduceFriend(String friendName, int friendAge) {
        System.out.println(name + " has a friend named " + 
            friendName + " who is " + friendAge + " years old.");
    }

    public boolean isAdult(int legalAge) {
        return age >= legalAge;
    }

```

First we will add just type itself, since all fields are private, we will add it without them:  
```Scala
Bind package: "org.person" content: [
    type Person
]
```
Then lets bind getters and setters. For that we have additional `fields:` arg:  
```Scala
Bind package: "org.person" content: [
    type Person
] fields: [
    Person
        name: String
        age: Int
]
```
Now let's add methods.  
To bind `void changeAge(int newAge)` we will use keyword message `changeAge::Int -> Unit`  
  
With `introduceFriend(String friendName, int friendAge)` we have a problem:  
we can't just bind it because it contains 2 arguments, so first lets come up with a new name.  
  
It could be `introduceFriend: age: ` if we are pragmatic, but in Smalltalk methods usually named so it reads like English.  
`person introduceFriend: "Bob" age: 42` is not too good so lets make it longer but more Smalltalk like:  
```Scala
person introduceFriend: "Bob" withAge: 42
```
See, much better, even though it is longer now, we write code once and read dozens of times, 
so readability is much more important, plus the length is unimportant because the methods will be 
autocompleted by LSP.  

Try to come up with a name for the isAdult function.  
  
Full bind would look like:
```Scala
Bind package: "org.person" content: [
    type Person
    extend Person [
        on sayHello -> Unit
        on changeAge::Int -> Unit
        // your variant of `public boolean isAdult(int legalAge)`
        @rename: "introduceFriend" // real name of the Java function
        on introduceFriend::String withAge::Int -> Unit
    ] 
] fields: [
    Person
        name: String
        age: Int
]

```



## @rename:

`@rename` is useful when signature of niva method doesn't match with target 
matches: `1 from: 2 to: 3` == `1.fromTo(2, 3)`  
  
not: `HttpClient send::Request handler::Handlers` !=  
       `fun HttpClient.send(x: Request,y: Handlers){}` 
`send:handler:` will be transformed to `sendHandler` function, but we need just `send`.  

## @emit:
`@emit: "code"` will directly emit some backend lang code, kinda like asm pragma from C.  
This can be useful in some corner cases when just renaming with `@rename:` is not enough.  

### Bind a function without receiver
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
There is no such type as Console in Kotlin, but it looks pretty good still.

### change args order
Sometimes it can be useful to change the order of arguments, for example to bind a function for the type of the first real argument.  
Or just to make a better api with some shortcuts like here.  
For example look at this binding:

```Scala
  @emit: "$0.redirectOutput(ProcessBuilder.Redirect.INHERIT).redirectError(ProcessBuilder.Redirect.INHERIT)"
  ProcessBuilder redirectOutput -> Unit
```
$0 means the receiver of the method, in this case its ProcessBuilder.   
`$1` `$2` etc. is for other arguments by position.  
These numbers are checked at compile time. 
So a single
```Scala
p = ProcessBuilder command: {"gcc" "--help"} 
p redirectOutput
```
will generate: 
```Kotlin
val p = ProcessBuilder(litsOf("gcc", "--help"))
p.redirectOutput(ProcessBuilder.Redirect.INHERIT).redirectError(ProcessBuilder.Redirect.INHERIT)
```

Full file:
```Scala
Bind package: "java.lang" content: [
  type ProcessBuilder command: MutableList::String

  @emit: "ProcessBuilder($1).directory(java.io.File($2))"
  constructor ProcessBuilder command::List::String dir::String -> Unit

  type Process 
  
  ProcessBuilder start -> Process
  
  @emit: "$0.redirectOutput(ProcessBuilder.Redirect.INHERIT).redirectError(ProcessBuilder.Redirect.INHERIT)"
  ProcessBuilder redirectOutput -> ProcessBuilder
  
  Process waitFor -> Int

  @emit: """System.getProperty("user.dir")"""
  constructor Process currentDir -> String
]

```

