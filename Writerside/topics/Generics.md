# Generics

## Type generic
A type marked with a single capital letter is considered a generic, for example T or G:
```Scala
type Box v: T

intBox = Box v: 1
boolBox = Box v: false

intBox v + 1 |> echo
boolBox v not |> echo
```

Here we created a box with one generic field, so u can put different types here.  

### Explicit declaration
In some cases niva can infer the generic type, because there is no arguments.
```Scala
union Option = 
| Some v: T
| None
```
Here its easy for `Some`, but not for `None`, so we need to say type explicitly: 
```Scala
x = Some v: 42
y = None::Int new
x echo
y echo
```
```
Some v: 42
None
```

## Messages
Same as for types, the list of generic params for msg will be inferred from arguments and return type:
```Scala
Person x::T y::T -> MutableList::T = [
  ^ {x y}
]

p = Person new
list = p x: 1 y: 2  
list echo // {1, 2}
```
