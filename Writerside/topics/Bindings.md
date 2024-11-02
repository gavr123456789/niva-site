# Bindings

Here we will look at some more real world examples of writing code in niva.
https://github.com/gavr123456789/bazar/tree/main/Bindings  

## Simple example
To bind some external types and methods we use internal `Bind package:content:` method.  
Lets imagine we need to bind a type Person from Java.  
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