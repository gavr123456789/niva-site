# Coding

First of all there are no some special project file structure, config files or build system.  
Single niva file is already can be a project.  

## Entry point and top level statements
main.niva
```Scala
1 echo
```
Now try `niva run`    

By default, a file with this name is considered the entry point to the program.  
You can specify a different entry point with `niva run file.niva`(can be found in `--help`)

All top-level code is executed from it.
Top level expressions from other files are ignored, until you run the project from them.  
This choice of entry point makes it convenient to experiment while writing 
modules, imagine that you are writing a GUI application, and each file 
contains a separate widget.
You can write the code for constructing this 
widget at the top level of the file and run it to check how it looks 
separately from the entire project.


```Scala
// car.niva
type Car
Car brrr = "brrr" echo

testCar = Car new 
testCar brrr

// seller.niva
type Seller name: String
Seller sayHello = "Hi, my name is $name"
Seller sell::Car = "*slaps car*" echo

testSeller = Seller name: "Bob"
testSeller sayHello

// main.niva
car = Car new
seller = Seller name: "Bob"
seller sell: car
```
Now with `niva run` only the code from main will be launched:  
❯ niva run  
*slaps car*  
With `niva run car.niva` only the top level code from car file
❯ niva run car.niva  
`brrr`  
And the last one `niva run seller.niva`   
❯ niva run b.niva  
`Hi, my name is Bob`  

## No global state
This feature makes it impossible to create a global mutable state.  
Since top-level variables are actually local to the current entry point:

```Scala
type Example
type Person age: Int
Person birthday = age <- age inc

globalPerson Person age: 21
Example useGlobal = [
    globalPerson birthday // ERROR
]
 

```


## Packages 
The source files in niva do not have the same meaning as in ordinary languages, but they successfully pretend.
In fact, niva uses the image approach (as in Smalltalk, Factor, Common Lisp),
it's just that this image is built from scratch every time since it does not yet support serialization deserialization.

With that, it becomes possible to completely untie the file hierarchy from the package structure.  
By default file name = package name and 1 file is 1 package.  
But you can make several packages in one file:
```Scala
Project package: "aaa"
type Person name: String wallet: Wallet
Project package: "bbb"
type Wallet money: Int
```



