---
title: "NodeJS Code With Mosh (part-2)"
date: "2019-07-18"
---

# Making a RESTFull Service

restful service, or restful APIs are services that respond to a specific input! Normally via HTTP requests! REST is short for **REPRESENTATIONAL STATE of TRANSFER** , I know that this makes no sense (this term was introduced the first time by a Ph.D. Student, as part of his thesis) but it means those kinds of service that responds to demand from the client. So we use a simple HTTP protocol to provide support to Create Read Update Delete operations (CRUD).

## VIDLY.COM RENT A MOVIE

To better understand this concept we are going to create a restful service for our company that rents videos on the web! We need to expose a service and an end point like this:

```
vidly.com/api/customers
```

So the client of our platform can perform an HTTP request to this endpoint and talk to our service. A few things about those endpoint:

- The address can start with HTTP or HTTPS (that depends on the applications and Its requirements HTTPS is secure channel)
- Before we have the Domain and After we have APIs
- You can build an API as a SUBDOMAIN (api.vidly.com)
- We have /customer that in this case refers to the collection of customers, in the REST world this is called a _**RESOURCE,**  so_ all the operations on the clients (CRUD) will be handled with this resource

That said let's talk about kind of HTTP requests, for each of our operations (CRUD), there is a different kind of HTTP request, we have

- GET -> Getting Data
- POST -> Creating Data
- PUT -> Updating Data
- DELETE -> Deleting data

Now let's explore each one of this! Let's just imagine that we want to request the list of costumers to our app, well we need to send an HTTP GET request to our api.vidly.com/customers and the server should respond something like this:

```
//An Array of customers objects

[

  {  id:1, name: "x"},

  { id:2, name:"y"}

]
```

If you want specific Customer information, you should put the customer ID in the address like this:

```
vidly.com/api/customers/1
```

And if you want to UPDATE customer information we should send a PUT HTTP request on this endpoint with the specific ID we want to change and we will pass also the object of the customer with the modifications done! Same thing to DELETE a customer. Of course, if we want to ADD a customer we don't need to send an ID (because we don't have one) we will use a POST HTTP request on the basic API Resource, but we need to include a Customer Object with the information of the new Customer. So Those are the BASICS of a RESTful App, exposing an endpoint with a meaningful and easy address, and support various operations around them such as creating or updating them, using standards HTTP methods. Now we are going to create this kind of Resource, but to do so we are going to use **EXPRESS**, a lightweight framework made to make this operations simpler!

# EXPRESS

Express is (probably) the most famous server handling framework for NodeJS! This is an example of Express works:

```
const express = require('express');

const app = express();

// The HTTP request we can handle with express

// app.get();

// app.post();

// app.put();

// app.delete();

// Use express website to see all the methods



app.get('/',(req,res)=>{

res.send('hello world');

});



app.get('/api/courses', (req,res)=>{

res.send([1,2,3,4]);

});


```

But of course, this is not the all the power of Express One of the most important things in express is the ability to routs requests! So that all the "Courses" requests can be handled via a different file like courses.js, express so is giving us the possibility to have a structure, a skeleton!

## NODEMON Modification on the fly!

```
$ (sudo) npm i -g nodemon
```

This package actually look for modifications in the files we are working on and restart the server when there are some when you have installed it you can use the command :

```
nodemon index.js
```

To make sure the software will look for differences

## APP.LISTEN The Port of HELL

When we "listen" on the port 3000 it is ok when we are in our development environment, but in a real-life scenario is the server where you host the software that tells you which port to use! So we can't rely on the port 3000 to be available! But we can fix this using an **ENVIRONMENT VARIABLE: PORT!!**

```
const PORT = process.env.PORT || 3000

app.listen(3000,()=>{

console.log("Listening on port "+ PORT);

})
```

## ENVIRONMENT VARIABLE

An Environment variable is a variable that is part of the environment where the process runs (so is set externally by the environment) which means that it is not coded inside the software but inside the Operating System! This is useful for storing important passwords instead of storing them inside the code.

## STORING ENV VARIABLES

You can store ENV VAR permanently in your system, or just for the terminal session, we will see in this case just the terminal session **WINDOWS:** Use "set" command:

```
POWERSHELL or CMD

set MYVAR=This is my var
```

Here some more info (permanent install and others) for windows [http://www.dowdandassociates.com/blog/content/howto-set-an-environment-variable-in-windows-command-line-and-registry/](http://www.dowdandassociates.com/blog/content/howto-set-an-environment-variable-in-windows-command-line-and-registry/) **LINUX:** Use Export command

```
export API_KEY="ABDJFdfrpf956irjglkfmgi5kgf"
```

More info: [https://hackernoon.com/how-to-use-environment-variables-keep-your-secret-keys-safe-secure-8b1a7877d69c](https://hackernoon.com/how-to-use-environment-variables-keep-your-secret-keys-safe-secure-8b1a7877d69c)

## ROUTES PARAMETERS

Until now we have seen how to get a list of courses using "/API/courses" but how we can handle a request for a specific course? Passing like a specific ID? : "/API/courses/1"? Well for this kind of situations we have ROUTES! :

```
app.get('/api/courses/:id',(req,res)=>{

    var id= req.params.id; //We access the request id

    res.send(id);

})


```

```
app.get('/api/courses/:year/:month',(req,res)=>{

    var year= req.params.year; //We access the request year

    var month=req.params.month; // We access the request month

    var others=req.query.test; // We can access a query value ("?test=1") that we pass in! 

    res.send(month+"/"+year+" Others: "+others);

})
```

## HTTP STATUS

And of course, if we are using HTTP request for handling all of this we should also use the HTTP status methods!! First a little recap: HTTP is a protocol to send pieces of information via the Internet, it is not the only one of course, but is the most know! The HTTP protocol wants that there are STATUS of the request, if we don't find the thing we are looking for, for example, we usually set the status of the HTTP as 404 => not found!

```
app.get('/api/courses/:id',(req,res)=>{

    if (!!NOT FOUND!!) res.status(404).send('Course Not Found') // We set the status of the request to 404 but also we give a response!

})
```

[https://it.wikipedia.org/wiki/Codici\_di\_stato\_HTTP](https://it.wikipedia.org/wiki/Codici_di_stato_HTTP)  

## HTTP POST REQUESTS:

If we want to CREATE (CRUD) something in our database (even if for the moment there is no DB) we may want to use the POST method!

```
app.post("/api/courses",(req,res)=>{

    const course = {

        id: courses.length+1,

        name: req.body.name // The

    };

    courses.push(course);

    res.send(course); // We respond the same obj that we put in beacause,

                    //  probably the client may need the id to this new obj (if its created by the DataBase)              

})
```

The POST method creates an OBJ that have a BODY on our request (req) that we can access to have parameters! Usually, also, we send back the same obj that we put in the DB cause the client may actually use it, he may need the ID for example, that we dynamically attribute it with our DB. To check the POST methods of our program we can use a plugin of chrome called POSTMAN: [https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop)

## HANDLING ERRORS AND BAD REQUESTS

Now that we can put something inside our code, we must validate the input we receive, and if it is not good we must handle the error! let modify our code like this:

```
app.post("/api/courses",(req,res)=>{

    if(!req.body.name || req.body.name.length <3){

        //400 Bad Request

        res.status(400).send("The Name is required and should be 3+ Char");

        return; //this is because we dont want the rest of the software to be excecuted

    }

    const course = {

        id: courses.length+1,

        name: req.body.name

    };

    courses.push(course);

    res.send(course); // We respond the same obj that we put in beacause,

                    //  probably the client may need the id to this new obj (if its created by the DataBase)              

})
```

This is an example of how we can validate the input, but in a real-world scenario, this is not sufficient, cause there is too much info too validate! But we can solve this with JOI :)

## JOI the joyful  validation npm package

[https://www.npmjs.com/package/joi](https://www.npmjs.com/package/joi) JOI is an npm package that makes validations way easier!! First of all (of course) we have to install it on our machine! (npm i joi)

## MIDDLEWARE FUNCTIONS CONCEPT

A Middleware function is a function that you set in a pipeline of functions, the scenario is simple, you have a pipeline of function that does something on your data, it's like passing data through an array of functions, before ending to the final function! The functions that are in the array are called middleware! EX:

```
app.use(function(req,res,next){

    console.log('Logging...');

    next();

})



app.get('/',(req,res)=>{

    res.send('hello world!!!');

});
```

First of all.. there is no array of functions, so you have to pass a "next" argument (that will be a function but we don't care) and we have to call it before the end of our function... So in order to put a function inside the pipeline, you use the .use method, and you pass the function you want! In this case, we pass a simple function that's log that we are logging (lol). In the end, we call the next function to connect our function to the pipeline of functions! There are different kinds of middleware functions, there are built-in middleware functions, third-party middleware functions that we can set to have a better use of our code! The middleware functions in the pipeline are called in the order we use the .use method! So pay attentions to the order! I you want ( and you should usually do) you can put your middleware function in a different file, or module :

```
// MODULE CODE logger.js

function log(req,res,next){

  console.log('logging...');

}



module.exports = log



// ON THE INDEX.JS FILE

logger=require("./logger.js")

app.use(logger);
```

## THIRD PARTY MIDDLEWARE

On expressjs.com you can easily find a good amount of good middleware that we can use: [https://expressjs.com/en/resources/middleware.html](https://expressjs.com/en/resources/middleware.html) Of course, you don't have to use all of them cause every middleware function you use will slow (a little bit of course) your app, choose wisely. "helmet" is considered one of the best practice, it helps secure your apps by setting different HTTP headers.

##  THE PRODUCTION ENVIRONMENT

This is an important concept in the world of software developing, you may think at the production environment as a stade in theater play: First, you have to reharsal your script on the stage, you don't have costumes yet, no sounds effects, and no light, there is someone near to you on the stage ready to suggest the lines you forget, this is what we can call a "development enviroment", but when you will perform in front of your public those things must be(lights, costumes, sound effects, music) and other must be gone(suggestions) and thats called a "production environment"! In the same way, you can set your production environment to "development" (is just a simple word in a variable at the end) and storing all the configurations in a specific way and when you are ready to step up for "production" the configuration file (with all the code for databases etc) will be different! Of course, you can have more than just 2 kinds of Production Environment, like STAGING, TESTING or others that you can invent yourself for your needs! Here a link that explains how and where use those variables : (https://medium.com/the-node-js-collection/making-your-node-js-work-everywhere-with-environment-variables-2da8cdf6e786)

## USING PRODUCTION ENV FOR DIFFERENTS CONFIGURATIONS

We are going to simplify our life for this and we will use some package already thought for doing so! The most popular one is RC : [https://www.npmjs.com/package/rc](https://www.npmjs.com/package/rc) But I personally prefer config: ([https://www.npmjs.com/package/config](https://www.npmjs.com/package/config)) Config uses the simple idea of the differents production environment to stack up config JSON files a little bit as CSS does. you have a default JSON file where you put all your configuration Infos, then you create differents JSON for each production env. The system will look up for differences in the JSON for the current production env from the default one! it's a pretty neat system!

## USING PRODUCTION ENV WITH EXPRESS

Maybe in our "dev-production", we want to use just some middleware and no others and vice-versa, EXPRESS has a function to let us know in what ENV we are:

```
app.get("env") //Development
```

If an environment is not defined, it will automatically set to development!
