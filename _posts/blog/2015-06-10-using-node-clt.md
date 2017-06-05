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

Let's create a simple CLT app that to send mails using command line:
 
 * Create a directory ´cltool´
 * Run ´npm init´ to create a package.json file
 * Replace content of package.json with following data:
 
 {% highlight javascript %}
   {
     "name": "heyhey",
     "version": "1.0.0",
     "description": "sends a mail. Usage: 'heyhey <mail@mail.com> <subject> <message>'",
     "scripts": {
       "test": "echo \"Error: no test specified\" && exit 1"
     },
     "author": "Juan Carlos Cancela",
     "license": "MIT",
     "preferGlobal": true,
     "bin": {
       "heyhey": "heyhey.js"
     },
     "dependencies": {
       "nodemailer": "*"
     }
   }
 {% endhighlight %}
 
 In **name**, it is defined the name of the command line tool
 
 In **bin**, it is established the file to be executed when command is invoked
 
 * Create a file heyhey.js
 
 {% highlight javascript %}
   #! /usr/bin/env node

   var userArgs = process.argv.splice(2);
   var to = userArgs[0];
   var subject = userArgs[1];
   var message = userArgs[2];

   var mailOptions = { from: 'cancela.juancarlos@gmail.com', to: to, subject: subject, text: message, html: '<b>'+message+'</b>'};
   var transporter = require('nodemailer').createTransport({ service: 'Gmail', auth: { user: 'cancela.juancarlos@gmail.com', pass: 'XXXXX'}});

   transporter.sendMail(mailOptions, function(error, info){
      error ? console.log(error) : console.log('Message sent: ' + info.response);}
   );
 {% endhighlight %}
 
 In **1** it is defined node to run the script
 
 In **2** user provided arguments are obtained
 
 In **3** is created the command to be executed (using user provided args)
 
 In **4** we execute the given command
 
 
 * Finally, execute *npm link* to create a symlink to the command line application
 

#### Using CLT

From command line, try executing the recently created command; in example; ´heyhey "mail@gmail.com" "hi!" "hey how are you!"´
 
 
#### Example code 

Code used in this article is available in this [repo](https://github.com/juancancela/jcw-clnt)




