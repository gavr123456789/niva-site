
# Message declaration

`Type name = body` -  declaration  
`instance name` - call   

```Scala
Type name = body // declaration
instance name    // call
Int add2 = this + 2 // declare message for Int
  1 add2            // call
```

As you can see, the declaration and the call are consistent.
Declaration of binary and unary work the same way:

```Scala
// Binary
// ReceiverType op arg = [...]
Path / x::String = 1 echo
// Keyword
// ReceiverType key1 key2 = [...]
Int from::Int to::Int = 1 echo
```

## Local arg names
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

## Short form
If the method body consists of a single expression, 
then you can do without a block of code:
```Scala
Int add::Int = this + add
```
With short form you can omit the return type(`-> Int`) and return statement(`^`)
