# LearnXinY style


Variable:

```Scala
a = 21
b = a + a
c::String = "string" // type declaration is ::
d = true
mut x = 5
x <- 6 // mutate
```

Type:  

```Scala
type Square side: Int
type Person
    name: String
    age: Int
```

Create instance:
```Scala
square = Square side: 42
alice = Person name: "Alice" age: 24

// destruct fields by names
{age name} = alice
```

Method for type:  

```Scala
Int double = this + this
Square perimeter = side double

square = Square side: 42
square perimeter // call
```

Type constructor:  
its like a message for type itself instead of instance
```Scala
constructor Float pi = 3.14
x = Float pi // 3.14
```

Functions with many args:
```Scala
type Range from: Int to: Int
Int to::Int = Range from: this to: to

1 to: 2 // Range
```

Conditions
=> is syntax sugar for ifTrue message, since conditions is pretty common
```Scala
// syntax sugar
1 < 2 => "yay" echo
// everything is message send(this is free because of lambda-inlining)
1 < 2 ifTrue: ["yay" echo]

// with else branch
1 > 2 => "yay" echo |=> "oh no" echo

1 > 2 ifTrue: ["yay" echo] ifFalse: ["oh no" echo]
```

Cycles:  
there is no special syntax for cycles
```Scala
{1 2 3} forEach: [ it echo ]
1..10 forEach: [ it echo ]

mut c = 10
[c > 0] whileTrue: [ c <- c dec ]
```

Matching:
there is special syntax for matching, since niva heavily utilize tagged unions
```Scala
x = "Alice"
// matching on x
| x
| "Bob" => "Hi Bob!"
| "Alice" => "Hi Alice!"
|=> "Hi guest"


// It can be used as expression as well
y = | "b"
    | "a" => 1
    | "b" => 2
    |=> 0
y echo // 2
```

Unions

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

Nullability
```Scala
x::Int? = null
q = x unpackOrPANIC
x unpack: [it echo]
w = x unpack: [it inc] or: -1
e = x unpackOrValue: -1
```

Handling the error:
```Scala
x = file read orPANIC
x = file read orValue: "no file"
```
Look for more in [](Error-handling.md)

## Misc

Local arg names:
```Scala
Int from: x::Int to: y::Int = this + x + y
```

Syntax sugar for this
```Scala
Person foo = [
    .bar
    this bar // same thign
]
```

Compile time reflection
```Scala
Foo bar::Int baz::String = [
    // getting string representation from call side
    a = Compiler getName: 0
    b = Compiler getName: 1
    c = Compiler getName: 2
    a echo
    b echo
    c echo
]
```