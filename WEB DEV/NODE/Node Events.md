---
title: "NodeJs Events"
date: "2018-10-17"
---

One of the core concepts in NodeJS is the concept of EVENTS, A lot of core functionalities of Nodejs are based on events, an event is basically a signal that something has happened in our application that some listeners receive. This is very very important for example when you use the HTTP class of Node, every time, for example, the class receive a request on a port it raises an event. There is a module especially  for events called "Events", that have a class called EventEmitter! Its one of the core building class of Node and a lot of classes are based on this EventEmitter. Here is an example of how the emitter class works:

```
const EventEmitter=require("events");

const emitter = new EventEmitter(); // EventEmitter is a class so a objet must be create



emitter.on("saluto",()=>{console.log("hello wolrd")}); //you can also use "emitter.addListener" its the same

emitter.emit("saluto");
```
