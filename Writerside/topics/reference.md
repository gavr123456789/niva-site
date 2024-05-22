# Introduction

It will be Smalltalk like language, but statically typed.
Niva targets JVM, and provides easy way to call any Java\Kotlin code.  
On an imaginary graph of complexity, I would put a field here:  
Go < Niva < Java < Kotlin < Scala

Niva is strongly inspired by the forgotten Smalltalk language  

![smalltalk rules.png](smalltalk rules.png)

Niva combines some OOP and functional properties  
  
OOP  
There are no functions, everything is method, in other words there is always a receiver

Functional  
There are no inheritance or interfaces. Instead, tagged unions are heavily utilized.  
This greatly reduces dynamism, the code is easier to understand because you always know which specific method will be called.
