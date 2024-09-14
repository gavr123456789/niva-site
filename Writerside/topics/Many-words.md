# Many words

I have often come across the fact that in functional languages it is 
necessary to keep a lot of functions in memory.  
Of course, it often helps that they are divided into modules, for example List.map or List.filter.

But this is not so useful with more specific data types.  
Most of the time, I only have a rough idea of what operations 
I want to perform and the only solution is to go and look for something similar in the documentation in the documentation.

## OOP
OOP in some sense solved this problem by placing functions inside types. 
Now I can do `object.` and in the drop-down list I can see all possible 
actions with this object, such functionality first appeared in Smalltalk, of course, as did the very concept of IDE [1](https://medium.com/smalltalk-talk/how-learning-smalltalk-can-improve-your-skills-as-a-programmer-fc5b2f56685)

C++/Java Algol-like OOP in turn became mainstream and brought many new problems, 
which are wonderfully outlined in the video Object-Oriented Programming is Bad https://youtu.be/QM1iUe6IofM

## UFCS

In Nim, this problem was solved differently, they implemented UFCS - universal function call syntax.  
In other words, in a procedural language, the call `add(1, 2)` can be written as `1.add(2)`  
the first argument of each function also acts as a receiver, thereby adding the ability to autocomplete 
without introducing OOP.

```Haskell
type Vector = tuple[x, y: int]

proc add(a, b: Vector): Vector =
  (a.x + b.x, a.y + b.y)

let
  v1 = (x: -1, y: 4)
  v2 = (x: 5, y: -2)

  v3 = add(v1, v2)
  v4 = v1.add(v2)
  v5 = v1.add(v2).add(v4)
```

I really like this idea, until it combine UFCS together with regular OOP like in DLang. 
Now you can't tell by reading the code whether `foo.bar()` is a method of class `Foo` or a regular 
function taking `Foo` as the first argument.

## Extension methods
There are also extension methods. In OOP languages, they solve the problem of adding new functionality 
to a third-party class (for example, from a third-party lib that cannot be changed)  
A simple example, we can extend the built-in String type with our own wordCount method instead of 
creating some strange StringUtils class (though in C# the class will still have to be created).  
[C#](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/extension-methods#orderby-example)

```C#
public static class MyExtensions
{
	// mention "this" here
	public static int WordCount(this string str)
	{
		return str.Split(" ");
	}
}
// ...
public static int Main() 
{
	string s = "Hello Extension Methods"; 
	int i = s.WordCount();
}
```
Kotlin
```Kotlin
fun String.wordCount() = this.split(" ")
fun main() {
	val s = "Hello Extension Methods"
	val i = s.wordCount()
}
```

Under the hood, it works exactly the same as UFCS, it just creates a function that takes as its 
first argument.
```java
public final static wordCount(Ljava/lang/String;)Ljava/util/List;
...
LINENUMBER 16 L0
LDC "Hello Extension Methods"
INVOKESTATIC MainKt.wordCount (Ljava/lang/String;)Ljava/util/List;
POP
```
## Niva
Niva is a typed Smalltalk, meaning everything is an object, 
everything has methods (unlike the functional approach), and there are no top-level functions,
which means the extension methods approach will suit the best.

```Scala
String wordCount = this split: " "

"Hello Extension Methods" wordCount
```

However I also like the UFCS approach from nim (niva = nim + vala) 
so why not simplify things and make all methods extension methods.  
That is, from the outside it seems that everything is as usual, but in fact, everything is a static 
function that takes the receiver as the first argument (what becomes `this`). 
This means it will work much more efficiently.  
Static call is the cheapest and easiest to optimize, since it does not require searching for a method 
in ancestors, or going to the virtual method table)

Here are the types of calls in the order of deceleration
- INVOKESTATIC
- INVOKEVIRTUAL - extracting a reference to a method from a pool of constants, checking that the object is an object of the specified class or its subclass, searching for a method from the actual class (dynamic dispatching), call
- INVOKEINTERFACE - extracting a reference to an interface from a pool of constants, checking whether the object implements this interface, searching for a specific method in the class hierarchy, call
- INVOKEDYNAMIC - nothing is known, the call was most likely through reflection, ala every js/python call

Ofc, these paths are memorized after a few calls without changes.

Niva does not have inheritance, interfaces, abstract classes, @Override and everything else 
inherent in OOP, but still sending messages remains the main way of computing, this is Smalltalk!  

### Then where do you get polymorphism?

I will specifically give this idiotic example from Wikipedia:
```Java
abstract class Animal {
    protected String name;
    protected int age;

    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public abstract void makeSound();
}

class Dog extends Animal {
    public Dog(String name, int age) {
        super(name, age);
    }

    @Override
    public void makeSound() {
        System.out.println(name + " says: Woof!");
    }

    public void fetch() {
        System.out.println(name + " is fetching the ball.");
    }
}

class Cat extends Animal {
    public Cat(String name, int age) {
        super(name, age);
    }

    @Override
    public void makeSound() {
        System.out.println(name + " says: Meow!");
    }

    public void scratch() {
        System.out.println(name + " is scratching the furniture.");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal myDog = new Dog("Rex", 5);
        Animal myCat = new Cat("Whiskers", 3);

        myDog.makeSound(); 
        myCat.makeSound(); 

        if (myDog instanceof Dog) {
            ((Dog) myDog).fetch();
        }

        if (myCat instanceof Cat) {
            ((Cat) myCat).scratch();
        }
    }
}

```

So my answer is tagged unions - sums of types with a tag for each of the branches.

```Scala
union Animal name: String age: Int = 
| Dog
| Cat

// exhaustive match
Animal makeSound = | this
| Dog => this woof // type narrowing, this is Dog here 
| Cat => this meow

Dog woof = "$name says: Woof!" echo
Cat meow = "$name says: Meow (=^･ω･^=)" echo

// specific
Dog fetch = "$name is fetching the ball" echo
Cat scratch = "$name is scratching the furniture" echo

StrangeZoo animal::Animal = [
	animal makeSound
	
	| animal
	| Dog => animal fetch
	| Cat => animal scratch
]
```

I named the makeSound woof and meow methods differently just for clarity, 
they could easily all be named the same(makeSound) since they have different receiver types.  

What are the benefits?  
As I mentioned earlier, now instead of vtable we manually select a method depending on the tag 
(the `Animal makeSound` method). If we assume that with JIT the performance of 
INVOKEINTERFACE will be the same as static, we still have explicitness.

By going to the definition of makeSound, we will clearly see all possible options and can easily move 
on to specific implementations. I have come across piles of a huge number of abstraction levels 
(Clean Code like) many times in production, which is actually a frequent criticism of OOP.  

The second advantage is exhaustive. If we add a new Animal heir to the previous variant, 
all matches will immediately light up red, requiring the missing variant to be implemented.  
Also, all this simply takes up less space and is easier to read.

### Sealed classes and enums of Java 17/C#/Dart/Swift/Rust
Yes, a similar concept was added to Java 17 and C#. But
1) A huge amount of code has been written and continues to be written without using sealed
2) Migration to new versions is very slow
3) Everything else is still there
4) Exhaustive check is not present everywhere (for example, it is not in C#)

In niva there is no other way to achieve polymorphism, which is the main difference, it is an experiment.

### The problem of extensibility
