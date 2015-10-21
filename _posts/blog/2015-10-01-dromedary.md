---
layout: post
title: "Introducing Dromedary, a minimalistic, lightweight API decorator"
date: 2015-10-01 16:54:46
author: Juan Carlos Cancela
categories: 
- blog
- javascript
- integration
- nodeJS
img: dromedary_grey.jpg
thumb: code.jpg
---

A time ago, during a side project I worked for, I was faced with the need to integrate a couple of services. 
All in all, it was needed to consume some HTTP services, apply some transformations and establish a clear routing 
through them.
Since client was already using a Java technology stack, we end up using [Apache Camel](http://camel.apache.org/) to get 
the job done.
Anyway, I was particularly interested to see if it could be a way to achieve not so complex integrations using non Java 
technologies. In particular, I wanted to try if using [Express Middleware](http://expressjs.com/es/guide/using-middleware.html), plus some configuration-driven architecture could eventually 
enable a potential generic solution for such use cases.



####10000 feet view

This is more or less the way I imagine it

![Screenshot](https://dl.dropboxusercontent.com/u/3868882/juancancela.work.io/posts/2015-10-01-dromedary/dromedary.png){: .center-image }


So the case is an existing HTTP service (maybe I wouldn't limit wrapped application to be an HTTP service, who knows?) that is directly consumed by some clients.
Owner of HTTP service wants to decorate its existing API adding things like security, third party API consumption, or applying certain modifications to incoming requests.
Dromedary would act as a proxy service that will handle every single request to the API, and apply an ordered list of configurable plugins before letting request reach existing service.
Additionally, once HTTP response comes back from it, it would apply any plugin configured to be executed after service invocation.


####Implementation Wish list

Next, lets craft a wish list of characteristics that could be made in order to make library implementation simpler and somehow different compared to other options. So...

* Solution must be configuration driven: Ideally, through a config file it would be possible to declare and configure the plugins that compose the gateway.
* Plugin based architecture: Library must provide a way to consume plugins that can accomplish different features. In example, it would exist a plugin that can be declared through the configuration file, that enables transforming some values in an HTTP response. 
* Lets be clear again about first wish: Integration is a config file. Not a Node app, a Javascript file, or something else. Just a plain file that can be pass to a command line tool that takes care of correctly process it.
* This config files would be composable. So, a it would be able to consume another config file. Ideally, a much complex integration could be split in several config files, each of them abstracting integration details.



![Screenshot](https://dl.dropboxusercontent.com/u/3868882/juancancela.work.io/posts/2015-10-01-dromedary/dromedary-2.png){: .center-image }


####Dromedary alpha version

Taking as input the use cases plus implementation wishes detailed above, I have developed a prototype of the library to start trying and iterating through it. Library is registered in NPM registry [here](https://www.npmjs.com/package/dromedary), and code is versioned [here](https://github.com/randiantech/dromedary).


####Next
On second part (soon!) I'll take a closer look at proof of concept implementation, and show some real cases where it could actually be quite useful.
