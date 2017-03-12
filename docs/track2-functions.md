# Part 1: Clojure data types and functions

## Clojure datatypes

Clojure is a functional language on top of the Java programming language. Thus Clojure data types are Java data types under the hood. This provides the convenience of using Java methods to work with Java types, but also requires type conversions to maintain consistency with Java types.

Ciphers that we will be exploring in this lesson work with letters that are represented as a character `char` type. However, they are treated as integer numbers `int` to make mathematics of encryption work. Letters form strings, which are a datatype different from characters. 

## Clojure functions

Clojure functions are declared as an expression in parentheses that starts `defn` keyword, followed by the function's name, a vector of parameters (in square brackets), and the body of the function that specifies what the function returns. For instance, the following definition

```clojure
(defn f [x y] (+ x y 100))
```

defines a function `f` that takes two parameters, `x` and `y`, and returns their sum plus `100`. 

You can call this function as 
```clojure 
(f 2 3)
```
This will return `105`. 

**Exercise:** Write a function `g` that takes two numbers `x` and `y` and returns `x` times `10` plus `y`. 

**Exercise:** Look up how `if` works in Clojure **TO-DO: add a link** and returns the absolute value of `x - y`.

## Converting between `int` and `char`

Now we will write some
conversion functions between `int` and `char` types. 

The conversion that we would like to accomplish is to represent letters as numbers `0, 1, 2, ...25`, where `a` is `0`, `b` is `1`, etc. 



```clojure

``` 

