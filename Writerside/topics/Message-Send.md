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
1 inc inc // 3
5 factorial // 20
"Hello" count // 5
"Hello" reversed // "olleH"
```

Some ML languages use pipe operators to achieve the same readability  
`42 |> foobar |> bar |> foo`


So every call has a receiver that receive "message"  
`1 inc factorial`  
can be read as:  
`receiver <- message1 <- message2`

There are three kinds of messages.  
You are already familiar with unary, it has one arg(it's receiver).  
But what if we need more args?

### Keyword


What about sending messages with arguments?   
`1 add 2`  
Okay, but what if we have more args  
`1 addFirst 2 addSecond 3`  
This is pretty unreadable, so we need to distinguish each arg  
```Scala
1 add: 2
widget 
  widgh: 400
  height: 250
```

So function calls in niva are always named:  
`receiver keyword1: arg1 keyword2: arg2`  
equivalent to C/Java positional based syntax:  
`receiver.keyword1keyword2(arg1, arg2)`  

Here you can see why I think named arguments are better than positional ones:

![compareWithUnison.png](compareWithUnison.png)  

1) `replaceAll " " "-" test`, it's nearly impossible to guess what's going on here
2) All the functions in niva have receivers, because they are actually messages(Smalltalk term). 
So it's just much easier to get a list of all possible messages with auto-completion on any value and understand what they mean too:  
```Scala
    charAt 0 it 
    // vs
    it at: 0
    
    replaceAll " " "-" text
    // vs
    text replace: " " with: "-"
```
> Ofc its cheating since I niva version can throw on `it first` message, but my point is not about the length.
{style="note"}



If you need to compose keyword msgs inside each other use (), also you can put args on new lines:   
```Scala
widget 
    widgh: 250 
    height: 250
    color: (Color r: 0 b: 255 g: 255)
```

```C
"12.09.2024" replace: "." with: "-"
1 to: 5 do: [ it echo ] // 12345
```


In C like languages there is a concept of calling functions with named arguments  
`...`  
So in niva name of the function and name of the arguments are the same



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
// Pharo
'http://www.pharo.org' asUrl retrieveContents
('2014-07-01' asDate - '2013/2/1' asDate) days
// niva
42 factorial echo

// Colors for text, just unary messages
"Hello"red + " World"purp + "!"cyan 

"1969-07-20" split: "-" |> reversed |> joinWith: "/"
// {"1969" "07" "20"} - { "20" "07" "1969" } - "20/07/1969"

#(1 2 3) intersect: #(3 4 5) // #(3)
```

----

So, the general rule is simple, every expression in niva is a message, and all the messages works the same way:  
`receiver <- message1 <- message2`  

```Scala
1 inc // 1 is a receiver and inc is a message it receives
// 1 <- inc
1 from: 2 to: 3 // 1 receives message "from:to:"
// 1 <- from:to:
1 + 2 + 3 // 1 receives 2 messages +2 and +3
// 1 <- + 2 <- + 3
```

## Send keyword to result of another keyword  
There is easy to do it with unary and binary: `1 + 2 + 3`, `1 inc inc`  
But with keyword what if you want to send `to:` kw msg to the result of`from:`  
`1 from: 2 to: 3`  

This is just a single msg `from: to:` to solve this you need to put the first part into parenthesis:  
`(1 from: 2) to: 3`  

With more calls it starts to get messy:  
`((1 from: 2) to: 3) and: 4`  
kinda like in C, but the amount of parenthesis grow on the left side:  
`and(4, to(3, from(1, 2)))`   

So I added pipe operator, which wraps everything to the left in brackets  
`1 from: 2 |>  to: 3 |> and: 4`  

More on that here: [pipes](Pipes-and-Cascades.md)