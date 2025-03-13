---
title: "NODE.JS under the hood"
date: "2018-10-14"
---

# GLOBAL vs WINDOW

In Javascript, all the function that are global (like setTimeout or Alert) etc are part of a much larger Object called **WINDOW** In NodeJS this **is not the case**, those functions are part of an OBJ called **GLOBAL**

# MODULE WRAPPER FUNCTION

The code inside each module is not executed directly but it is actually wrapped inside a function like this:

```
(function (exports,require,module,__filename,__direname){

//here we put our code!!!

}
```

```
This means that when we do :
```

```
module.exports
```

we are actually working on an object called module, that has an object called exports.  

# JSHINT

a tool that helps to detect errors and potential problems in your JavaScript code. To use instead of node ex:

```
$ node app.js

$ jshint app.js
```

# STRINGS WITHOUT CONCATENATION

You can build string without concatenation with a simple trick: use this :

```
`Result : ${variable}`

` //This character is made with ALTGR and number 7
```

# USE EXTENDS FOR GET MORE OUT OF YOUR CODE:

```
const EventEmmitter = require('events');



class Logger extends EventEmitter {

    this.on("event",callback);

}
```

This is could be useful also for others classes not only events

# REST / RESTful Services

Most of the applications (web) we see today follow the structure "CLIENT <-> SERVER" in order to work. The Client needs to "talk" to the server and the server responds to the client with the information needed. Usually, this communication happens via the HTTP (or HTTPS) protocol, the client can call the server via sending an HTTP request, and here is where REST come in! REST stands for

### _**RE**presentational **S**tate **T**ransfer_

And it's a conventional name for this kind of HTTP SERVICES!! The Server is there for this 4 groups of operations called

### **CRUD OPERATIONS**

CRUD Operations stands for: **Create - Read - Update - Delete**

# API STRUCTURE

Since we are talking about RESTful Apps that for most can actually be considered an API, it is correct also to talk about API conventions!! API Example:

```
http://vidly.com/api/customer



http:// -> This can be http but also https: its up to you



vidly.com -> This is the domain name where our API is stored



/api -> Conventionally the API are under the api folder or under a sub domain api like api.vidly.com



/customer -> With this we are telling that we need to work (CRUD) with the customers section
```

## HTTP METHODS

Since we are using HTTP to execute CRUD operations on the server we should use HTTP methods too : **GET -> TO READ** **POST -> CREATE** **PUT -> UPDATE** **DELETE -> DELETE**

# STORING SECRETS IN ENVIRONMENT VARIABLES

You should know what environment variables are, those are variables that are stored inside the node core! But what you don't know is that you can SET your Env Variables, and this is a good practice for storing password securely!! in windows you can do that using one of those codes:

```
$env:NODE_ENV="production" //This work properly with the powershell (not cmd)



set NODE_ENV=production // This didnt work
```

Or if you using Unix based Os (Mac and Linux) :

```
$export app_password=1234
```

This subject seems a little bit more complex than it looks like... First of all, that variable is stored in  the ram just for the time the shell is up if you reload the shell the variable is gone I need to investigate further for better comprehension of  how to use env variable in a real-world scenario.
