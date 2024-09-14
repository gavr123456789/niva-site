# Functional syntax problems

## Ambiguous functions argument positions
This is a problem where the user cannot properly tell where to place function arguments properly without looking at documentation.
This problem is actually prevalent for every programming languages that favors prefix function invocations , for example C, Bash, Assembly, and most FP languages.

For example, without referring to documentation or previous experiences, a user can never tell which of the following will yield intended result, even though the user have the domain knowledge (in this case, basic English).

```Scala
// Is this correct?  
splitBy ","  "1,2,3,4,5"  

// or this?  
splitBy "1,2,3,4,5"  ","
```
  
This problem is slightly mitigated in object-oriented programming(OOP) languages due to their infix function invocation nature. 
For example, in Java or Scala, the code above can be rewritten in a more comprehensible manner:

```Scala
// This is obviously not too right
",".splitBy("1,2,3,4,5")

// This should be right, because it reads out more naturally  
"1,2,3,4,5".splitBy(",")
```
However, OOP languages also suffers the same problem whenever the number of function/method arguments get more than two, for example:

```Scala
// Which one is right?
"Hello world".replace("Hello", "Bye")
"Hello world".replace("Bye", "Hello")
```
Fortunately, this problem is actually solved by a languages called Smalltalk, which is designed by Alan Kay and intended to be used by children of all ages. Smalltalk solves this by using its message syntax, for example:
```Scala
"Hello world" replace: 'Hello' with: "Bye"
```
And the good thing of such syntax is that it scales, no matter how many arguments your function is taking, for example:
```Scala
"Hello world" replaceFrom: 0 to: 4 with: "Bye"
```


Not Intellisense friendly
The other problem of FP languages is that they are not Intellisense friendly, in another word, they don't integrate well with integrated development environment (IDE).

This is also due to their prefix function invocation nature, which does not allow Intellisense to provide good code completions suggestion. The best they can do is to provide some low-level Intellisense such as:

Naively suggesting variable or function names

Suggesting function when typing out a module name

This kind of Intellisense are dwarfed when compared to those of OOP languages, which provide higher-level Intellisense, which is:

Suggesting related method based on type of the object
