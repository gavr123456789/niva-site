# Unions

Tagged unions are the main way to achieve polymorphism in niva

Unions are types that can be represented by one of several options at a time.

Declaration of union is like declaration of `type` + switch
```Scala
union Shape =
    | Rectangle width: Int height: Int
    | Circle    radius: Double
```

## Pattern match
To work with a union, you first need to understand what form it is in.   
Switch syntax is used for this.


```Scala
constructor Double pi = 3.14
Double square = this * this

Shape getArea -> Double = 
    | this // "this" is Shape here
    | Rectangle => width * height |> toDouble
    | Circle => Double pi * radius square
```

## Exhaustive
If you don't check all form options, this will be a compile time error.

```Scala

Shape getArea -> Double = 
    | this 
    | Rectangle => width * height |> toDouble
//  | Circle => Double pi * radius square
//  Not all possible variants have been checked (Circle)
```

## Instantiate
It is impossible to instantiate the root of the union, 
since then it is not clear what form it will correspond to.  

But you can create branches, each union branch is no different 
from a regular type declaration.

```Scala
x = Rectangle width: 2 height: 3
x getArea echo
```
> Note that you don't need the root qualifier like `Shape.Rectangle`  
This is debatable, but I like the branches to be no different from the declaration of ordinary top-level types
{style="note"}


## Single line syntax

```Scala
union Color = Red | Blue
union Shape = Circle r: Int | Rectangle w: Int h: Int
```

## Fields for Roots
Unlike other ML languages, union roots can contain fields
```Scala
union Person name: String = 
| Student
| Teacher
```

Both `Student` and `Teacher` have a `name` field here.  
This simplifies 2 things at once:
1) There is no need to repeat common fields for each union's branch. 
2) You don't need to pattern match the Person to gain access to the `name` field

```Scala
Printer printPerson: p::Person = 
    p name echo
```


## Include one union into another

```C
union Vehicle = 
    | ^Car
    | Plane
    | Ship

union Car = RaceCar | Truck | PassengerCar
```
Now Vehicle is Car | Plane | Ship or RaceCar | Truck | PassengerCar | Plane | Ship

So now u can pattern match it like 
```Scala
| sas
| Sos => ...
| Sus => ...
| Ses => ...
```

Or on more general `Sas`
```Scala
| sas
| Sos => ...
| Sas => ...
```

You will get a compilation error if you inadvertently 
check out more general branches before more specific ones
```Scala
| sas
| Sos => ...
| Sas => ... // Sas is more general than Sus
| Sus => ... // So Sus is unreachable here
```

## Homework
Create a well known example with Cat Dog, all of them are Animal. 
Animal can eat food. Food are Meat or Vegetable

