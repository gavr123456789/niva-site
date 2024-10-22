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

TODO