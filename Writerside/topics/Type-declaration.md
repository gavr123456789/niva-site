# Type declaration
Type names are in SnakeCase
```Scala
type Person name: String age: Int

type Rectangle
    width: Int 
    heigth: Int
    x: Int
    y: Int
```

## Type declaration is the type constructor
The type declaration and the constructor of this type are
one and the same.  
To create an object, you just need to send a message
from the fields of the type to the type itself
```Scala
person = Person name: "Alice" age: 24

rectangle = Rectangle
    width: 1 
    heigth: 2
    x: 3
    y: 4
```

You can change the order of the fields, 
this is very convenient if you want to add a new field, 
but do not want to add it to the end.   
  
`person = Person age: 24 name: "Alice"`

## No fields
Types with zero fields are completely fine
```Scala
type Printer
```

Since all messages have a receiver in the field, 
you are always in the context of some type, 
so empty types can be used simply to organize the code

```Scala
extend Printer [
    on info::String = "Info: $info" echo
    on debug::String = "Debug: $debug" echo
]
```