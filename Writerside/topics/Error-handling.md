# Error effects handling


Errors in niva is something between errors as values(go\rust) and exceptions(JS\Vala\C#\Kotlin).
Both approaches have their advantages.

With errors as values, you can always see the possible errors of a function right from its signature, 
but you always need to unpack the results one way or another (if err not nil in go and unpacking Result type in ML)

![rust-error-unpack.png](rust-error-unpack.png)  

![roc-error-unpack.png](roc-error-unpack.png)

With exceptions, you do not see possible problems, as if walking blindly, 
you cannot guarantee that you have processed all possible errors, 
even if go to implementation of the function and manual verification does not guarantee this, 
because any of the called functions there can also throw an exception.  

But this approach also has advantages,
the case when you can't do anything about the error except to terminate the program is quite common,
and when this is the default behavior, you just get rid of the need for a boiler plate to unpack possible errors.  
Also, when you prototype something, it doesn't matter to you that something can go wrong, you need a scripting 
like experience.  

In niva I combined both of these approaches into one. 
Errors are effects that propagate through the call stack until you process them.  

Errors should be placed in the function signature, here a fake example:  
`File read -> String!{NoSuchFile, CantRead}`  

Return type is still String, its marked with `!` which meanst that method can throw NoSuchFile or CantRead error.  
  
There is also a lazy variant, you can omit the error list:  
`File read -> String!`
This means the same, the compiler will still infer the correct set of errors.

Now lest see go to practice.


// Errors in go https://x.com/zack_overflow/status/1850620600882258374  



## Default Error type
There is a built in `Error` with a single message `throwWithMessage`: 
```Scala
Error throwWithMessage: "Something went wrong!!!"
```
You can use it to throw the general errors.

## Custom error types declaration
You can define your specific errors the same way you define unions:  
```Scala
errordomain MyError =
| Error1 text: String
| Error2 code: Int
```

Then you can create such error and throw it with throw unary msg: 
```Scala
type Person
Person foo -> Int = [
  err = Error1 text: "qwf" // create error object
  err throw
  ^ 42
]
```

After that you will get a compiler error:

![possible-errors-example.png](possible-errors-example.png)  

It says that possible errors detected, where are they, and what you should do.  
So for now lets add this suggested return type and change
`Person foo -> Int = [` 
to 
`Person foo -> Int!Error1 = [`

The compiler error disappear, because now the return type says it can be error there.  
But now, if we call this foo message from another:  

![niva-error-from-another-method.png](niva-error-from-another-method.png)  

That's what I meant by saying "Errors are effects that propagate through the call stack until you process them", 
I hope now you get it.  
The error will stay until you process it somehow, lets move to that part.


## Processing errors

### orPANIC
If your program can't continue you can send `orPANIC` message, and program will stop in case of an error:  
```Scala
type Person
Person foo -> Int!Error1 = [
  err = Error1 text: "qwf"
  err throw
  ^ 42
]

Person bar -> String = [
  this foo orPANIC // HERE

  ^ "hallo everynyan"
]

Person new bar
```
And you will receive the minimalistic looking stack trace: 

```
----------
no message
----------
Method: foo             File: main.niva::7
Method: bar             File: main.niva::13
Method: main            File: main.niva::17
```

### orValue
orValue will replace the error with the value in case of an error:
```Scala
Person bar -> String = [
  x = this foo orValue: -1
  x echo // -1
  ^ "hallo everynyan"
]
```
You can see that return type of bar doesn't contain errors, its just `String`, 
because all the possible errors were processed!  

### ifError
`ifError` message is for cases when many messages are possible and 
you need to do different things depending what error you get.  

Lets change the `foo` implementation so it can throw both Error1 or Error2: 
```Scala
Person foo -> Int!{Error1 Error2} = [
  (Error2 code: 404) throw
  (Error1 text: "something went wrong") throw
  ^ 42
]
```

Have you noticed that the error declaration looks the same as union? 
That's because it is! 
And just like for unions, you can use match to automatically generate and process all cases!  

![if-error-match.png](if-error-match.png)  

![if-error-match-done.png](if-error-match-done.png)

```Scala
Person bar -> String = [
  this foo ifError: [
    | it
    | Error1 => [
      text = it text 
      "got $text error!" echo
    ]
    | Error2 => [
      code = it code
      "got error with code: $code!" echo
    ]
  ]

  ^ "hallo everynyan"
]
```

Now when when you run this you will get `got error with code: 404!` in console.   

This match is exhaustive(unions!) so if you miss any error kind it will tell you.

![error2-is-missing-from-match.png](error2-is-missing-from-match.png)  


### ifError as expression
You can return values from each of ifError branches  

Here is a full code, so you can copy it in your editor and try it yourself ^_^
```Scala
errordomain MyError =
| Error1 text: String
| Error2 code: Int

type Person
Person foo -> Int!{Error1 Error2} = [
  (Error2 code: 404) throw
  (Error1 text: "something went wrong") throw
  ^ 42
]

Person bar -> String = [
  result = this foo ifError: [
    | it
    | Error1 => [
      text = it text 
      "got $text error!" echo
      42
    ]
    | Error2 => [
      code = it code
      "got error with code: $code!" echo
      34
    ]
  ]
  result echo

  ^ "hallo everynyan"
]

Person new bar
```
Now each branch of error matching returns a number, then it assigns to `result` and we print it.
`got error with code: 404!`
`34`

----------OLD guide
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