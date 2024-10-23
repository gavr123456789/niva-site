# List

Literal for mutable list collection is `{1, 2, 3}`  
Commas are optional `{1 2 3}`
## Creating list
```Scala
x = {1 2 3}
```
To create an empty list you need to specify the type, because its impossible to infer it from the empty collection.
```Scala
x::MutableList::Int = {}
```
You can put messages inside collection with braces:
```Scala
collectionWithMessages = {(1 inc) (2 inc) (3 inc inc)}
// {2 3 5}
```

## Accessing elements
Indexing starts from 0, like in most programming languages
```Scala
list = {1 2 3}
second = list at: 1 
second echo // 2
```
There are a more safe way of doing that - `atOrNull:` message will return nullable value.  
You need to unpack it with one of unpack methods. Here are the most common ones:
```Scala
maybeSecond = list atOrNull: 1
// will eval body only if there are a value
maybeSecond unpack: [it echo] 
// if its null, replace it with 0
real1 = maybeSecond unpackOrValue: 0 
// unsafe unpacking, will throw an error
real = maybeSecond unpackOrError 

```

## Mutation
You can add new elements with `add: T` message:
```Scala
list = {1 2 3}
list add: 4
list echo
```
Add all elements of another collection
```Scala
list addAll: {4 5 6}

```

You can add elements into specific places with `at: Int put: T` message:
```Scala
list = {1 2 3}
list at: 2 put: 4 // C-like: list[2] = 4
// {1 2 4}
```

## Common messages
```Scala
list = {1 2 3}
list count // 3
isThere2 = list contains: 2

llist map: [it toString + "s"]
list filter: [it % 2 == 0]
list chunked: 2 // {{1 2} {3}}
list reversed
list shuffled // usefull for solitaire implementation
list find: [it % 2 == 0] // find element, retunrs nullable T?
list firstOrNull // get first item
list firstOrNull: [it % 2 == 0] // find first item
list indexOfFirst: [it % 2 == 0] // find first index by condition
list indexOfLast: [it % 2 == 0] // find first index by condition
list clear // remove all elements

```
To get all of them run `niva info > info.md`

## Lazy
If you wanna run many processing messages like `map:` `filter:` on the collection it's better to transform it to sequence.  
Then, instead of creating intermediate collections, actions will be performed on each element separately.
```Scala
    list = {1 2 3 4 5 6 7}
    str = list asSequence |>
        filter: [it % 2 == 0] |> 
        map: [it * 10] |>
        joinTransform: [it + it |> toString]
    // 40, 80, 120
```
