# Introduction
Niva is strongly inspired by the forgotten [Smalltalk](https://www.codeproject.com/Articles/1241904/Introduction-to-the-Smalltalk-Programming-Language) language.  
[How learning Smalltalk can improve your skills as a programmer](https://medium.com/smalltalk-talk/how-learning-smalltalk-can-improve-your-skills-as-a-programmer-fc5b2f56685)  

Smalltalk tried to minimize the number of parentheses in it's syntax while remaining very similar to Lisp in its nature.  
Instead of everything being a S-expression, everything is a message send.  
So we can say that niva is a typed Lisp with much less parentheses, + methods and tagged unions.    

On an imaginary graph of complexity, I would put it here:  
Go < Niva < Java < Kotlin < Scala



Niva is my experiment in creating a typed Smalltalk with some crazy "what if" features.

-------  
Here some bullet points:
- Minimalistic Smalltalk-like syntax with types
- Tagged unions are the main way to achieve polymorphism instead of inheritance.
- Prefer static to dynamism, the code is easier to understand because you always know which specific method will be called.
  - No method overloading for same receiver, no methods @override, no late binding
- There are no functions, everything is method, in other words there 
is always a receiver, `"Hello" echo` instead of `echo("Hello")`, 
print is a message for String, not a top level function
- Heavy lambda utilization. They are much easier to create(`[42]` is valid lambda that returns 42) and free to use thanks to inlining
- There are no if\while\do while statements, everything is a message send, like in Smalltalk with a few exceptions for type declaration and matching
- Null safety (Swift\Kotlin like nullable types)
- Errors are between values and exceptions(Nim\Roc-like effects) and all possible errors of the scope is union that can be exhaustively matched
- No imports. Nowadays modern IDEs are making imports for you, so why not
move this feature to the language level. As long as the types do not completely match in name and field names, the imports will be inferred automatically
- Application level language. 
  - Its not about moving bits and bytes around. JVM is fast enough [1](https://www.techempower.com/benchmarks/#hw=ph&test=json&section=data-r22) [2](https://github.com/kostya/benchmarks?tab=readme-ov-file#mandelb) and can be compiled to static optimized binary with [GraalVM](https://www.graalvm.org/latest/reference-manual/native-image/)


![smalltalk rules.png](smalltalk rules.png)

---

## Syntax without much explanation
Here are syntax examples to give you a general impression. Everything will be explained in detail later.
```Scala
// declare type with 2 fields
type Person name: String age: Int
person = Person name: "Alice" age: 24 // instantiate

person name echo // get name and print it 
personWithNewName = person name: "new name"

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

## Target
Right now Niva targets JVM, and provide easy way to call any Java\Kotlin code.  
Im planning to add more compilation targets for faster comp-time in the future.