---
layout: post
title: "Using NodeJS for command line tools"
date: 2015-06-10 16:54:46
author: Juan Carlos Cancela
categories: 
- blog 
- javascript
- experiences
- nodeJS
img: 2015-06-10-using-node-clt.jpg
thumb: js.png
---

Since arrival of NodeJS, using Javascript to create command line tools (CLT) as a replacement of traditional bash 
scripting is a reasonable option.
 
 
####Creating a simple node app to be used as CLT

Let's create a simple CLT app:
 
 * Create a directory ´cltool´
 * Run ´npm init´ to create a package.json file
 * Replace content of package.json with following data:
 
 {% highlight javascript %}
  {
   "name": "cltool",
   "version": "1.0.0",
   "description": "searches for files",
   "author": "Juan Carlos Cancela",
   "license": "ISC",
   "preferGlobal": true,
   "bin": {
     "filesearch": "index.js"
   }
 {% endhighlight %}
 
 * Create a file index.js
 
 {% highlight javascript %}
   #! /usr/bin/env node
 
   var applicationArguments = process.argv.splice(2);
   var userInput = applicationArguments[0];
   var command = 'ls -a | grep ' + userInput;
 
   var exec = require('child_process').exec;
   var child = exec(command, function(err, stdout, stderr) {
     console.log(stdout);
   });
 {% endhighlight %}
 
 * Finally, execute *npm link* to create a symlink to the command line application
 
 
#### Example code 

Code used in this article is available in this [repo]: https://github.com/juancancela/jcw-clnt




