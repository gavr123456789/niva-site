
# Message declaration

`Type name = body` -  declaration  
`obj name` - call   

## Cheat Sheet
```Scala
type Foo
foo = Foo new

// unary
Foo unary = 
    1 echo

foo unary

// binary
Foo + x::Int =
    (x + 1) echo
    
foo + 5   
  
// keyword 
Foo from::Int to::Int = 
    from + to 

foo from: 1 to: 2
```
## Unary 
```Scala
Type name = body // declaration
instance name    // call
Int add2 = this + 2 // declare message for Int
  1 add2            // call
```
Declarations and calls always have same signature.  
The only difference is that Declarations use type names and calls use instances of these types.  
  
New messages can be declared for any types, custom or internal like Int.  
## With body
When there you need many statements use square brackets:
```Scala
type Foo field: Int
Foo birthday = [
    x = field inc
    y = field dec
    
    ^ x + y // in messages with body implicit return is needed
]
```


## Return type
`-> Type` is used to declare return type
```Scala
Int add2 -> Int = this + 2
```
If its not declared it will be inferred. 

## Keyword

```Scala
// ReceiverType key1 key2 = [...]
Int from::Int to::Int = 
    from + to + this

1 from: 2 to: 3 // 6
```
### Local arg names
If the key is not well suited for the name of a local variable, 
there is a syntax for declaring a separate name for them.

```Scala
Int for: x::Int except: y::Int -> Int = [
    ^ x + y
]
```

## Single expression form
If the method body consists of a single expression,
then you can do without a block of code:
```Scala
Int add::Int = this + add
```
With short form you can omit the return type(`-> Int`) and return statement(`^`)
```Scala
// same thing
Int add::Int = [
    ^ this + add
]
```

## this
`this` is an implicit argument of any method representing a receiver

```Scala
Int add: x::Int -> Int = [
    ^ this + x
]
result = 1 add: 2
result echo
```
Try to run this example `niva filename.niva`  

There is also syntax sugar, just `.` can be used instead of `this`
```Scala
    type Foo
    foo = Foo new
    Foo bar = "hi!!!" echo
    Foo zig = [
        this bar
        .bar // same thing
    ]
    
    
```

## Fields
All the fields of the type are added in to scope of the method
```Scala
type Rectangle x: Int y: Int

Rectangle area = [
    ^ x * y
]
```


## extend
Syntax sugar to declare many messages for the same type
```Scala

type Person

extend Person [
    on unary = 1 echo
    on + binary::Int = binary echo
    on key::Int word::String = key echo
    on withLocalName: x::Int = x echo
]
```

## Binary
I would not recommend to create a custom binary messages(i.e. operator overloading),
usually keyword message with one arg is more readable.  

But here are rare cases when its really suits, for example for some DSL purposes.
```Scala
11d/9m/1998y
```
here we have 5 message calls, 3 unary `d` `m` `y` and 2 binary `/` between them.  
Since unary always evals first it is possible to create such DSL for dates.
Same for file path, for example message like
```Scala
"home" / "user" / "music"
```
Could create a path with proper delimiters(`/` on Unix and `\` on Windows)


```Scala
// ReceiverType op arg = [...]
Path / x::String = 1 echo
```

And ofc its not possible to overide the defaults operators like `Int + Int`
