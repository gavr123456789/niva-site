# Map

A collection that holds pairs of objects (keys and values) and supports 
efficiently retrieving the value corresponding to each key.  
    
Literal for MutableSet is `#{1 "one" 2 "two"}` or `#{1 "one", 2 "two"}` because its hash based

## Creating map
```Scala
map = #{1 "one" 2 "two"}
```

## Accessing elements
To get the element from the map you need to know its key.  
`at: key` message returns nullable value, in case there no such key, so you need to unpack it
```Scala
map = #{1 "one" 2 "two"}
one = map at: 1
real = one unpackOrError
real = one unpackOrValue: "no value"
real = one unpack: [it echo]
```

You can iterate over keys and values with forEach message:
```Scala
map forEach: [ k, v ->
    "key: $k value: $v"
] 
// or iterate over keys\values only
map values forEach: [it echo]
map keys forEach: [it echo]
```
Map also supports filter: and map: with the same signature as `forEach:`.  

Check if there is a key or value: 
```Scala
map containsKey: 1
map containsValue: "2"
```

## Mutation
```Scala
x = #{1 "one"}
x at: 2 put: "two"
x remove: 1 // remove element by key
x clear // remove all elements

x putAll: #{2 "two" 3 "three"}
```

## Common messages
```Scala
map = #{1 "one"}
map isEmpty // false
```

The same as Sets, Maps support `+` and `-` messages, it creates new map, without mutation.
```Scala
map1 = #{1 "one" 2 "two"}
map2 = #{1 "one"}
map3 = map1 - map2

>map1 //> {1=one, 2=two}
>map2 //> {1=one}
>map3 //> {1=one, 2=two}
```

It can be useful to construct a map based on a list with `inject:into:` message
```Scala
//> {1=false, 2=true, 3=false, 4=true, 5=false}
>{3 4 5} inject: #{1 false 2 true} into: [map, cur ->
    map at: cur put: cur % 2 == 0
    map
]
```


## Converting 
You can take the values or set of keys from a map

```Scala
map = #{1 "one" 2 "two"}
keys = map keys
values = map values
```