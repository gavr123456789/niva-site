# Mutable List

### unary
```Scala
count -> Int
```
```Scala
echo -> Unit
```
```Scala
first -> T
```
last or panic, use lastOrNull for safety
```Scala
last -> T
```
```Scala
firstOrNull -> T?
```
```Scala
lastOrNull -> T?
```
Immutable list, elemets will be shadow copied
```Scala
toList -> List::T
```
Mutable list, elemets will be shadow copied
```Scala
toMutableList -> MutableList::T
```
Like in Solitaire
```Scala
shuffled -> MutableList::T
```
All processing methods like filter map, will execute lazy
```Scala
asSequence -> Sequence::T
```
```Scala
isEmpty -> Boolean
```
```Scala
isNotEmpty -> Boolean
```
```Scala
reversed -> MutableList::T
```
{1 2 3} sum == 6
```Scala
sum -> Int
```
```Scala
clear -> Unit
```
### keyword
```Scala
forEach: [T -> Unit] -> Unit
```
```Scala
onEach: [T -> Unit] -> MutableList::T
```
{1 2 3} forEachIndexed: [i, it ->
"$i element is $it" echo
]
```Scala
forEachIndexed: [Int, T -> Unit] -> Unit
```
```Scala
map: [T -> G] -> MutableList::G
```
```Scala
mapIndexed: [Int, T -> G] -> MutableList::G
```
```Scala
filter: [T -> Boolean] -> MutableList::T
```
like list[x] in C, can panic
```Scala
at: Int -> T
```
safe version of at
```Scala
atOrNull: Int -> T?
```
{1 2 3} contains: 1 is true
```Scala
contains: T -> Unit
```
Returns a list containing all elements except first n elements
```Scala
drop: Int -> MutableList::T
```
Returns a list containing all elements except last n elements.
```Scala
dropLast: Int -> MutableList::T
```
Splits this collection into a list of lists each not exceeding the given size
```Scala
chunked: Int -> List::List
```
{1 2 3} joinWith: ", " is 1, 2, 3
```Scala
joinWith: String -> String
```
Usable for joining fields of objects without unpacking them, elements joining with ", "
 ```Scala
 type Person name: String
 {(Person name: "Bob") (Person name: "Alice")} 
     joinTransform: [ it name ] // Bob, Alice
```
```Scala
joinTransform: [T -> G] -> String
```
```Scala
indexOfFirst: [T -> Boolean] -> Int
```
```Scala
indexOfLast: [T -> Boolean] -> Int
```
For sorting collection of objects by one of their field
```Scala
{(Person name: "Bob") (Person name: "Alice")} 
    sortedBy: [ it name ]
// {Person name: "Alice", Person name: "Bob"}
```
```Scala
sortedBy: [T -> G] -> MutableList::T
```
Same as joinToString, but you can choose how to join(not only ", ")
```Scala
joinWith: String transform: [T -> G] -> String
```
Accumulates value starting with injected value and applying operation from left to right to current accumulator value and each element(kinda fold)
{1 2 3} inject: 0 into: [acc, next -> acc + next] // is 6
```Scala
inject: G into: [G, T -> G] -> G
```
Accumulates value starting with the first element and applying operation from left to right to current accumulator value and each elementsame as inject:into: but without accumulator arg
```Scala
reduce: [T, T -> G] -> T
```
Splits the original collection into pair of lists, where first list contains elements for which predicate yielded true, while second list contains elements for which predicate yielded false.
{1 2 3 4} partition: [it % 2 == 0] // {[2, 4], [1, 3]}
```Scala
partition: [T -> Boolean] -> Pair(Sequence::T, Sequence::T)
```
```Scala
sumOf: [T -> T] -> T
```
Returns the first element matching the given predicate, or null if no such element was found.
```Scala
find: [T -> Boolean] -> T?
```
Returns a view of the portion of this list between the specified fromIndex (inclusive) and toIndex (exclusive). The returned list is backed by this list, so non-structural changes in the returned list are reflected in this list, and vice-versa.
```Scala
viewFrom: Int to: Int -> MutableList::T
```
```Scala
add: T -> Boolean
```
```Scala
addFirst: T -> Unit
```
Add all items from other collection
```Scala
addAll: MutableList::T -> Boolean
```
Remove element by index
```Scala
removeAt: Int -> Unit
```
Remove element
```Scala
remove: T -> Unit
```
like C arr[x] = y
```Scala
at: Int put: T -> Unit
```
