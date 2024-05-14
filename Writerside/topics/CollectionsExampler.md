# CollectionsExampler

## Lists

```Scala
list = {1 2 3 4}
list add: 5
//> [1, 2, 3, 4, 5]
> list

list at: 0 put: 6
//> [6, 2, 3, 4, 5]
> list

mappedList = list map: [x -> x toString + "s"]
//> [1s, 2s, 3s, 4s, 5s]
> mappedList

mappedList2 = mappedList map: [(it at: 0) digitToInt + 1]
//> [2, 3, 4, 5, 6]
> mappedList2


emptyList::MutableList::Int = {}
emptyList add: 5
//> [5]
> emptyList

// you can put messages inside collection with braces(because , is optional)
collectionWithMessages = {(1 inc) (2 inc) (3 inc inc)}


// fold
//> 6
>{1 2 3} inject: 0 into: [a, b -> a + b]

//> [1, 2, 3]
>{3 3 3} inject: #(1 2 3) into: [set, cur ->
    set add: cur
    set
]
//> {1=false, 2=true, 3=false, 4=true, 5=false}
>{3 4 5} inject: #{1 false 2 true} into: [map, cur ->
    map at: cur put: cur % 2 == 0
    map
]


```

## Maps

```Scala

map = #{1 "one" 2 "two"}

map at: 3 put: "three"
//> {1=one, 2=two, 3=three}
>map

map forEach: [ k, v ->
    k inc echo
    (v at: 0) echo
]

// empty collection
map2::MutableMap(Int, String) = #{}
map2 at: 1 put: "sas"
//> {1=sas}
>map2
```

## Sets

```Scala
set1 = #(1 2 3)
set2 = #(2 3 4)

intersect = set1 intersect: set2
>intersect


set3 = set1 + set2 + 0 - 4
//> [1, 2, 3, 0]
>set3

set4 = set3 - #(3 0)
//> [1, 2]
>set4

set1 add: 10
set1 remove: 1
//> [2, 3, 10]
>set1

// empty collection
set::MutableSet::Int = #()
set add: 5
//> [5]
>set

```