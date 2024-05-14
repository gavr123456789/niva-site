# ControlFlow

```Scala
// messages
x = 1
x < 5 ifTrue: ["yes!" echo] ifFalse: ["nnno!" echo]
x > 5 ifFalse: ["yes!" echo] ifTrue: ["nnno!" echo]
// single ifTrue: also works
true ifTrue: ["true!" echo]

// single expression (can't chain if elif else)
42 > 0 => "Single Expression if!" echo
// can contain else (|=>)
42 < 0 => "Nothing" echo |=> "Something" echo
// can be used as expression
l = 4 < 0 => "yes" |=> "no"
l echo // no


// multiline switch
| x
| 1 => "switch 1" echo
| 2 => "switch 2" echo
|=> "what?" echo
// single line switch
m = |x| 1 => x inc | 2 => x dec |=> 0
// m = 2 because of x inc


// multiline if elif else chain
_
| x > 5 => "x > 10" echo // if
| x < 0 => "x < 10" echo // else if
|=> "x > 0 and < 10" echo // else

// another way to do that
y = 5

y > 5 => 1 echo |=>
y < 7 => 2 echo |=>
y + 42 |> echo



// single line if
_| 5 < 6 => 7 echo
u = _| 1 > 2 => 3 |=> 4
u echo


// codeblock can be used as well
1 > 2 => [
    1 echo
    1 echo
]|=>[
    2 echo
    2 echo
]

// single if expression inside switch
| 5
| 4 => 1 echo
| 5 => 1 > 2 => "wat?" echo |=> "yay!" echo



// control flow expressions as arguments
Boolean to::Boolean = [
    to echo
]
o = 5
(o > 5 => true |=> false) to: (o == 5 => true |=> false)

```