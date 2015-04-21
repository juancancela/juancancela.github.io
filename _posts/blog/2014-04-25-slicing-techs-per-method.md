---
layout: post
title: "Slicing technologies per method"
date: 2014-04-25 16:54:46
author: Juan Carlos Cancela
categories: 
- blog 
- architecture
- experiences
img: 2014-04-25-slicing-techs-per-method.png
thumb: arch.jpg
---

During a project I have worked for, as dev team we were confronted with the need to scale an already existing API, 
trying to minimize changes to its core. 
Current state could be summarized as follows:

* API was developed using a typical Java - Spring - Hibernate stack
* MongoDB was used as persistence store
* Service was slow due to the way serialization/deserialization of objects was implemented

As mentioned before, modifying core to improve serialization/deserialization was not an option. 

After talking with product owner, we realized that due to the nature of required feature, <b>we could expect way more
readings</b>, but no -or minimal- growth on <b>non safe, non idempotent</b> operations.

We could safely assert that we needed to scale API in order to support newly large amount of GET operations. 
 
 
####Scaling GET

Actually, read operations were quite simple to handle: Just creating a dynamic predicate, getting matching documents 
from MongoDB, and sending them back on response. Problem was that the already mentioned serialization/deserialization
process was involved in current service implementation.   

![Screenshot](https://dl.dropboxusercontent.com/u/3868882/juancancela.work.io/posts/2014-04-25-slicing-techs-per-method/img1.png){: .center-image }

To tackle that, we initially tried to find a way to overpass ser/des calls, but due to the way service was 
implemented, that was not possible.

####Routing GET to a new service

So finally we decided to implement a new service, exclusive for GET calls to API. To do that, we implemented a service
using NodeJS. Since NodeJS is Javascript and MongoDB persists docs as BSON, there was no need of additional ser/des 
process. We just needed to adjust reverse proxy in order to correctly route API calls to the proper implementation.
In this way, we were able to:

* Successfully scale service as expected
* Do not change current legacy service, as requested
* All changes were 100% transparent for API users

![Screenshot](https://dl.dropboxusercontent.com/u/3868882/juancancela.work.io/posts/2014-04-25-slicing-techs-per-method/img2.png){: .center-image }

####Generalizing concept

After this experience, I found that in some scenarios (as the one described in this post) picking different
technologies per API method could make sense. In example, using a class based, compiled language as Java could be
eventually more convenient in some routes of API, and lightweight, easier to implement Javascript code could be used on
others.



