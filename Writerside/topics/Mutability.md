# Mutability
There is 2 types of mutability:
1) Variable mutability(let const in JS)
2) Type mutability (fields of type are immutable)

## Variable mutability
```Scala
x = 5 // immutable
mut y = 5 
y <- 10
y <- y inc
```

## Type mutability

By default all types are immutable and mutating a field creates shadow copy  

```Scala
type Person 
    name: String
    age: Int
    
p = Person name: "Alice" age: 32
a = p name: "Bob"
b = p age: 31

// p == Person name: "Alice" age: 32
// a == Person name: "Bob" age: 32
// b == Person name: "Alice" age: 31
```



To create mutable typed variable you need to mark its type as `mut`

```Scala

p::mut Person = Person name: "Alice" age: 32
a = p name: "Bob" // ERROR
p name: "Bob" // p mutated

// p == Person name: "Bob" age: 32
```

## Errors
So that you don't confuse the mutation of the field 
with the creation of a shadow copy:  
If you forget to use result of shadow copy it will be an error
```Scala
p = Person name: "Alice" age: 32
p name: "Bob" // ERROR
```

Conversely, if you use the result of a mutation, this is also a mistake.
```Scala
p::mut Person = Person name: "Alice" age: 32
a = p name: "Bob" // ERROR
```

