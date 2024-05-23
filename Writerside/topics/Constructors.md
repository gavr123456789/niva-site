# Constructors

Constructors are messages that are sent to a type, 
not an instance of a type.

```Scala
constructor Float PI = 3.14
Float PI echo
```
`PI` here is a message for type Float


## Default parameters mimicry
It's impossible to create default field vales in niva,
but you can replace them with constructors:
```Scala
type Rectangle 
    x: Int
    y: Int
    w: Int
    h: Int
    
constructor Rectangle w: Int h: Int = Rectangle 
    x: 0 // default x and y
    y: 0 
    w: w // send remaining args
    h: h
    
// default constructor
rectangle1 = Rectangle x: 0 y: 0 w: 5 h: 10
// our custom constructor
rectangle2 = Rectangle w: 5 h: 10
```



## No global state
Constructors are very similar to static methods, 
but note that there are no static fields, it is impossible to create 
a global state.

