# CodeBlocks
A code block is a collection of statements - the basic 
units of a language(it's a lambda)

```Scala
[
    x = 1
    y = 2
    x + y
]
```

The type of such block is `[ -> Int]` because the last expression of the block is its result.  
From this you could already guess that blocks can be assigned to variable and executed:
```Scala
x = [1 + 2]
x do echo
```

So blocks are kinda lazy evaluated code. 
They can store values from outside:
```Scala
w = 21
x = [w * 2]
x do echo
```

This can be used to reduce the coherence of the code.  
Put the dependencies necessary for the computation inside block in one place, 
and send the block to another!  

So you can have many producers and one consumer, meaning nothing specific about the producers.
// TODO link to example from other chapter

## Blocks for code organization 

```Scala
isSHA = [
    x = "a/b/c/d"
    y = FileSystem readAll: x
    y contains: "SHA"
] do
```

With eval-in-place blocks, you can organize complex calculations 
without noising your scope with temporary variables.

## CodeBlocks with args

```Scala
add2nums = [a::Int, b::Int -> a + b]
```
Since blocks always return the last value, `->` here is used to separate the list of arguments and statements. 
I call it consistency.  

Now instead of `do` we will send the args itself to it:
```Scala
result = add2nums a: 21 b: 21
```

## Send blocks as arguments

```Scala
// btw [ -> ] means no arg no return
Int repeat::[ -> ] = [
  repeat do
  (this - 1) repeat: repeat 
]
5 repeat: [ "UwU" echo ]
```

If block has args then the types themselves become arguments for a block

```Scala
Int add2num::[Int, Int -> Int] = [
  x = add2num Int: this Int: 31 // types are args
  x echo
]
11 add2num: [a, b -> a + b]
```
What is the answer?

## It
If a block of code accepts a single argument, you can omit it and use `it`:
```Scala
Int double::[Int -> Int] = [
  x = add2num Int: this
  x echo
]
5 double: [it * it]
5 double: [a -> a * a] // same thing
```

This is very often used in methods for collections:
```Scala
TODO filter map ...
```

But do not forget to give unique names if you call 
one block inside another, niva allows name clashes between different scopes:

```Scala
{{1 2 3} {4 5 6} {7 8 9}} forEach: [
  it forEach: [
    // now I cant use the it from the upper scope, it clashed
  ]
]

{{1 2 3} {4 5 6} {7 8 9}} forEach: [ array ->
  it forEach: [ elem ->
    elem echo
  ]
]
```


## Just more examples 
### Simple 
```Scala
// block with no argument
o = [
  // some complicated computations
  1 + 41
] do // call block in place, so o == 42

// block with q argument
x = [q::Int -> q + 5]
w = x q: 2 // call block with arguments
w echo

lambda = [str1::String, str2::String -> str1 + " " + str2]
e = lambda str1: "Hello" str2: "World!"
e echo


Int from::[Int, String -> Unit] = [
    from Int: 1 String: "sas"
    1 echo
]

1 from: [x::Int, y::String -> x echo]
```

### Return block from block
```Scala
x = [
    x = 5 + 5
    y = x - 1
    z = x + y + 22
    z
] do

y = x + 1
y echo

q = [
    x = 5 + 5
    y = x - 2
    z = x + y + 24
    [
        x + y + z + y
    ]
] do do
w = q + 1
w echo


```

### LambdaCalculus Prove
This is funny example of hardcore blocks utilization
```Scala
truly  = [t::[ -> ], f::[ -> ] -> t do]
falsy = [t::[ -> ], f::[ -> ] -> f do]

ify = [c::[[ -> ], [ -> ] -> ], t::[ -> ], f::[ -> ] -> c x: t y: f]


ify c: truly t: ["true" echo] f: ["false" echo]
ify c: falsy t: ["true" echo] f: ["false" echo]

```

