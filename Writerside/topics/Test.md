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