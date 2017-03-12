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

## Working with words: sequences, `map`

## Working with strings