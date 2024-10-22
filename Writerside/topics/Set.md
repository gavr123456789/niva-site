# Set

A generic unordered collection of elements that does not support duplicate elements, and supports adding and removing elements.  
Literal for MutableSet is `#(1 2 3)` 

## Creating set
```Scala
set = #(1 2 3 3 3) // 1 2 3
```
It's possible to create new set from the two others by by addition, subtraction or intersection:
```Scala
sum = #(1 2 3) + #(3 4) // $(1 2 3 4)
sub = #(1 2 3) - #(4 5) // #() 
intersect = #(1 2 3) intersect: #(2 3 4) // #(2 3)

```

## Accessing elements
You cant access elements by index, since order of 
elements is not the same.  
But you can iterate over set the same way as over list:

```Scala
#{1 2 3 3 4} forEach: [it echo] 
```
## Mutation
```Scala
set = #(1 2 3)
set add

set clear // #()
```

## Common messages


```Scala
set = #(1 2 3)
set count // 3
```
## Converting 
```Scala
#(1 1 1 2) toMutableList // {1 2}
#(1 1 1 2) toList // {1 2}
```