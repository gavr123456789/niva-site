# CompileTime Reflection
// TODO

You can get the names of the arguments with 
`Compiler getName: numberOfArgument` message.
It can be really helpful to create cool debug and assert messages.

```Scala
TODO print debug
T debug -> T = [
    receiverName = Compiler getName: 0
    value = this toString
    "$receiverName = $value" echo
    ^ this
]
x = 5
x debug // "x = 5"

TODO assert
```

```Scala
type Tape pos: Int

Tape sas = [
    receiverName = Compiler getName: 0
    "sas receiverName is $receiverName" echo
]

Tape key1::Int key2::String = [
    receiverName = Compiler getName: 0
    firstArg = Compiler getName: 1
    secondArg = Compiler getName: 2
    "key1:key2: receiverName is $receiverName" echo
    "key1:key2: firstArg is $firstArg" echo
    "key1:key2: secondArg is $secondArg" echo
]

tape = Tape pos: 5
tape sas

strangeTape = Tape pos: -1
strangeTape sas

foo = 1
bar = "bar"

tape key1: foo key2: "bar"


```