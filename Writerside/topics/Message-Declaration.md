
# Message declaration

`Type name = body` -  declaration  
`obj name` - call   
## Unary 
```Scala
Type name = body // declaration
instance name    // call
Int add2 = this + 2 // declare message for Int
  1 add2            // call
```

As you can see, the declaration and the call are consistent.


## Return type
`-> Type` is used to declare return type
```Scala
Int add2 -> Int = this + 2
```

## Keyword

```Scala
// ReceiverType key1 key2 = [...]
Int from::Int to::Int = 1 echo
```
### Local arg names
If the key is not well suited for the name of a local variable, 
there is a syntax for declaring a separate name for them.

```Scala
Int for: x::Int except: y::Int -> Int = [
    ^ x + y
]
```
^ is return, this is Smalltalk syntax. 


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

## Single expression form
If the method body consists of a single expression, 
then you can do without a block of code:
```Scala
Int add::Int = this + add
```
With short form you can omit the return type(`-> Int`) and return statement(`^`)

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
I would not recommend to create a custom binary message(i.e. operator overloading),
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
