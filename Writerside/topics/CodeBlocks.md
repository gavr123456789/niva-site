# CodeBlocks
//TODO  
Codeblocks are blocks `[]` with code in it.

To eval one send `do` message to it.  
Blocks can have arguments, then you need to send them as a keyword message:  
## Simple 
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

## Return block from block
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

## LambdaCalculus Prove
```Scala
truly  = [t::[ -> Unit], f::[ -> Unit] -> t do]
falsy = [t::[ -> Unit], f::[ -> Unit] -> f do]

ify = [c::[[ -> Unit], [ -> Unit] -> Unit], t::[ -> Unit], f::[ -> Unit] -> c x: t y: f]


ify c: truly t: ["true" echo] f: ["false" echo]
ify c: falsy t: ["true" echo] f: ["false" echo]

```

