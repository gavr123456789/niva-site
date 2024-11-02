# StringBuilder
Frequent concatenation of strings (e.g., with + in a loop) creates multiple String objects due to immutability, 
which can be memory- and performance-intensive. Using StringBuilder or StringBuffer instead allows for efficient 
concatenation as they modify their content without creating new objects.  

```Scala
sb = StringBuilder new
sb appendLine: "a veeeery long string"
sb appendLine: "another veeeery long string"
result = sb toString
```

### unary
```Scala
toString -> String
```
```Scala
first -> String
```
```Scala
last -> String
```
```Scala
count -> Int
```
### keyword
Appends the string representation of the arg
```Scala
append: Any -> StringBuilder
```
Appends the string representation of the arg
```Scala
appendLine: Any -> StringBuilder
```
