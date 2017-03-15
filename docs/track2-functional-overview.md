# Overview of Clojure. 

## Principles of functional languages

Clojure is a functional language. This means that rather 
than updating variables in memory, programs work by function composition. 

Variables and data structures in Clojure are immutable by default, i.e. they never change in place. Every time a modification is needed, a new object is created. For instance, if you add an element to a list, a new list is created with the new element added, and the old list stays the same. 

This may seem like an inefficient approach, but underneath there is a very efficient sharing of the parts that didn't change, but it's invisible to the programmer. Clojure vectors are in particular efficient in this sense. 

The benefit of immutability is that it is easier to know exactly what's happening in a program since every object that a program references is always the same as when it was originally created. This also makes it easier to incorporate concurrency since there is much less of a need to keep track of simultaneous updates, so much less of a need to lock objects. In fact, one of the main design goals of Clojure was the ease of concurrent programming with minimal locking. 

## Clojure and Java

Clojure compiles into Java bytecode and runs on Java Virtual Machine (JVM). There is a convenient way to call Java methods and use Java libraries in Clojure. This means that Clojure can use any predefined Java functionality. 

Unfortunately, this also means that Clojure error messages are Java exceptions. 

## Clojure Syntax 

To-do: Prefix notation

To-do: Style of naming

To-do: mention comments

## Creating a project with Nightcode

To-do: how to run it

To-do: definitions in upper right panel, test functions in REPL. 

## About this lesson

To-do: what it is and what it isn't 

**Next:** [Clojure data types and functions](track2-functions.md)