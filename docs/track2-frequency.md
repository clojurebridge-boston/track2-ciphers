# Clojure maps and frequency analysis 

## Frequency analysis for breaking ciphers

English language, as most languages, has an even distribution of letters. The most frequent letter in English is `e`, which, on average, constitutes close to 13% of English texts, followed by `t` (about 9%) and `a` (a bit over 8%). The least frequent letter is `z` which occurs less than 0.1%.

A complete distribution of English letters is given here: [Relative frequencies of letters in the English language (wikipedia)](https://en.wikipedia.org/wiki/Letter_frequency#Relative_frequencies_of_letters_in_the_English_language) You might want to open this link in a new tab since you would need it. 

The non-even distribution of letters and the fact that every letter is always encrypted by the same letter (no matter where it appears in the text) allow breaking a Caesar cipher encryption (i.e. finding out the key) much faster than trying all possible keys and looking if the encryption contains any words. This approach is known as *frequency analysis*. 

Note that we need a somewhat long English text in order to do frequency analysis: a single word is not enough. 

Our example will be this text encrypted with Caesar cipher with an unknown key:

```clojure
"radyjgtxhpsncpbxrvtctgpaejgedhtegdvgpbbxcvapcvjpvtrdbqxcxcv
iwtpeegdprwpqxaxinpcsxcitgprixktstktadebtciduphrgxeixcvapcvjp
vtlxiwpctuuxrxtcipcsgdqjhixcugphigjrijgtudgbjaixiwgtpstsegdvg
pbbxcvo"
```

## Clojure hashmaps

*Relevant functions:* [assoc](https://clojuredocs.org/clojure.core/assoc), [zipmap](https://clojuredocs.org/clojure.core/zipmap)

A convenient way of storing of frequencies is a Clojure collection type known as a *hashmap*, also referred to as just a *map* - just don't confuse it with a function `map`. 

A map is a sequence of pairs of a key and a value. It is written in curly braces. For instance, `{"apple" 2, "banana" 3}` is a map with a key `"apple"` bound to a value `2`, and a key `"banana"` bound to a value `3`. 

Keys and values can be anything: numbers, strings, vectors, etc. It is common to use a special Clojure type known as a *keyword* for keys, as it may make lookups in larger maps slightly faster, but this is by no means required. Keywords in Clojure are written like this: `:apple`. 

Since we are computing frequencies of characters, our keys will be characters, and values will be counts or percentages.

Here is some of functionality of maps: 

1. `{}` is an empty map. 
2. You can add elements to a map using a function `assoc` by providing a key and a value. If a key given to `assoc` is already present, the value gets replaced by the new one.  
```clojure
(assoc {} "a" 3) ; results in {"a" 3}
(assoc {"a" 3} "x" 2) ; results in {"a" 3, "x" 2}
(assoc {"a" 3, "x" 2} "a" 5) ; results in {"a" 5, "x" 2}
```
3. The main use of maps is to be able to look up a value for a key. There is no special function that does it: the map itself serves as a function that, when passed a key, returns its value. If the key isn't found, a special value `nil` is returned:
```clojure
({"a" 3, "x" 2} "a") ; results in 3
({"a" 3, "x" 2} \a) ; results in nil (\a is different from "a')
({"a" 3, "x" 2} "b") ; results in nil
({} "any") ; results in nil
```
4. Another way of creating a map is to give it a vector of keys and a vector of corresponding values. The function that does this is called `zipmap`:
```clojure
(zipmap [\a \b] [2 5]) ; results in {\a 2, \b 5}
```

**Exercise:** Practice with maps. For instance, create a small map of animals and their sounds (e.g. "duck"/"quack"), and look up animal sounds by the animal. 

## Counting the number of occurrences of a character 
*Relevant functions:* [count](https://clojuredocs.org/clojure.core/count), [first](https://clojuredocs.org/clojure.core/first), [second](https://clojuredocs.org/clojure.core/second), [=](https://clojuredocs.org/clojure.core/=).

In order to compute frequencies of letters, we need to find out how many times each letter appears in the encrypted string. We also need to work with vectors that are pairs of a key and a value. 

The functions that we need for work with vectors (not necessarily all in this section) are:

1. `count` that returns the  umber of elements in a vector: `(count ["apple" "banana"])` returns `2`.
2. `first` returns the first element of a vector: `(first ["apple" "banana"])` returns `"apple"`. 
3. `second` returns the second element of a vector: `(second ["apple" "banana"])` returns `"banana"`, so does `(second ["apple" "banana" "kiwi"])`

Note that these functions also work with sequences (we will see those later). 

Now we are back to writing Clojure functions. We need to write a function `count-letters` that, given a character and a string, would return the number of occurrences of this character in the string. For instance,
```clojure
(count-letters \a "aadvark") ; 3
(count-letters \x "aadvark") ; 0
```

One (not the most efficient, but straightforward) way is filter out all the other characters, and then return the count of the result. In Clojure `=` is a function that can be used on any two objects (be it numbers, strings, vectors, etc.). For instance, `(= (/ 6 2) 3)` returns `true` (note that `(/ 6 2)` is the result of dividing 6 by 2), and `(= "97" 97)` returns `false` since a string is not equal to a number. 

Here we need to use `filterv` with an anonymous function that checks if its parameter is equal to the given character, and then we need to return the `count` of the result.  

**Exercise:** Look at your previous function that uses `filterv`, and then write a call to `filterv` to filter out all characters other than `\a` in the word `"aadvark"`. Once you have accomplished this task, make it into a function `count-letters` that works as in the example above. Try it on a different word and with a different character.

## Clojure lazy sequences, ranges
*Relevant functions:* [range](https://clojuredocs.org/clojure.core/range), [take](https://clojuredocs.org/clojure.core/take), [drop](https://clojuredocs.org/clojure.core/drop)
**To-do:** explain ranges, explain why it works

```clojure
(mapv to-char (range 26))
```  

**To-do:** Use zipmap to create a map of letter/count pairs. 

## Clojure sorting
*Relevant functions:* [sort-by](https://clojuredocs.org/clojure.core/sort-by), [last](https://clojuredocs.org/clojure.core/last), [reverse](https://clojuredocs.org/clojure.core/reverse)

**To-do** sort the map on counts, `last`, `drop`

Alternatively, you can reverse the resulting list after you have sorted it, reverse it using the function `reverse`, and take the first item or the first few items to find likely key values. 

## Computing the cipher key

**To-do:** explain, but not show, the formula for the key. 

**Exercise:** Now try this approach on the string "ahixblmaxmabgzpbmayxtmaxklmatmixkvaxlbgmaxlhnetgwlbgzlmaxmngxpbmahnmmaxphkwltgwgxoxklmhiltmtee".

**Exercise:** Also, try uploading your own longer examples of encryption (but not the key) to slack and try figuring out the keys for other examples uploaded there. Make sure they are long enough for frequency analysis. Use only English texts. 

**Next:** [Vigenere cipher](track2-vigenere.md)
<br />
**Previous:** [Caesar cipher](track2-caesar.md)