# Cycles

```Scala
1 to: 3 do: [ it echo ]
3 downTo: 1 do: [ it echo ]

// new way!

(1..10 step: 2) forEach: [
//> 1, 3, 5, 7, 9
    >5 it
]

1..<3 forEach: [
    it echo
]
// with index
3..4 forEachIndexed: [i, it ->
    "index: $i" echo
    "value $it" echo
]

// also ranges
//> 1
> (1..5) first
//> 5
> (1..5) last
//> true
> (1..5 step: 2) contains: 3
//> 2
>(1..<5) random


mut x = 10
"whileTrue" echo
[x > 0] whileTrue: [
  x echo
  x <- x dec
]
"whileFalse" echo
[x == 5] whileFalse: [
  x echo
  x <- x inc
]


```