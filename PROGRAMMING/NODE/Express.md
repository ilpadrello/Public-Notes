---
title: "NODEJS - EXPRESS"
date: "2018-11-04"
---

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

# NODEMON Modification on the fly!

```
$ (sudo) npm i -g nodemon
```

This package actually look for modifications in the files we are working on and restart the server when there are some when you have installed it you can use the command :

```
nodemon index.js
```

To make sure the software will look for differences

# APP.LISTEN The Port of HELL

When we "listen" on the port 3000 it is ok when we are in our development environment, but in a real-life scenario is the server where you host the software that tells you which port to use! So we can't rely on the port 3000 to be available! But we can fix this using an **ENVIRONMENT VARIABLE: PORT!!**

```
const PORT = process.env.PORT || 3000

app.listen(3000,()=>{

console.log("Listening on port "+ PORT);

})
```

# ENVIRONMENT VARIABLE

An Environment variable is a variable that is part of the environment where the process runs (so is set externally by the environment)

# ROUTES PARAMETERS

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

# HTTP STATUS

And of course, if we are using HTTP request for handling all of this we should also use the HTTP status methods!! First a little recap: HTTP is a protocol to send pieces of information via the Internet, it is not the only one of course, but is the most know! The HTTP protocol wants that there are STATUS of the request, if we don't find the thing we are looking for, for example, we usually set the status of the HTTP as 404 => not found!

```
app.get('/api/courses/:id',(req,res)=>{

    if (!!NOT FOUND!!) res.status(404).send('Course Not Found') // We set the status of the request to 404 but also we give a response!

})
```

# HTTP POST REQUESTS:

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

The POST method create an OBJ that have a BODY on our request (req) that we can access to have parameters! Usually, also, we send back the same obj that we put in the DB cause the client may actually use it, he may need the ID for example, that we dynamically attribute it with our DB. To check the POST methods of our program we can use a plugin of chrome called POSTMAN: [https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop)

# HANDLING ERRORS AND BAD REQUESTS

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

# JOI the joyful  validation npm package

[https://www.npmjs.com/package/joi](https://www.npmjs.com/package/joi) JOI is an npm package that makes validations way easier!! First of all (of course) we have to install it on our machine! (npm ijoi)

# MIDDLEWARE FUNCTIONS CONCEPT

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

# THIRD PARTY MIDDLEWARE

On expressjs.com you can easily find a good amount of good middleware that we can use: [https://expressjs.com/en/resources/middleware.html](https://expressjs.com/en/resources/middleware.html) Of course, you don't have to use all of them cause every middleware function you use will slow (a little bit of course) your app, choose wisely. "helmet" is considered one of the best practice, it helps secure your apps by setting different HTTP headers.

STORING SECRETS IN ENV VARIABLES
