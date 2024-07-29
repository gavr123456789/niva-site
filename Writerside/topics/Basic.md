# Basic Syntax


```Scala
"Hello world" echo
```

`niva run main.niva`  
Instead of `echo("Hello world")` we use `"Hello world" echo`




## Message send

Everything in niva except the type declaration is a message sending, kinda like lisp, but no parentheses and typed.

So, let's get familiar with message syntax.



// add badge
To run the code, create a file main.niva and run it with `niva run`  
It will take longer for the first time


## Structured by AST

1) Declarations
   1) [Type](Type-declaration.md) 
   2) [Message](Message-Declaration.md)
   3) [Constructor](Constructors.md)
   4) [Variable declaration](Variables.md)
   5) [Unions](Unions.md)
   6) [Enums](Enum.md)
2) Expressions
   1) [Message send](Message-Send.md)
   2) Control flow TODO
   3) [Code blocks](CodeBlocks.md)
   4) [Pipes](Pipes-and-Cascades.md)
   5) [Cascade](Pipes-and-Cascades.md)


