---
title: "NodeJS Code With Mosh (part-1)"
date: "2019-05-07"
---

In this page ill take notes from the course of NodeJs done by CodeWithMosh (https://codewithmosh.com/courses/enrolled/293204)

# WHAT IS NODE?

NodeJs is a tool to use javascript outside the web, its written in C++ and lets you build server-side applications, like webapp or mobile apps or API It's made for large scale, data intensive, real time web-apps! Node is specially good for asynchronous applications even if that means getting a little dirty in the way we structure a software

# THE MODULE SYSTEM

(experimenting with the module system: [http://www.simonepanebianco.fr/nodejs-experimenting-with-modules/](http://www.simonepanebianco.fr/nodejs-experimenting-with-modules/)) The module system is the best way to organize your code, you can easily import code from another module and use it without worrying about contrasting var declaration, or function declaration because each module is airtight with the rest of the application, it's a little bit like classes in Java, but in a different way

#### THE GLOBAL OBJECT

The global object is the object that contains all the functions and variables of the code in that page, every standard function (like setTimeout, or others) and variable belongs to the **global** object, like the same happens in the web pages with the window object (global.setTimeout())

#### THE MODULE SCOPE

```
Every module is a file, and the scope of that file will rest inside that file, it means that only the things that we decide to export (with module.export) will be exported!
```

#### THE MODULE OBJECT

Module is actually an object of a superior level of a function and if you don't put anything inside this object it will be passed as empty! So you can add everything you want into this object, variables, functions, objects, classes here some examples

#### PASSING VARIABLES

```
var multi = 10;

module.exports.multi = multi;



//{multi:10}
```

#### PASSING FUNCTIONS

```
function foo(num) {

 return num;

}



module.exports.foo = foo;

//{ foo: [Function: foo] }
```

Or even better:

```
function foo(num) {

 return num;

}



module.exports = foo; <- here is the difference
```

in this way, we transform the "module.export" object in a function and we can call it directly!

```
var foo = require("./personal_modules/functions");

foo(13);
```

#### PASSING OBJECTS

```
var users = {

 num: 10,

 foo: function(num) {

 return num;

 }

};

module.exports = users;
```

This method can be tricky because when you passing an object like this (note the "module.exports=") and you modify something in the object this will be modified also in the memory so if some one wants to access the same module this will be modified!

#### PASSING CLASS

 

### REQUIRE

when you use require you are actually passing the object of the module not only what is inside : here is an example

```
//multiplayer.js

var multi = 10;

module.exports.multi = multi;



//Index.js

var multi = require("./personal_modules/multiplayer");

console.log(multi); //{ multi: 10 }
```

As you can see here when we use the require function we are not passing just the variable but the entire object, if we wanted to use the variable inside this object we should do:

```
//index.js

var multi = require("./personal_modules/multiplayer");

console.log(multi.multi); //10
```

Or

```
var multi = require("./personal_modules/multiplayer").multi;

console.log(multi); //10
```

In this way we can use the variables

# RESTFUL CRUD API

One of the best ways to use Node is to create a series of API, instructions that we can use to build our software, to do so we need to manage all the HTTP request that the client side sends us.

### GET POST PUT DELETE (CRUD : CREATE READ UPDATE DELETE)

In order work with HTTP request we must know the basics of it, we have 4+ type of request to do the basic operations in the server (CRUD Create Read Update and Delete), GET - > READ this is used to GET the info that we need from the server POST -> This is use CREATE something in the server (database or other) PUT -> This is used for UPDATE the data in the server DELETE -> Well... Delete from the server To be able to respond to all of this requests Nodejs must start a server, we can use the HTTP module that node already have or.. better use a very well know framework for handling servers that is know as **EXPRESS.js**

# THE EVENT SYSTEM

NodeJS is an Event-Based framework! This means that understanding how the event system work and what are the best way to implement it is very important, but let start with basic! **What is an Event?** Well an Event is just a "signal" that is raised when "something" happens in our application! For example in node we can put a "Listener" for an http request in a port, and each time there is a request on that port we "signal" o better we **raise** an event, and we can respond to that event!

## THE EVENT MODULE

So if we go in the Node Js documentation we find an Event Module: [https://nodejs.org/dist/latest-v10.x/docs/api/events.html](https://nodejs.org/dist/latest-v10.x/docs/api/events.html) In this module we can find that we have a class called EventEmitter. A lot of classes are based on this EventEmitter! So lets see how we can work with this event emitter

## Event Emitter Class

```
//Let's require the events class

const EventEmitter = require("events");

// Now we create an instance of the class

const emitter = new EventEmitter();


```

The EventEmitter class have a lot of methods, but what we are interested in the most are : "_emit_" and "_on_"

```
//This will raise an event with id="messageLogged"

emitter.emit("messageLogged");
```

This will raise an event with id="messageLogged" but just emitting an event doesn't do a lot! We need to prepare our ear (with a listener) to listen to that event!

```
//The method "on" will listen to an event called "messageLogged" and will run a function when this happens!

emitter.on("messageLogged", () => {

  console.log("message logged");

});
```

The method "on" will listen to an event called "messageLogged" and will run a function when this happens! Its also important to put this code before the emit event, or our ears will not be ready to receive the event!

```
//Let's require the events class

const EventEmitter = require("events");

// Now we create an instance of the class

const emitter = new EventEmitter();



//The method "on" will listen to an event called "messageLogged" and will run a function when this happens!

emitter.on("messageLogged", () => {

  console.log("message logged");

});



//This will raise an event with id="messageLogged"

emitter.emit("messageLogged");
```

That's how a basic event system works!

### LET'S DIVE A LITTLE DEEPER!

## EVENT ARGUMENTS

Normally when you raise an event you want to send also some data! To do so we can pass the data as arguments on the  "emit" method:

```
//The method "on" will listen to an event called "messageLogged" and will run a function when this happens!

emitter.on("messageLogged", (id, text) => {

  console.log("message logged", id, text);

});



//This will raise an event with id="messageLogged"

emitter.emit("messageLogged", 1, "ciao mamma");
```

Here we have passed 2 data in the emit method: 1 and "ciao mamma"! And in the "on" method we passed in the function 2 arguments: id and text! This is useful, ok, but **its not the best way to pass data**! Let's see how we can get better:

```
//The method "on" will listen to an event called "messageLogged" and will run a function when this happens!

emitter.on("messageLogged", arg => {

  console.log("message logged", arg.id, arg.text);

});



//This will raise an event with id="messageLogged"

emitter.emit("messageLogged", { id: 1, text: "ciao mamma" });
```

This is a better way to pass data, enclosure to a literally obj! This object that we pass to the event listener is often called **"event argument"** but you can call it as you want, common names are: "_arg, event, e, eventArg..._"

## EXTEND EVENTEMITTER CLASS

Is quite rare that you want to work directly with this EventEmitter class, instead normally it is preferable to EXTEND the class, and then use that class in our code, and that is because of the fact that two different instances of the same class, in two separate modules, will not work as one event queue! Those are two different queue of events and so an event raised on one module will not work on a listener that is in another module. Let's imagine that we want to create a logging module (not log-in)

```
//log.js



function log(message) {

  console.log(message);

}



module.export = log;



//App.js

const log = require("./log.js");

log("message");
```

Now we want to raise an Event when logging something! So, first of all, let's make a class out of the log function

```
class Log{

    log(message) {

        console.log(message);

      }

}

module.export = Log;
```

And now we need to Extend the class Log as a child of the class EventEmitter to have all his methods:

```
class Log extends EventEmitter{
```

Now we use the EventEmitter capacity to raise an event:

```
//log.js Module

const EventEmitter = require("events");



class Log extends EventEmitter {

  log(message) {

    console.log(message);

    this.emit("messagelogged", { id: 1, text: "some text" });

  }

}



module.export = Log;
```

**Please note that:** 

- We required the class "events" => EventEmitter to be capable of extending it on the Log Class
- Classes always start with a capital letter (EventEmitter, Log)
- Inside the class Log, we are able to use "this" to refer to the methods of the parent Class (EventEmitter)

So now we can work out in the app.js file:

```
//This will import our class that extends from "events"

const Log = require("./log.js");

//Now we need to create an object out of this class

const log = new Log();

//Now to log we use directly this class so:

log.log("message");
```

Good but still we need to listen to this event! So we need to add a listener:

```
const Log = require("./log.js");

const log = new Log();



//We can use directly the "on" method on log of course!

log.on("messagelogged", arg => {

  console.log("listener called", arg);

});



log.log("message");
```

This will raise however a problem, you need to know what is the "id" of the event raised, that is inside the log.js module, but we will discuss this later!

# THE HTTP MODULE:

Another building block of Node is the HTTP module, that is very useful for creating networking applications, we can easily create a web server that listens for HTTP request on a specific port, and so we can create a backend server for our client application, like a web app that we can build with React or Vue, or mobile app on a mobile device! As for the event module here is the link to the documentation: ([https://nodejs.org/dist/latest-v10.x/docs/api/http.html](https://nodejs.org/dist/latest-v10.x/docs/api/http.html)) lest load the HTTP module:   The HTTP module is actually a module that Extends (in a way) from the "events" module, so he has all the capacity and the methods of the "events" module and we can use the "on" method!

```
const http = require("http");

const server = http.createServer();



server.on("connection", soket => {

  console.log("new connection", soket);

});



server.listen(3000);



console.log("listening on port 3000");
```

In normal software, you usually don't respond to a connection event to build an HTTP server! So let's modify the code:

```
const http = require("http");

//This will take a request a parameter and not a socket!

const server = http.createServer((req, res) => {

  if (req.url === "/") {

    res.write("Hello World");

    res.end();

  }

});

server.listen(3000);

console.log("listening on port 3000");
```

As you can see we have deleted the "server.on" part, and we work directly on the "createserver" method In fact when someone tries to connect to our site he normally sends an HTTP request that we pass in the function "createserver",  and the response  too (cause he has some functions/methods we can use to respond). To a better understanding of those (req,res) link to the documentation ([https://nodejs.org/dist/latest-v10.x/docs/api/http.html](https://nodejs.org/dist/latest-v10.x/docs/api/http.html)) If we want to create a backend service for the web o for mobile app, we need to handle differents requests and specifically differents routes like :

```
req.url === "/api/courses"
```

# NPM: NODE PACKAGE MANAGER

in this section we are going to see the NPM Node Package Manager, that is basically a command-line tool as well as a third-party library, that we can add in our node application. Pretty much any kind of functionality you want to add to your application, there is most likely a free, open-sourced library or package or node module, whatever you want to call it on NPM registry. go on [npmjs.com](http://npmjs.com) to see how many packages for javascript are there! Those packages could be seen as a building block for your application, and of course, you can create your own module!

## NPM Command Tool

To use the NPM you must have NPM installed on your machine, usually if already installed with node, so you don't need to install another tool Remember that is a command-line tool so you need to open your terminal! Try this to see what version of NPM you have:

```
npm -v
```

Mine is 6.4.1

## Package.json

In order to work with NPM, you need to create a file called **Package.json** it's a JSON file that contains some basic information about your application, such as its name, it's version, the adress of its git repository, etc. All node application, by standard, have this Package.json file! An easy way to create this file is to run:

```
npm init
```

This will walk you through creating a package.json file, step by step! but if you want to skip this, and go faster (even if it is not a good idea) you can you the flag --yes:

```
npm init --yes
```

## INSTALLING A NODE PACKAGE

Now that we have our package.son, we can install libraries to our project! We can install a popular javascript module called "\_" (underscore), a tool that upgrades the capacity of javascript without touching the core! First, we can search for an "underscore" package on the npm website ([https://www.npmjs.com/package/underscore](https://www.npmjs.com/package/underscore)), in there we can see that the instruction to install this package are :

```
npm i underscore //i stand for install
```

When we run this a few things happen: In our package.json, we can see that under "dependencies" now we have **"underscore"**, so in our package.json we specify all the dependencies of our product and their version! Those modules are installed in our project folder, inside a folder called **"node\_module"**, and for "underscore" you can see there will be a folder called "underscore". But it is not the end! Inside this folder, there is another package.json file!!! So every node module have their own package.json that contains information about the module! In previous versions of node, the proper way to install a module (let's take the case of "underscore") was:

```
npm i underscore --save
```

without the flag --save you wouldn't get underscore in the list of dependencies, but this behavior is changed in the recent versions of NPM, so we no longer need to use the --save flag, we just run "npm i" (or install) and this will update the package.json and download the latest version of that module! Now that we have the module installed in our project we can use it! In our index.js (or elsewhere) we can now do this:

```
const _ = require('underscore'); //For this package is typical to use a real underscore to store the package
```

For this package is typical to use a real underscore to store the package, and as you can see we didn't need to specify a folder for our module because Node already know how to look for!   -G Flag is for Global Package, package that you use for more than one project, they are installed globally on your machine, and you can use all the same command for those package too!

```
// Install a package

npm i <packageName>

// Install a specific version of a package 

npm i <packageName>@<version>

// Install a package as a development dependency

npm i <packageName> —save-dev

// Uninstall a package

npm un <packageName>

// List installed packages 

npm list —depth=0

// View outdated packages

npm outdated

// Update packages 

npm update 

//-To install/uninstall packages globally, use -g flag.
```
