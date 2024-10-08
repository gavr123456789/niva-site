# Message Send


### Unary


Let's look at nested function call from C like lang:
```C
foo(bar(foobar(42)))
```

It reads from the inside out or from right to left.  
Let's remove parenthesis
```C
foo bar foobar 42 
```
And now invert, to make it readable:
```Scala
42 foobar bar foo
```



Now we got niva syntax!
```Scala
1 inc inc == 3
5 factorial == 20
"Hello" count == 5
"Hello" reverse == "olleH"
```

Some ML languages use pipe operators to achieve the same readability  
`42 |> foobar |> bar |> foo`


So every call has a receiver that receive "message"  
`1 inc factorial`  
`receiver <- message1 <- message2`

There are three kinds of messages.  
You are already familiar with unary, it has one arg(it's receiver).  
But what if we need more args?

### Keyword
> This is the most common way to send messages
{style="note"}

What about sending messages with arguments?   
`1 add 2`  
okay, but what if we have more args  
`1 addMany 2 3 4 add 2`  
this is pretty unreadable, so we need to distinguish each arg  
`1 add: 2`  
`widget widgh: 250 height: 250`  
If you need to compose keyword msgs inside each other use (), also you can put args on new lines:   
```Scala
widget 
    widgh: 250 
    height: 250
    color: (Color r: 0 b: 255 g: 255)
```
Do you find this readable?


```C
"12.09.2024" replace: "." with: "-"
1 to: 5 do: [ it echo ] // 12345
```
In C like languages there is a concept of calling functions with named arguments  
`...`  
So in niva name of the function and name of the arguments are the same

![pharoKWSyntax.png](pharoKWSyntax.png)

### Binary
Binary messages is a pretty common syntax:
```Scala
1 + 2
"a" + "b" + "c"
```
But its not operators, its still a messages for receiver:  
`receiver + arg`

unary always evaluate first,  
`1 inc inc + 4 dec` ->    
`(1 inc inc) + (4 dec)` ->    
`3 + 3` ->  
`6`

Here are some examples that became looks like DSL because of that rule:
```Scala
TODOOOOOOOOOOOOOOOOOOOO
Date
...
Colors for text
```

## Send keyword to keyword

`1 from: 2 to: 3`
Is



