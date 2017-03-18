# Vigenere cipher

## Overview of the cipher
Vigenere cipher addresses two main weaknesses of Caesar cipher: Caesar cipher has only 26 keys, and all occurrences of a letter are always encoded by the same letter in the encrypted text. 

Here is how it works: we pick a key which is a word. Let's use the word "cipher". The phrase we would like to encrypt is "Welcome to ClojureBridge!", which turns into "welcometoclojurebridge" after removing non-letters and converting to all lowercase. 

Now imagine taking the phrase that you would like to encrypt and then writing the key word under it as many time as needed to get the same length:
```
welcometoclojurebridge
cipherciphercipherciph
```
Let's take `w` (the first letter of the text we are encrypting) and `c` (the letter of the keyword right under it). Then `w` is encrypted as taking its integer value, `22`, adding the integer value of `c`, `2`, adding them together, and taking the result modulo 26 if it is greater than 25. The result will be `24`, which corresponds to the letter `y`. 

The encryption of `e` is the sum of its value `4` and the value of `i` (the letter written under `e`) modulo 26, so it's `4 + 8 = 12`, which is `m`. 

If you continue the process, you will get the following result (the third line below):
```
welcometoclojurebridge
cipherciphercipherciph
ymajsdgbdjpflcglfiklvl 
```
Note that the two letters `e` in "welcome" are encoded differently: as `m` in the first case, and as `g` in the second. This hides patterns in the text and doesn't allow frequency analysis the way we did it for Caesar cipher since the same occurrence of a letter in ciphertext may've come from different letters in the original text. 

**Exercise** Write a function `encrypt-letter` that, given two characters, returns the first one encrypted with the second one. For instance, given `\w` and `\c`, it returns `\y`. Make sure to test it on examples in which addition results in a number higher than 25 (encrypting `\u` with `\i`, for instance, gives `\c`). 

## Encrypting and decrypting with Vigenere cipher
*Relevant functions:* [cycle](https://clojuredocs.org/clojure.core/cycle) (new), [map](https://clojuredocs.org/clojure.core/map), [mapv](https://clojuredocs.org/clojure.core/mapv) revisited. 

Now we are going to use `mapv` to encrypt longer texts. 

To do that, we consider a slightly different example of using `mapv` (or `map`): we can use it with multiple sequences. The simplest example is:
```clojure
(mapv + [6 7 8] [3 2 1]) ; [9 9 9]
(mapv - [6 7 8] [3 2 1]) ; [3 5 7]
```
Here `mapv` applies the given function (`+` in the first example, and `-` in the second case) to the first elements of the two given vectors, then to the second ones, etc. 

Of course, we can use it with anonymous functions as well; the function must have two arguments:
```clojure
(mapv #(if (< %1 %2) %1 %2) [2 4 5] [3 6 1]) ; [2 4 1]
```
In this case the anonymous function returns the smaller of its two arguments. Thus, given `2` and `3`, it returns `2`, which is the first element of the result. Then given `4` and `6`, it returns `4`, etc.

**Exercise:** Use `mapv` to compute the sum of squares of the elements of the given vectors. For instance, given `[1 2]` and `[3 1]`, it would return the vector `[10 5]`. 

Here is a cool feature: if one the given sequences is longer than the other, the result is as long as the shorter sequence, and the remaining elements are ignored:
```clojure
(mapv + [6 7 8] (range)) ; [6 8 10]
```
This allows us to use a `cycle` function to generate an infinite sequence of repeated keyword and then "add" it to text to be encrypted. `cycle` takes a sequence and creates an infinite lazy sequence by repeating the given sequence. For instance,  
```clojure
(def cycle1 (cycle [1 2 3])) ; infinite cycle
(take 10 cycle1) ; (1 2 3 1 2 3 1 2 3 1)
(def cycle2 (cycle "word")) 
crypto-solutions=> (take 10 cycle2) ; (\w \o \r \d \w \o \r \d \w \o)
```

**Exercise:** Use the cycle of a keyword "`cipher"`, `mapv` (or `map`), and the `encrypt-letter` function that you wrote earlier to encrypt the string `"welcometoclojurebridge"`. 

**Exercise:** Once you get it to work, write a function `vigenere-encrypt` that takes a text to encrypt and a keyword and performs the encryption. Just like in the Caesar cipher, you need to first use `get-letters` to get rid of other symbols and convert the letters to lowercase. 

**Exercise:** Now write a function that takes an encrypted string and a keyword and performs the decryption. Note that you will be doing subtraction modulo 26 instead of the addition. 
<br />
Make sure that when you encrypt a string with a keyword, you can get it back (except in all lowercase letters and without punctuation) by decrypting it with the same keyword. 

**Exercise:** Post an encryption (with a key) on slack. Try decrypting other people's encryptions.  

## Breaking Vigenere cipher
It seems like a Vigenere cipher doesn't allow frequency analysis since the same letter may be encrypted in multiple different ways. However, one can actually guess a key by a process that's just slightly more complicated than breaking a Caesar cipher. It also requires a longer text. 

Suppose you know the length of the key. Let's say, the key is 5 letters long. That means that if I take letters in the encrypted text at positions 0, 5, 10, 15, etc., they are all encrypted with the same letter of the key, so they are all shifted by the same amount. The same is true for letters at the positions 1, 6, 11, 16, etc. 

This means that you can do frequency analysis on each of these groups of letters, and determine a likely one-letter key  for each group exactly the same as you determined it for the Caesar cipher. Putting these one-letter keys together forms the keyword that you are trying to guess. 

But how do you know the length of the key??? It turns out that if you take letters in the encrypted string at the distance that equals to the length of the keyword (as we have done above), their frequencies are very uneven, much like the frequencies of letters in the English alphabet. However, if you take any other distance, frequencies tend to be more uniformly distributed (closer to random noise). 

Armed with these ideas, you can now try it on the example given at the end of this section. Below we explain how various Clojure functions can be used for this task. 

### Part 1: determine the length of the key

*Relevant functions:* [reduce](https://clojuredocs.org/clojure.core/reduce), [take-nth](https://clojuredocs.org/clojure.core/take-nth), [assoc](https://clojuredocs.org/clojure.core/assoc) (revisited). 

### Part 2: frequency analysis on the subsequences

Here is an example for you to try to break. The key is between two and 8 characters long. Be patient, this may take time. Try to automate you process as much as possible. We also recommend that you work in pairs. 
```
rzsrppgeamjllagcpwxismxxcalecwygluetcaguepwwlznpclepcsgcpkgbac
ltcifstvntybwsepwutzkinweettwgqwjpnweefbwgazgvciebtvyalvyjlowh
smhdacdpcqrtobzttlwpznepnpacpqfspxwcomfiazgvciebtvyalvyjlowhhp
arstwsewlwplwkptgexmfiznudmwddymguepwutzkisqywwmntwxjdrzsbxqfv
wifvfiytdwoxyoldsmjpnkgbatahsuwceascopwgyinpwzscvazthikhzuwitu
whcmxtczwsewshlusotvyvciutepwjdvskjijapqywmcjzpkdpdayjtlwsxqkh
ttwspalgzgwgfakwzxhtceshyietonggsmjhsmopdxghepmbzckajiopclwsep
wecmkxomfitidbplsaznxgpmvdxjqecmkxomfimijpnsgqlus
```

## Are we done? 
This was a long day! However, I do recommend that now you take a few minutes and go back to the overview page and read the description of Clojure and functional programming again. Hopefully a lot of this now would be something that you can relate to today's experience. 

#### Thanks for attending ClojureBridge!!! 

**Back to the beginning:** [Overview](track2-functional-overview.md)
<br />
**Previous:** [Breaking Caesar cipher](track2-frequency.md)