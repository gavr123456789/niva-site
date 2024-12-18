# Code examples
That's just a copy of some examples from https://github.com/gavr123456789/Niva/tree/main/Niva/Niva/examples  

## Types
```Scala
// create type
type Person name: String age: Int
// create method
Person haveBirthday = age <- age + 1

// create instance
person = Person name: "sas" age: 6
// send message
person haveBirthday

```

## Enums
```Scala
enum Color r: Int g: Int b: Int =
| RED   r: 255 g: 0 b: 0
| GREEN r: 0 g: 255 b: 0
| PURPLE r: 128 g: 0 b: 128

Color sumOfColors = r + g + b

Color.PURPLE sumOfColors echo

rg = Color.RED r + Color.GREEN g
rg echo

// Simple

enum Letter = A | B | C

x = Letter.A
```

## Unions

```Scala
// single line
union Color = Red r: Int | Green g: Int | Purple r: Int g: Int

// multi line
// x and y belong to each branch
union Shape x: Int y: Int =
    | Rectangle width: Int height: Int
    | Circle    radius: Int


constructor Float pi = 3.14

Shape getArea -> Float =
    | this
    | Rectangle => width * height |> toFloat
    | Circle => Float pi * radius * radius


x = Rectangle width: 2 height: 3 x: 0 y: 0
x getArea echo

// another example
union User =
    | LoggedIn name: String
    | Guest

User getName -> String = | this
    | Guest => "Welcome guest"
    | LoggedIn => name


User welcome =
    ("Welcome " + this getName) echo

user = LoggedIn name: "Oleг"
guest = Guest new
user welcome
guest welcome
```

