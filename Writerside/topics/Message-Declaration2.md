
# Message declaration

`Type name = body` -  type
`obl name` - call message


```Scala
Int add2 = this + 2 // declare message for
  1 add2            // call
```
Assign the result to a variable and output its value
`niva run main.niva`

As you can see, the declaration and the call are consistent.
Declaration of binary and unary work the same way:

```Scala
// Binary
ReceiverType op arg = [...]
Path / x::String = 1 echo
// Keyword
ReceiverType key1 key2 = [...]
Int from::Int to::Int = 1 echo
```

## Full examples
```Scala
Int add: x::Int -> Int = [
  ^ this + x
]
1 add: 2 // call
```

^ is return, this is Smalltalk syntax. 