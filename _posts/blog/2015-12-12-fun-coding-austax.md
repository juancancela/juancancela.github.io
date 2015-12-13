---
layout: post
title: "Code for fun: Australian Tax Calculator!"
date: 2015-12-12 16:54:46
author: Juan Carlos Cancela
categories: 
- blog
- javascript
- coding
- nodeJS
img: programming.png
thumb: code.jpg
---

So a time ago I was required to do a simple test. Objective was to create an app to generate australian employee payslip. 
In particular, given an annual salary, the super rate and initial period date, it is required to return the gross and net
income, super as well as income tax.

![Screenshot](https://camo.githubusercontent.com/a3e6cf63e229718e69d044dbcb9e113d94b7a8d6/68747470733a2f2f75706c6f61642e77696b696d656469612e6f72672f77696b6970656469612f636f6d6d6f6e732f7468756d622f622f62392f466c61675f6f665f4175737472616c69612e7376672f32303070782d466c61675f6f665f4175737472616c69612e7376672e706e67){: .center-image }


## CLI

Originally, app was an http service, but since it would eventually be useful for someone, I refactored it so it can be used 
in a much more simpler fashion as a command line tool. So, if you want to know how much government will grab from your
money, just execute something like

{% highlight javascript %}
$ austax ANNUAL_SALARY SUPER_RATE INITIAL_PERIOD
{% endhighlight %}

In example:

{% highlight javascript %}
$ austax 100000 0.4 12/12/12
{% endhighlight %}

####Code

I have published solution and tests in this [repo](https://github.com/juancancela/austax), as well
as a couple of tests for it. Enjoy!
