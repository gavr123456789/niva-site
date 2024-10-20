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

You can add elements into specific places with `at: Int put: T` message:
```Scala
list = {1 2 3}
list at: 2 put: 4 
// {1 2 4}
```

## Common messages
```Scala
list add: 4
list at: 0 put: 42 // C-like: list[0] = 42
newList = list map: [x -> x toString + "s"]
emptyList::MutableList::Int = {}
// you can put messages inside collection with braces
collectionWithMessages = {(1 inc) (2 inc) (3 inc inc)}
```
