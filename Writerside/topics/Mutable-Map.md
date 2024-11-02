# Mutable Map

### unary
```Scala
count -> Int
```
```Scala
isEmpty -> Boolean
```
```Scala
isNotEmpty -> Boolean
```
```Scala
echo -> Unit
```
```Scala
clear -> Unit
```
```Scala
keys -> MutableSet::T
```
```Scala
values -> MutableSet::G
```
### binary
```Scala
+ MutableMap(T, G) -> MutableMap(T, G)
```
```Scala
- MutableMap(T, G) -> G
```
### keyword
```Scala
forEach: [T, G -> Unit] -> Unit
```
```Scala
map: [T, G -> Unit] -> Unit
```
```Scala
filter: [T, G -> Unit] -> Unit
```
```Scala
at: T put: G -> Unit
```
```Scala
at: T -> G?
```
```Scala
remove: T -> T?
```
```Scala
putAll: MutableMap(T, G) -> Unit
```
```Scala
containsKey: T -> Boolean
```
```Scala
containsValue: G -> Boolean
```
