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

A convenient way of storing of frequencies is a Clojure collection type known as a *hashmap*, also referred to as just a *map* - just don't confuse it with a function `map`. 

A map is a sequence of pairs of a key and a value. It is written in curly braces. For instance, `{"apple" 2, "banana" 3}` is a map with a key `"apple"` bound to a value `2`, and a key `"banana"` bound to a value `3`. 

Keys and values can be anything: numbers, strings, vectors, etc. It is common to use a special Clojure type known as a *keyword* for keys, as it may make lookups in larger maps slightly faster, but this is by no means required. Keywords in Clojure are written like this: `:apple`. 

Since we are computing frequencies of characters, our keys will be characters, and values will be counts or percentages.

Here are a few functions that work with maps: 

**To-do**: `assoc`

## Counting the number of occurrences of a character 

In order to compute frequencies of letters, we need to find out how many times each letter appears in the string. 

**To-do**: `count` 

**To-do:** the approach: filterv and count. 

## Clojure ranges
**To-do:** explain ranges, explain why it works

```clojure
(mapv to-char (range 26))
```  

## Clojure sorting

**To-do** sort the map on frequencies. 


