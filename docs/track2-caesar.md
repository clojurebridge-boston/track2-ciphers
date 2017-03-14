#Caesar cipher

## Letter shift 
*Relevant functions on clojuredocs:* [+](), [-](https://clojuredocs.org/clojure.core/-), [mod](https://clojuredocs.org/clojure.core/mod)

**to-do** a link to https://clojuredocs.org/clojure.core/+ is having an issue: + is interpreted as redirect. 

Caesar cipher works by shifting the alphabet by a given number of positions to the right, wrapping around at the end. 
The key for the cipher is how many positions the letters are shifted by. 
For instance, if the key is 3 then `a` is replaced by `d`, `b` by `e`, etc. Here is the shift of the entire alphabet:
```
abcdefghijklmnopqrstuvwxyz
defghijklmnopqrstuvwxyzabc
```

In order to write a function `shift` that shifts a letter
by a given number, we need to:

1. Convert the letter to an integer number using `to-int` function.
2. Add n to it.
3. Take the result modulo 26 (that would allow us to wrap around). For instance, if the letter is `x` (position 23 in the alphabet, where `a` is 0), and the shift is 3, the result would be 23 + 3 = 26. The largest letter is `z`, at the position 25, so 26 should result in 0. Taking the result modulo 26 accomplishes this task. 
4. After computing the position, we need to convert it back to a character by applying the `to-char` function that you wrote earlier. 

The function that performs modulo arithmetic is `mod`. Here are a few examples of how it works:
```clojure
(mod 7 26) ; result: 7
(mod 27 26) ; result: 1
(mod 55 26) ; result: 3
(mod -5 26) ; result: 21
```

**Exercise:** Write a function `shift` according to the description above. Some examples for how it should work:
```clojure
(shift \a 3) ; result \d
(shift \b 20) ; result \v
(shift \z 3) ; result \c
```
Note: if you shift by a negative number, you are performing a reverse operation. For instance, `(shift \d -3)` gives you `\a`. Thus decryption is just using the same function, but with the opposite (negative) key.  

## Working with words: sequences, `map`
Now you can "encrypt" a letter, but you probably want to encrypt words. If you were writing a program in python or Java, you probably would be thinking of writing a loop. However, in Clojure we use *higher-order functions* that traverse sequences for us, and we just need to specify what operation we would like to perform on each element. 

`map` is one of such higher-order functions. It takes in a sequence of elements and a function, and returns a sequence that results from applying the given function to each element. 

This sounds very abstract, so let's look at an example. We use a function `inc` (increment) that takes an integer and returns the next integer, i.e. `(inc 1)` returns 2. now we are going to increment each element of a sequence of numbers
using `map`:
```clojure
(map inc [1 3 2]) ; returns (2 4 3)
```
Here `[1 3 2]` is a *vector* of numbers. This is the easiest way of giving Clojure a collection of elements in a specific order. What you get back is also an ordered collection of elements, each of them is incremented by 1. The reason it appears in parentheses is because it's a slightly different type of a collection. We will not worry about these details now, just remember that when you are giving Clojure a collection of items, the easiest way to write it is a vector.

**Exercise:** What do you expect when you type in Clojure REPL?
```clojure
(map to-int [\a \b \c])
``` 
Try it, see if the result is what you were expecting. If it's not, make sure to understand what it is and why. 

**Exercise:** Copy the following definition of the function 
`square` into the definitions panel of Nightcode (right upper panel): 
```clojure
(defn square
  "Takes a number and returns its square"
  [x]
  (* x x))
```
Reload the file. Now in the REPL panel **type in** an expression, **using `map`**, that computes the squares of numbers `[1 3 -2]`.

### Using anonymous functions
Maps are often used together with anonymous functions. These
are one-time-use functions that are put together "on the fly" and not given a name. they also don't given names to their parameters, referring to them as `%1, %2, %3` - or just `%` if there is only one. 

They are often used with higher-order functions, such as `map`. Here is an example:
```clojure
(map #(* % %) [1 3 -2])
``` 
This returns `(map #(* % %) [1 3 -2])` (the sequence of squares of all given numbers, just like in the exercise above). 
The anonymous function passed to the `map` is `#(* % %)`. It is equivalent to the `square` function above. The `%` sign here refers to the parameter of the function, it is used instead of `x`. The `#` in front of the expression indicates that this is a function. 
      
**Exercise:** Use `map` and an anonymous function to take the opposite of each number in a given vector. For instance, if the vector is `[2 -1 0 3]`, the result would be `[-2 1 0 -3]`.

### Converting between strings and sequences 
`map` is very convenient for working with sequences. Strings, however, are not a sequence of characters, so some conversion is needed. 

In order to convert from a string to a sequence of characters, you can use a `seq` function: 
```clojure
(seq "word") ; results in a sequence (\w \o \r \d)
```

The reverse conversion is a bit less obvious:
```clojure
(apply str [\w \o \r \d]) ; results in a string "word"
```
Here a sequence of characters is given as a vector. 

**Explanations of `apply` (you can skip this):**
TO-DO: add a link to Clojure docs. 

Now we are done with the nitty-gritty details for our ciphers, and are ready to do some encryption and decryption.

### Encrypting with Caesar cipher

Now we can encrypt words with Caesar cipher. Let's say we want to encrypt the word "apple" by shifting the alphabet by 20. We need to do the following steps:

1. make the word into a sequence by using `seq` function
2. use `map` to shift each letter in the sequence by 20 positions; we can write the actual shifting as an anonymous function that uses the function `shift` that we wrote earlier.
3. use `apply str` to convert the result from a sequence to a string. 

Feel free to write this out on paper or in Nightcode before you look at the solution below. 

```clojure 
(def s (seq "apple")) ; s is the sequence (\a \p \p \l \e)
(def encr-seq (map #(shift % 20) s)) ; encrypt the sequence
(def result (apply str encr-seq)) ; convert to a string
```
The result, `"ujjfy"`, is what the encryption of "apple" with the key `20`. 

Instead of saving intermediate results in variables, you can
also write all the steps in one line of code:
```clojure 
(apply str (map #(shift % 20) (seq "apple")))
```
The latter style is more common in Clojure.

Of course, we want to encrypt different words, not just "apple", and use keys other than `20`. Thus, we want to write 
a function that takes a word and a number `k`, and shifts the word by `k`. Here `k` serves as a key for the cipher. 

**Exercise** Below is the start of a function that encrypts 
a word `w` with a key `k`. Fill in the body of the function and test it on some examples. 
```clojure 
(defn caesar-encrypt
  "encrypting a word w with a key k using Caesar cipher"
  [w k]
                                     ) 
```
Don't forget to 
write all functions in the right upper panel of Nightcode, save and reload file, and test the function in the REPL. 

Make sure that `(caesar-encrypt "apple" 20)` 
returns the same result as the expression that you wrote 
earlier, and that passing different words (all lower-case letters, no spaces) and different keys gives you different encryptions. 

## Decrypting with Caesar cipher
Encryption is good only if we can later decrypt the text. 

**Exercise** Based on the function `caesar-encrypt`, write a
function `caesar-decrypt` that takes an encrypted word (all lower-case, no spaces or other symbols) and a key and returns 
its decryption. Recall how we can use the same `shift` function for decryption. 

Test that `(caesar-decrypt "ujjfy" 20)` returns `"apple"`.
Then try your decryption on the following:

- `(caesar-decrypt "gtxyts" 5)`
- `(caesar-decrypt "mvytebolbsnqo" 10)` 

**Exercise** Decrypt your own examples and post them on slack (with the key), then try other participants' examples posted there. 

## Working with strings that have other symbols
