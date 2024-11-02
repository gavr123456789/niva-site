# Int

### unary
```Scala
echo -> Unit
```
increments the number by 1  
1 inc == 2
```Scala
inc -> Int
```
1 dec == 0
```Scala
dec -> Int
```
```Scala
toFloat -> Float
```
```Scala
toDouble -> Double
```
```Scala
toLong -> Long
```
```Scala
toString -> String
```
```Scala
toChar -> Char
```
### binary
```Scala
== Int -> Boolean
```
```Scala
!= Int -> Boolean
```
```Scala
> Int -> Boolean
```
```Scala
< Int -> Boolean
```
```Scala
<= Int -> Boolean
```
```Scala
>= Int -> Boolean
```
```Scala
+ Int -> Int
```
```Scala
- Int -> Int
```
```Scala
* Int -> Int
```
5 % 2 == 1
```Scala
% Int -> Int
```
```Scala
/ Int -> Int
```
Creates a range from this value to the specified (included), use ..< for excluded
```Scala
.. Int -> IntRange
```
Creates a range from this value to the specified (excluded)
```Scala
..< Int -> IntRange
```
### keyword
Returns a Range from this value down to the specified to value with the step -1
```Scala
downTo: Int -> IntRange
```
1 to: 3 do: [it echo] // 1 2 3
```Scala
to: Int do: [Int -> Any] -> Unit
```
1 to: 10 by: 2 do: [it echo] // 1 3 5 7 9
```Scala
to: Int by: Int do: [Int -> Any] -> Unit
```
1 until: 4 do: [it echo] // 1 2 3
```Scala
until: Int do: [Int -> Any] -> Unit
```
3 downTo: 1 do: [it echo] // 3 2 1
```Scala
downTo: Int do: [Int -> Any] -> Unit
```