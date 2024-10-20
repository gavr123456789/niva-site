# Cycles
There is no special syntax for cycles in niva, everything is a message send.
## For loops

```Scala
1..3 forEach: [ it echo ] // 1 2 
1..<3 forEach: [ it echo ] // 1 2 3
```
Here `..` is a binary message for Int that creates `IntRange`.  
And `IntRange` can receive `forEach:` message.

with indexing:
```Scala
3..5 forEachIndexed: [i, it ->
    "index: $i" echo
    "value $it" echo
]
```
The type signature of `forEachIndexed` here is  
`IntRange forEachIndexed: [Int, Int -> Any]`  
Which means that it takes a codeblock that receive 2 parameters, one of them is index and second is the number itself.

With custom steps:
```Scala
1 to: 10 by: 2 do: [it echo]
// 1 3 5 7 9
```

## While loop
While loop is just a message for the codeblock:  
`[ -> Boolean] whileTrue: [ -> Unit]`
```Scala
// there is also whileFalse
mut x = 10
[x > 0] whileTrue: [
  x echo
  x <- x dec
]
```

## Iterate over collections

```Scala
{1 2 3} forEach: [it echo]
```

```Scala
{1 2 3} forEachIndexed: [i, it ->
    "index: $i" echo
    "value $it" echo
]
```

Over map:
```Scala
#{"a" 1 "b" 2 "c" 3} forEach: [ k, v ->
  k echo
  v echo
]
```
See [collections](Collection.md) chapter for more

## Ranges
Here are some useful messages for ranges
```Scala
(1..5) first // 1
(1..5) last // 5
(1..5 step: 2) contains: 3 // true
(1..<5) random // 2
```

----

## Iterate with break

If you really need to break while iterating collection you can add this method to your codebase:
```Scala
MutableList::T forEachBreak::[T -> Boolean] -> Unit = [
    this forEach: [
        // check that every iteration returns true
        // othervice return from the function
        forEachBreak T: it |> not => ^
    ]
]
```

And use it like:

```Scala
{1 2 3 4 5} forEachBreak: [
    it echo
    it % 3 != 0 // last statement must be Boolean
] 
```
So it will print 1 2 3 and break 3 is divisible by 3 without a remainder.  
Basically it can be replaced with `whileTrue:` msg