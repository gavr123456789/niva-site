# Test

You can create test very easily.  
Just create a unary message for the `Test` and it's done!

```Scala
Test math = [
    1 / 0
]
```

If you run `niva test` you will see this: 
```
main.mainTest[jvm] > math[jvm] ❌
    java.lang.ArithmeticException: / by zero
```

Any test that does not throw an error is considered passed, 
which means it's time for us to learn how to throw errors

## Test with asserts

Here is Assert function that will print both the result and failed expression

```Scala
// assert.niva
type Assert 

constructor Assert that::Any equals::Any -> Unit! = [
  a = Compiler getName: 1
  b = Compiler getName: 2

  that != equals => [
    Error throwWithMessage: "Assertion failed: $a' != $b' ($that != $equals)"
  ]
]
```

```Scala
// main.niva
type Data
    numbers::List::Int
/// returns list of numbers incremented by one
Data inc = 
    numbers map: [it inc]

Test simple -> Unit! = [
    data = Data numbers: {1 2 3 4}
    expected = {2 3 4 5}
    actual = data inc
    
    Assert that: actual equals: expected
]
```

`niva test`  

![niva-testing1.png](niva-testing1.png)  

Lets break the test

```Scala
Test fail -> Unit! = [
    data = Data numbers: {1 2 3 4}
    Assert that: data numbers equals: {2 3 4 5}
]
```

![niva-testing2.png](niva-testing2.png)
