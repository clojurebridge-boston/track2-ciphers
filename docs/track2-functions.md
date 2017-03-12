# Part 1: Clojure data types and functions

## Clojure datatypes

Clojure is a functional language on top of the Java programming language. Thus Clojure data types are Java data types under the hood. This provides the convenience of using Java methods to work with Java types, but also requires type conversions to maintain consistency with Java types.

Ciphers that we will be exploring in this lesson work with letters that are represented as a character `char` type. However, they are treated as integer numbers `int` to make mathematics of encryption work. Letters form strings, which are a datatype different from characters. 

## Clojure constants and functions

Constants are declared using the keyword `def`:
```clojure
(def answer 42)
```

Clojure functions are declared as an expression in parentheses that starts `defn` keyword, followed by the function's name, a vector of parameters (in square brackets), and the body of the function that specifies what the function returns. For instance, the following definition

```clojure
(defn f 
  [x y] 
  (+ x y 100))
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

The conversion that we would like to accomplish is to represent the 26 letters of the alphabet as numbers `0, 1, 2,..., 25`, where `a = 0`, `b = 1`, etc. 

Characters in Clojure are written with a backslash in the front. For instance, a character`x` is written as `\x`. 

Since characters are encoded by their ASCII values **TO-Do link** 
Converting a character to an `int` gives its ASCII value.
For instance, `(int \c)`is 99. 
ASCII codes for lowercase letters are in order: `(int \a)` is 97, `(int \b)` is 98, etc. 
Because of that, to convert a letter to an integer between 0 and 25 according to its position in the alphabet, we just need to subtract the ASCII value of `\a` from it:

```clojure
(def ascii-a (int \a))
(defn to-int
  "takes a lowercase letter character and returns its position in the alphabet: a = 0, b = 1, etc."
  [c]
  (- (int c) ascii-a))
``` 
Take a look at the function and make sure you understand what how it works. Try it on a few lowercase letters. 
Also note that we added a description of the function, a so-called doc-string. It is position after the function name and before the parameters. **To-do: add how we can look up doc-strings**

**Exercise:** write a reverse conversion function `to-char`, i.e. a function that takes a number between 0 and 25 (inclusive) and returns the corresponding lowercase letter. For instance, `(to-char 3)` should return `\d`.  

## Caesar cipher 

## Working with words: sequences, `map`

## Working with strings