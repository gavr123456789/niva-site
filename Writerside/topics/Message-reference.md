# Message reference

Sometimes you may need to reference a message: 

```Scala
list = {1 2 3 4}

list map: [it inc]
list map: &Int inc // same thing
```

Here `&Int inc` is a reference to `Int inc` message with signature `[Int -> Int]`

Another example
```Scala
type Rectangle x: Int y: Int
Rectangle area = x * y

list = {(Rectangle x: 2 y: 2) (Rectangle x: 3 y: 5)}

list map: [it area]
list map: &Rectangle area

```

## Manual interfaces
Niva doesn't support interfaces directly.  
It tries to be as static as possible.  
Try to code without inheritance\method overrides\abstract classes\interfaces and you will get why is that.  
But in rare cases when you really need the [late binding](https://en.wikipedia.org/wiki/Late_binding) you can fake it 
with a type that takes only codeblocks and fill them with method references.  

```Scala

type Person 
  name: String
  
// real methods implementations
extend Person [
  on sleep = "sleeeping" echo
  on sayHello = "Hi! I'm $name" echo
]

// fake interface(type with method signatures)
type Human 
  yapping: [] // means take no args and have no return
  sleeping: []

// something that takes our fake interface as argument
type Program
Program h::Human = [
  h sleeping do
  h yapping do
]

p = Person name: "Alice"
pr = Program new

// here we send our "interface" with implementations from other type
pr h: (Human yapping: &p sayHello 
             sleeping: &p sleep)
```

Here you can see that we can capture the reference to method
with its receiver(`p`) of type person. So when we send `p sayHello` to program, it will print  
`Hi! I'm Alice` because the Peron was initialized with name "Alice".  
