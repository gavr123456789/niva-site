# IntRange

### unary
```Scala
echo -> Unit
```
```Scala
isEmpty -> Boolean
```
```Scala
first -> Int
```
```Scala
last -> Int
```
Returns a random element from this range
```Scala
random -> Int
```
```Scala
toList -> List::Int
```
```Scala
asSequence -> Sequence::Int
```
```Scala
toMutableList -> MutableList::Int
```
### binary
```Scala
== IntRange -> Boolean
```
```Scala
!= IntRange -> Boolean
```
### keyword
The step of the progression
```Scala
step: Int -> IntRange
```
```Scala
forEach: [Int -> Unit] -> Unit
```
```Scala
forEachIndexed: [Int, Int -> Unit] -> Unit
```
```Scala
filter: [Int -> Boolean] -> IntRange
```
```Scala
contains: Int -> Boolean
```
