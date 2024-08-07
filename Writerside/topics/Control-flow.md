# Control flow
Everything is a message

## If
Usual if is a ifTrue message for Boolean type, that takes codeblock as argument
`1 < 2 ifTrue: ["obviosly" echo]`
`1 > 2 ifFalse: ["yep" echo]`
`1 < 2 ifTrue: ["obviosly" echo] ifFalse: ["waa!?" echo]`
`1 < 2 ifFalse: ["obviosly" echo] ifTrue: ["waa!?" echo]`
The last 2 message has signature 
`Boolean ifTrue: [ -> T] ifFalse: [ -> T] -> T` so they can be used as expressions
```Scala
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
`bool => expr` for `bool ifTrue: [expr]`  
and  
`bool => expr |=> expr`for `bool ifTrue: [expr] ifTrue: [expr]`

```Scala
1 < 2 => "yay" echo
1 < 2 ifTrue: ["yay" echo]

// with else branch
1 > 2 => "yay" echo |=> "oh no" echo
1 > 2 ifTrue: ["yay" echo] ifFalse: ["oh no" echo]
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

## Match
There was no way to do this in original Smalltalk, so they used to create 
a table with match-values to codeblocks  

But since in niva I replaced inheritance with tagged unions I really need special syntax for matching.  

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

Here example with unions, despite you will learn them a little bit later:

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





    