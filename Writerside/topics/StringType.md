# String
For now niva using java.String, so everything about Java strings also applies to niva strings.  
For example, immutability and deduplication(small strings are stored once if they are the same, so 10k of "Cat"s strings will take up space as one)  
  
Interpolation of variables is supported(but not for expressions):
```Scala
x = "Alice"
"Hi $x!"
```
  
Every niva object has a string representation:
```Scala
person = Person name: "Alice" age: 24
person toString // Person name: Alice age: 24
```
Message echo and string interpolation apply the toString method automatically.   


### unary
Length of this String
```Scala
count -> Int
```
Returns a string with characters in reversed order
```Scala
reversed -> String
```
Returns a string having leading and trailing whitespace removed
```Scala
trim -> String
```
Detects a common minimal indent of all the input lines, removes it from every line
```Scala
trimIndent -> String
```
true if this char sequence is empty or consists solely of whitespace characters
```Scala
isBlank -> Boolean
```
Only the "" is true
```Scala
isEmpty -> Boolean
```
True if its only digits and characters like "abc123"
```Scala
isAlphaNumeric -> Boolean
```
Opposite to isBlank
```Scala
isNotBlank -> Boolean
```
Opposite to isEmpty
```Scala
isNotEmpty -> Boolean
```
```Scala
toInt -> Int
```
```Scala
toFloat -> Float
```
```Scala
toDouble -> Double
```
Returns the first character or panic
```Scala
first -> Char
```
Returns the last character or panic
```Scala
last -> Char
```
```Scala
indices -> IntRange
```
```Scala
echo -> Unit
```
### binary
Concatenate 2 Strings
```Scala
+ String -> String
```
```Scala
== String -> Boolean
```
```Scala
!= String -> Boolean
```
### keyword
"Tom the cat" replace: "cat" with: "mouse"
```Scala
replace: String with: String -> String
```
"abc" forEach: [char -> char echo] // a b c
```Scala
forEach: [Char -> Unit] -> Unit
```
"abc" forEachIndexed: [index, char -> char echo] // a b c
```Scala
forEachIndexed: [Int, Char -> Unit] -> Unit
```
"abc" filter: [it != 'c'] // "ab"
```Scala
filter: [Char -> Boolean] -> String
```
"abc" startsWith: "ab" // true
```Scala
startsWith: String -> Boolean
```
```Scala
contains: String -> Boolean
```
see startWith:
```Scala
endsWith: String -> Boolean
```
Returns a substring of this string that starts at the specified startIndex and continues to the end of the string
```Scala
substring: Int -> String
```
Returns a string containing characters of the original string at the specified range
```Scala
 "abcd" slice: 0..<3 // abc
```
```Scala
slice: IntRange -> String
```

Returns a substring after the first occurrence of delimiter
```Scala
substringAfter: String -> String
```
```Scala
substringAfterLast: String -> String
```
see substringAfter:
```Scala
substringBefore: String -> String
```
```Scala
substringBeforeLast: String -> String
```
see substring:
```Scala
substringFrom: Int to: Int -> String
```
Returns the character of this string at the specified index or panic
```Scala
at: Int -> Char
```
Returns a string with the first n characters removed
"abc" drop: 5 // empty ""
```Scala
drop: Int -> String
```
Returns a string with the last n characters removed
```Scala
dropLast: Int -> String
```
```Scala
map: [Char -> T] -> List::T
```
```Scala
split: String -> List::String
```
