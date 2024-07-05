# Introduction

It will be Smalltalk like language, but statically typed.
Niva targets JVM, and provides easy way to call any Java\Kotlin code.  
On an imaginary graph of complexity, I would put it here:  
Go < Niva < Java < Kotlin < Scala

Niva is strongly inspired by the forgotten Smalltalk language  

![smalltalk rules.png](smalltalk rules.png)

Niva combines some OOP and functional properties  
  
OOP  
- There are no functions, everything is method, in other words there 
is always a receiver (`"Hello" print` instead of `print("Hello")`)

Functional  
- There are no inheritance, interfaces, abstract classes. Instead, tagged unions are heavily utilized.  
- This greatly reduces dynamism, the code is easier to understand because you always know which specific method will be called.
- There are no if\while\do while, everything is a message send, like in Smalltalk


## Syntax without much explanation
Here are syntax examples to give you a general impression. Everything will be discussed in detail below.
```Scala
// declare type with 2 fields
type Person name: String age: Int
person = Person name: "Alice" age: 24 // instantiate

person name echo        // get 
person name: "new name" // set

// unary method declaration
Person hi = "Hi! my name is $name" echo
person hi // unary call

// method with args
Person foo::Int bar::Int = [
    age + foo + bar |> echo // same as
    (age + foo + bar) echo 
]
person foo: 1 bar: 2 // 27 printed

union Shape =
| Rectangle width: Int height: Int
| Circle    radius: Int

constructor Float PI = 3.14
Float PI // constructor call
```