---
layout: post
title: "Code for fun: the value of a word!"
date: 2015-06-12 16:54:46
author: Juan Carlos Cancela
categories: 
- blog
- javascript
- coding
- nodeJS
img: programming.png
thumb: code.jpg
---

Last Friday, I was checking Facebook, and found an ad from a site named "hackealo.co". Out of curiosity, went there,
and was presented with a fun coding challenge:

####Problem:

The "value" of a word is the sum of the value of each of its characters.
The "value" of a character is the product of the position of the character in the work by the position in the alphabet.
IE, the value of "abc" is (1 x 1) + (2 x 3) + (3 x 2)

What is the value of permutations of word "kTBXJJU"?

####Resolution

{% highlight javascript %}
   
   /**
    * Based on ASCII position, returns the value of a character, 
    * taking in consideration the offset.
    * @param character character to which value will be calculated
    * @returns {number} value of character
    */
   function characterValue(position, character) {
       var OFFSET_ASCII = 96;
       return (character.toLowerCase().charCodeAt(0) - OFFSET_ASCII) * 
       (position + 1);
   }
   
   /**
    * Generates the n combinations of the given word
    * @param word word
    * @returns {Array} Array with combinations of word
    */
   function generatePermutations(word) {
       var permutations = [];
   
       function permute(arr, memo) {
           var current;
           var memo = memo || [];
   
           for (var i = 0; i < arr.length; i++) {
               current = arr.splice(i, 1);
               if (arr.length === 0) {
                   permutations.push(memo.concat(current));
               }
               permute(arr.slice(), memo.concat(current));
               arr.splice(i, 0, current[0]);
           }
   
           return permutations;
       }
   
       return permute(word);
   }
   
   function wordValue(word) {
       if (!word) return 0;
       var wordValue = 0;
       var wordPermutations = generatePermutations(word.split(""));
       wordPermutations.forEach(function (combination) {
           for (var i = 0; i < combination.length; i++) {
               wordValue += characterValue(i, combination[i]);
           }
       });
       return wordValue;
   }
   
   /**
    * Public Interface
    * @type {{wordValue: wordValue}}
    */
   module.exports = {
       wordValue: wordValue
   };
 {% endhighlight %}


####Code

I have published solution and tests in this [repo](https://github.com/juancancela/ita-js) in algorithms/extras, as well
as a couple of tests for it.
