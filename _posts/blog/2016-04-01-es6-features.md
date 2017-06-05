---
layout: post
title: "Ecmascript 6: the fun bits (part 1)"
date: 2016-01-04 16:54:46
author: Juan Carlos Cancela
categories: 
- blog
- javascript
- coding
img: js.jpg
thumb: code.jpg
---

Ecmascript has become an official standard during last months, so well worth to revisit some of the new features defined
in the new spec.

![Screenshot](http://codecondo.com/wp-content/uploads/2015/06/20-Resources-on-ES6-for-JavaScript-Developers.jpg){: .center-image }


## var vs let

In ES5/6, using var for variable declarations trigger an internal javascript engine process that "hoists" (move variable
to the top of the declaration) variables:

{% highlight javascript %}
// Using var, variable declaration is moved to the top of the function
// declaraction, no matter where is initially declared.

function dosth(value){
  if(value){
    var test;
    console.log("value");
  }

  // Intituively you may think test variable does not exist if value is not true,
  // but due to variable hoisting, thats not true.
  console.log("finished execution");
}

// This is how Javascript changes your function under the hood
function dosth_hoisted(value){
  var test;

  if(value){
     console.log("value");
  }

  console.log("finished execution");
}
{% endhighlight %}


With ES6, using let, it is possible to declare variables using let, allowing real block scope declarations. Check this
example:

{% highlight javascript %}
// Variables initialized with let follow the traditional approach of defining
// block level declarations (so, no variable hoisting, so, what you usually
// se in traditional Java like languages)
function dosth_let(){
  if(value){
    let test;
    console.log("value");
  }

  // If value is not evaluated as true, then test value wont exist.
  console.log("finished execution");
}
{% endhighlight %}


#### const to freeze keys of an object (but not values!)

const under ES6 is a block scope declaration, and furthermore, it allows freezing an object key set (but not its values). 
See this example:

{% highlight javascript %}
// Defining a const computer object, it wont be possible to add new keys to
// computer object.
const computer = {
  brand: "Apple",
  model: "II"
}

// valid!
computer.brand = "Microsoft";

// throw error
computer.price = "1564";
{% endhighlight %}


#### String interpolation

Another powerful feature in ES6 is string substitution. Let's see an example:
 
{% highlight javascript %}
// Prints numbers from 0 to 9
var countNumbers  = function(){
  for(var i = 0; i < 10; i++){
    console.log(`${i}`)
  }
}
{% endhighlight %}

As you can see, using backticks and ${variable}, you can interpolate strings with calculated values.


#### Default Parameters for function

Some subtle improvements were added to function manipulation in ES6. In particular, let's see how default values are 
handled:


{% highlight javascript %}
// If protocol parameter is not passed, it defaults to https
function create_url(protocol = "https", domain){
  return `${protocol}://${domain}`;
}
{% endhighlight %}


Even better, it is possible to send functions as default values. Check this example:

{% highlight javascript %}
// If date is not provided, defaults to current date
function print_date(date = getCurrentDate()){
  console.log(d);
}

function getCurrentDate(){
  return new Date();
}
{% endhighlight %}


#### More in github :) 

All code snippets are published in [repo](https://github.com/juancancela/es6-examples). ES6 is huge, and there are lots 
of new, powerful features. I'll be periodically updating repo with new examples. 
In the future, I'll be writing specific articles talking about some of the most complex features in ES6, in example,
Generators and Proxies. Enjoy!
