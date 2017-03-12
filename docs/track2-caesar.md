#Caesar cipher

## Letter shift 
Caesar cipher works by shifting the alphabet by a given number of positions to the right, wrapping around at the end. 
The key for the cipher is how many positions the letters are shifted by. 
For instance, if the key is 3 then `a` is replaced by `d`, `b` by `e`, etc. Here is the complete replacement:
```
abcdefghijklmnopqrstuvwxyz
defghijklmnopqrstuvwxyzabc
```
**TO-Do: move modulo discussion to before the list, i.e. here**

In order to write a function `shift` that shifts a letter
by a given number, we need to:
1. Convert the letter to an integer number using `to-int` function.
2. Add n to it.
3. Take the result modulo 26 (that would allow us to wrap around). For instance, if the letter is `x` (position 23 in the alphabet, where `a` is 0), and the shift is 3, the result would be 23 + 3 = 26. The largest letter is `z`, at the position 25, so 26 should result in 0. by taking the result modulo 26 we accomplish this task. 
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

Now we are done with the nitty-gritty details for our ciphers, and are ready to do some encryption and decryption. 

## Working with words: sequences, `map`
Now you can "encrypt" a letter, but you probably want to encrypt words. If you were writing a program in python or Java, you probably would be thinking of writing a loop. However, in Clojure we use *higher-order functions* that traverse sequences for us, and we just need to specify what operation we would like to perform on each element. 

`map` is one of such higher-order functions. It takes in a sequence of elements and a function, and returns a sequence that results from applying the given function to each element. 

This sounds very abstract, so let's look at an example. We use a function `inc` (increment) that takes an integer and returns the next integer, i.e. `(inc 1)` returns 2. now we are going to increment each element of a sequence of numbers
using `map`:
```clojure
(map inc [1 2 3]) ; returns (2 3 4)
```
Here `[1 2 3]` is a sequence of numbers (

**To-do: mention posting encryptions on slack**

## Working with strings