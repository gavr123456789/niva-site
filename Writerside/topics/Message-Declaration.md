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

## Constructors
Constructors looks like keyword messages for types:

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