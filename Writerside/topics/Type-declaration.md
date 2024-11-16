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
type Person name: String age: Int
person = Person name: "Alice" age: 24


type Rectangle
    width: Int 
    heigth: Int
    x: Int
    y: Int

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
So empty types can be used simply to organize the code

```Scala
extend Logger [
    on info::String = "Info: $info" echo
    on debug::String = "Debug: $debug" echo
]
l = Logger new
l info
```

## Doc comments
`///` is a doc comment, you can reference other types from it with @

![doc-comment1.png](doc-comment1.png)  

![doc-comment2.png](doc-comment2.png)  

Same works for methods declarations