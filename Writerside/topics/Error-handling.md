# Error effects handling

Errors in go
https://x.com/zack_overflow/status/1850620600882258374  

TODO rewrite

## Raising errors

Throwing errors is a simple message for error object
```Scala
err = Error1 text: "Important error information ^_^"
err throw
```

## Defining custom errors
The same as unions:
```Scala
errordomain MyError =
| Error1 text: String
| Error2 code: Int
```

## Catching
```Scala
x = "text.txt" reatContent ifError: [
    "got some error!!!" echo
    | it
    | Error1 => it text echo
    "default value"
]
```

## Defining possible errors in return type  
Errors are effects that poison call stack.  
Its kinda errors as values + exception in one thing.  
1) They are explicit in methods signature: `-> Int!Error` or `-> Int![Error1 Error2]`
2) You don't always need to catch them, they will go further up stuck until caught
3) You can force catching all possible errors if you define empty errors list: `-> Int![]`

If method can throw error you need to mark its return type other-vice it will be a compile time error.  

```Scala
// ERROR not all possible errors mentioned use "-> Int!Error1"
Int sus -> Int = [ 
  Error1 text: "qwf" |> throw
]
```

If you lazy you can use just `!`, like `-> Int!`.  
That's means any type of error, correct type will be inferred anyway, you just wont see it in signature

```Scala
Int sas2 -> Int! = [
    x = Error2 code: 42
    ^ this != 0 => x throw |=> 1
]
```

## Exhaustiveness

If there more than one error you need to check them all, same as union matching:

```Scala
Int dangerous -> Int![Error1 Error2] = [
    Error1 text: "abc" |> throw
    Error2 code: -1 |> throw
    ^ 42
]

result = 42 dangerous ifError: [
    | it
    | Error1 => [ "oups" echo ]  
    // ERROR not all possible errors processed: Error2
]
```

## Default value
You can ignore matching and just define default value:
```Scala
x = "text.txt" reatContent ifError: [ "default value" ]
```


## Full example
```Scala
errordomain MyError =
| Error1 text: String
| Error2 code: Int

Int sas2 -> Int! = [
    x = Error2 code: 42
    x throw
    ^ this != 0 => x throw |=> 1
]

Int sas -> Int!Error1 = [
    x = Error1 text: "Important error information ^_^"
    ^ this != 0 => x throw |=> 1
]

// catch error
Int sus -> Int = [
    y = 1 sas ifError: [
        "got some error!!!" echo
        | it
        | Error1 => it text echo
        42
    ]
    y echo // 42
    ^3
]

// ignore error
Int sus2 -> Int! = [
    ^5 sus
]

1 sus
```