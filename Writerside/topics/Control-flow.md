# Control flow
Everything is a message

## If
Usual if is a `ifTrue:` message for Boolean type, that takes [codeblock](CodeBlocks.md) as argument.
There is 4 messages with all possible combination.
The most common to use is `ifTrue:ifFalse:`
```Scala
1 < 2 ifTrue: [1 echo]
1 > 2 ifFalse: [2 echo]
1 > 2 ifTrue: [3 echo] ifFalse: [4 echo]
1 < 2 ifFalse: [5 echo] ifTrue: [6 echo]
```
Try to say what numbers will be printed.  

The last 2 messages have the signature:   
`Boolean ifTrue: [ -> T] ifFalse: [ -> T] -> T` so they can be used as expressions   
Which means that the last expression of each branch is returned as a result.  
In the first example x will be equal 1:  
```Scala
// simple
x = true ifTrue: [1] ifFalse: [2]

// `exist` returns type Boolean, so we can send ifTrue:ifFalse: to it
config = directory exist 
    ifTrue: [ directory / "config.conf" |> readAll ]
    ifFalse: [ 
        path = directory / "config.conf"
        content = Config empty
        FileSystem write: path content: content
        content
    ]
```

In this theoretical program we are getting content of the config file if it exists or create it if not.  
Notice: codeblocks always return its last expression as value  

## If syntax sugar
Since if is quite often thing, I created syntax sugar for it:    
`true => expr` for `true ifTrue: [expr]`    
and    
`true => expr |=> expr`for `true ifTrue: [expr] ifTrue: [expr]`

```Scala
1 < 2 => "yay" echo
1 < 2 ifTrue: ["yay" echo]

// with else branch
1 > 2 => "yay" echo |=> "oh no" echo
1 > 2 ifTrue: ["yay" echo] ifFalse: ["oh no" echo]
```

## Match
There was no way to do this in original Smalltalk, so they used to create 
a table with match-values to codeblocks  

But since in niva I replaced inheritance with tagged unions I really need special syntax for matching:  

```Scala
x = "Alice"
// matching on x
| x
| "Bob" => "Hi Bob!"
| "Alice" => "Hi Alice!"
|=> "Hi guest"


// It can be used as expression as well
y = | x
    | "Bob" => "Hi Bob!"
    | "Alice" => "Hi Alice!"
    |=> "Hi guest"
y echo // "Hi Alice!" printed
```

Here example with [unions](Unions.md), despite you will learn them a little bit later:

```Scala
union Shape =
    | Rectangle width: Int height: Int
    | Circle    radius: Int
    
constructor Float pi = 3.14

// match on this(Shape)
Shape getArea -> Float = | this 
    | Rectangle => width * height |> toFloat
    | Circle => Float pi * radius * radius
    
// its exhaustive, so when u add new branch 
// all the matches will become errors until all cases processed
```

## If abuse
There are no if ifelse else syntax chain, but you can do it like with `? :` op in C
```Scala
mark = "b"
score = mark == "a" => 10 |=>
    mark == "b" => 8 |=>
    mark == "c" => 6 |=>
    mark == "d" => 4
// score is 8 
```

I've never been in a situation where it would be necessary, prefer simple ifs and matches, this is for some very corner cases.
    