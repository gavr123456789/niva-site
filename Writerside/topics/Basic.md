# Basic Syntax


```Scala
"Hello world" echo
```

`niva run main.niva`  
Instead of `echo("Hello world")` we use `"Hello world" echo`

## Message send 
![smalltalk rules.png](smalltalk rules.png)

Everything in niva except the type declaration is a message sending, kinda like lisp, but no parentheses and typed.

So, let's get familiar with message syntax.



// add badge
To run the code, create a file main.niva and run it with `niva run`  
It will take longer for the first time 

## Structured by AST

1) Declarations
   1) [Type](Type-declaration.md) 
   2) [Message](Message-Declaration.md)
   3) Constructor
   4) Variable declaration
2) Expressions
   1) Message send
   2) Control flow
   3) Code blocks
   4) Pipes
   5) Cascade
