# Mutable Set

### unary
```Scala
count -> Int
```
```Scala
echo -> Unit
```
removes every element
```Scala
clear -> Unit
```
```Scala
first -> T
```
```Scala
last -> T
```
```Scala
toList -> List::T
```
```Scala
toMutableList -> List::T
```
### binary
```Scala
== MutableSet::T -> Boolean
```
```Scala
!= MutableSet::T -> Boolean
```
```Scala
+ T -> MutableSet::T
```
```Scala
- T -> MutableSet::T
```
### keyword
```Scala
forEach: [T -> Unit] -> Unit
```
```Scala
onEach: [T -> Unit] -> MutableSet::T
```
```Scala
map: [T -> G] -> MutableSet::G
```
```Scala
mapIndexed: [Int, T -> G] -> MutableSet::G
```
```Scala
filter: [T -> Boolean] -> MutableSet::T
```
```Scala
intersect: MutableSet::T -> MutableSet::T
```
```Scala
contains: T -> Boolean
```
Checks if all elements in the specified collection are contained in this set
```Scala
containsAll: MutableSet::T -> Boolean
```
```Scala
add: T -> Unit
```
```Scala
remove: T -> Boolean
```
```Scala
addAll: MutableSet::T -> Boolean
```
