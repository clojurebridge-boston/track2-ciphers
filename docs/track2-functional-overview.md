# Overview of Clojure. 

## Principles of functional languages

Clojure is a functional language. This means that rather 
than updating variables in memory, programs work by function composition. Functions in a functional language are treated as other elements of the language (numbers, strings, etc.), i.e. they can be stored in a variable, passed to other functions, returned from other functions, and even created at run time. This is often refers to as *functions being first class citizens in the language*. 

Variables and data structures in Clojure are immutable by default, i.e. they never change in place. Every time a modification is needed, a new object is created. For instance, if you add an element to a list, a new list is created with the new element added, and the old list stays the same. 

This may seem like an inefficient approach, but underneath there is a very efficient sharing of the parts that didn't change, but it's invisible to the programmer. Clojure vectors are in particular efficient in this sense. 

The benefit of immutability is that it is easier to know exactly what's happening in a program since every object that a program references is always the same as when it was originally created. This also makes it easier to incorporate concurrency since there is much less of a need to keep track of simultaneous updates, so much less of a need to lock objects. In fact, one of the main design goals of Clojure was the ease of concurrent programming with minimal locking. 

## Clojure and Java

Clojure compiles into Java bytecode and runs on Java Virtual Machine (JVM). There is a convenient way to call Java methods and use Java libraries in Clojure. This means that Clojure can use any predefined Java functionality. 

Since Clojure is implemented on top of Java, Clojure objects (numbers, strings, lists, etc.) are implemented as Java datatypes. 
Unfortunately, this also means that Clojure error messages are Java exceptions and refer to Java datatypes. This makes them less understandable to those not familiar with Java. We will try to help you navigate through error messages if they become confusing. 

## Clojure Syntax 

Clojure is a language in the Lisp family. Lisp languages are functional, dynamically typed languages with a simple uniform syntax. In Lisp languages there are a handful of keywords (such as Clojure `def` for defining a variable) and no predefined operations: even arithmetic operations, such as `+`, `-`, etc. are functions in Clojure. 

Clojure syntax follows prefix notation: a Clojure expression is surrounded with parentheses, the first element in the parentheses is a function name, and what follows is the function arguments:
```clojure
(+ 2 5)
```
is the call to a function `+` with the arguments `2` and `5`, which returns `7`. 

Many functions in Clojure take a variable number of arguments: 
```clojure
(+ 2 5 3)
```
passes three arguments to `+`, and returns `10`. 

Since functions in Clojure are first-class citizens, they can be passed to other functions. For instance,
```clojure
(filter odd? [2 3 -1 8])
```
is a call to a function `filter` that takes a function that returns true/false values and a collection of elements, and returns a new collection that only contains the elements for which the function returned `true`. In this case, only the elements `3` and `-1` will be in the resulting collection. 

All languages have their own naming and formatting conventions. Clojure (and other Lisp languages) use the following:

* Multi-word variable names use dashes, not underscores or camel-casing: `find-min-key`.
* Function names tend to be verbs, not nouns: `take`, `reduce`, etc.
* Names of functions that return boolean (true/false) values end with a question mark: `odd?`, `number?`. 
* Parentheses all close on the last line of a nested expression, as in the follwoing function definition: 
```clojure
(defn square
  [x]
  (* x x))
```

Comments start with `;` and go until the end of the line:
```clojure
(def n 10) ; n is the number of elements
```
Traditionally we use `;;` to start a comment that takes the 
entire line. 

For more on Clojure style please see [The Clojure Style Guide](https://github.com/bbatsov/clojure-style-guide) (and you might want to revisit this link after you are done with this lesson). 

## Clojuredocs 

To-do: what clojuredocs are and a bit on how to read them 

## Creating a project with Nightcode

To-do: how to run it

To-do: definitions in upper right panel, test functions in REPL. 

## About this lesson

To-do: what it is and what it isn't 

**Next:** [Clojure data types and functions](track2-functions.md)