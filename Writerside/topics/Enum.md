# Enums

the main difference between enums and unions is that a 
root qualifier is required for their instantiation
```Scala
enum Color = RED | BLUE | GREEN

color = Color.RED
```

> Dot syntax is used only in 2 places in niva: for enums and packages `Rectangles.Square new` and `Color.RED`
{style="note"}

## With fields
You can add custom fields to the root of enums, and set its value in each branch.

```Scala

enum Color r: Int g: Int b: Int =
| RED   r: 255 g: 0 b: 0
| GREEN r: 0 g: 255 b: 0
| PURPLE r: 128 g: 0 b: 128


rg = Color.RED r + Color.GREEN g
rg echo
```

## Messages
In niva everything is object, and you can declare and send messages to them, so enums are no different
```Scala
Color sumOfColors = r + g + b
Color.PURPLE sumOfColors echo
```
