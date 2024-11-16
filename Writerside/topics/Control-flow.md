# Control flow

## If
Usual if is a `ifTrue:` message for Boolean type, that takes [codeblock](CodeBlocks.md) as argument.  
It will evaluate this block if it was send to true.  
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
x = true ifTrue: [1] ifFalse: [2]
y = 1 inc inc > 5 dec dec ifTrue: [1 + 5] ifFalse: [5 - 1]
// 3 > 3 
// y = 4
```

In this theoretical program we are getting content of the config file if it exists or create it if not.  
Notice: codeblocks always return its last expression as value

## Match
There was no way to do this in original Smalltalk, so they used to create 
a table with match-values to codeblocks  

But since in niva I replaced inheritance with tagged unions I really need special syntax for matching.  
```Scala
| x // match against x
| 1 => x inc echo // if x == 1 then increment it and print
| 2 => x dec echo
|=> x echo // else\default branch
```
You can see that else branch, is just a usual branch where condition is missing  
`| 2 => x dec echo`  
`| ??? => x echo`  
`|=> x echo`

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


## If syntax sugar
Since if is quite often thing, I created syntax sugar for ifTrue:    
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
You can find consistency between if and match.

`| match againts => do`
`conditiont => do`

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
    