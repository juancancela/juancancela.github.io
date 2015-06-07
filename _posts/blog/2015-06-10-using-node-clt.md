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
 
 In ##name##, it is defined the name of the command line tool
 
 In ##bin##, it is established the file to be executed when command is invoked
 
 * Create a file index.js
 
 {% highlight javascript %}
   #! /usr/bin/env node  //1
 
   var applicationArguments = process.argv.splice(2); //2
   var userInput = applicationArguments[0];
   var command = 'ls -a | grep ' + userInput; //3
 
   var exec = require('child_process').exec; //4
   var child = exec(command, function(err, stdout, stderr) { 
     console.log(stdout);
   });
 {% endhighlight %}
 
 In ####1 it is defined node to run the script
 
 In ####2 user provided arguments are obtained
 
 In ####3 is created the command to be executed (using user provided args)
 
 In ####4 we execute the given command
 
 
 * Finally, execute *npm link* to create a symlink to the command line application
 

#### Using CLT

From command line, try executing the recently created command; in example; ´cltool package.json´
 
 
#### Example code 

Code used in this article is available in this [repo](https://github.com/juancancela/jcw-clnt)




