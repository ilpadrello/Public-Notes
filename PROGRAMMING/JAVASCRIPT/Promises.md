---
title: "Javascript Promises"
date: "2018-10-17"
tags: 
  - "javascript"
  - "js"
  - "promises"
---

FAST CODE:

```
var readFile= function (path,type){
  return newPromise((resolve,reject)=>{ //I KNOW IS CONFUSING BUT THIS IS THE CODE TO MAKE THE .THEN 
                                         // CHAIN POSSIBLE, EVERY FUNCTION IN THE CHAIN SHOULD RETURN A PROMISE 
    fs.readFile(path,type,(err,data)=>{
      if(err){
        reject(err);
      }else{
        resolve(data);
      }
    })
  })
}

//CHAIN THE FUNCTIONS WITH .THEN
readFile("index.html","utf8").then( data => {
  console.log(data);
}).catch(err=>{
  console.log(err);
})

//PASSING ALL THE FUNCTIONS AS AN ARRAY
Promise.all([
  readFile("index.html","utf8"),
  readFile("test.html","utf8")
]).then(datas=>{
  datas.forEach(element=> {
    console.log(element);
  });
}).catch(err=>{
  console.log(err);
})
```

A Promise in short:

"Imagine you are a **kid**. Your mom **promises** you that she'll get you a **new phone** next week." You _don't know_ if you will get that phone until next week. Your mom can either _really buy_ you a brand new phone, or _stand you up_ and withhold the phone if she is not happy :(

Let's convert this to JavaScript:

## CREATING THE PROMISE

```
 /_ ES5 _/
 var isMomHappy = false;

// Promise
 var willIGetNewPhone = new Promise( 
   function (resolve, reject) {
     if (isMomHappy) {
       var phone = {
          brand: 'Samsung',
          color: 'black'
       };
       resolve(phone); // fulfilled
     } else {
       var reason = new Error('mom is not happy');
       reject(reason); // reject
     }
   }
 );
```

In every promise, you must pass a function with 2 parameters that are 2 functions also (resolve, rejects) those functions are called if the promise is fulfilled (resolve) or not (rejects), even if it looks like you have to set those callback functions up, **this is not the case!!! And this is very important to understand how to USE promises**

But before that, we remember how to read file asynchronously:

```
const fs = require("fs");
fs.readFile("file.txt",'utf8',(err,content)=>{ //You pass a function for when the file is written
  console.log(content);
});
```

So, the readFile functions take 2 parameters and a callback function called when the reading of the file is done... The problem here is that if you want to chain others functions after the file is read (maybe because in the file you have the name of another file you want to read) you have to do so:

```
const fs = require("fs");
fs.readFile("file.txt",'utf8',(err,content)=>{ //You pass a function for when the file is written
  fs.readFile(content,'utf8',(err,data)=>{
    console.log(data);
  })
});

OR

function readOtherFile (path,type){
  fs.readFile(path,type,(err,data)=>{
    console.log(data);
  })
}
fs.readFile("file.txt",'utf8',(err,content)=>{ //You pass a function for when the file is written
  readOtherFile(content,'utf8')
});
```

And that's what you have to do to chain JUST 2 FUNCTIONS!! Imagine what it takes to chain 10 Asynchronous functions!

That's why we use promises, with promises the chain is far easier than this system

Here's are some Example on how to use promises :

```
var readFile= function (path,type){
  return newPromise((resolve,reject)=>{ //I KNOW IS CONFUSING BUT THIS IS THE CODE TO MAKE THE .THEN 
                                          // CHAIN POSSIBLE, EVERY FUNCTION IN THE CHAIN SHOULD RETURN A PROMISE 
    fs.readFile(path,type,(err,data)=>{
      if(err){
        reject(err);
      }else{
        resolve(data);
      }
    })
  })
}

//CHAIN THE FUNCTIONS WITH .THEN
readFile("index.html","utf8").then( data => {
  console.log(data);
}).catch(err=>{
  console.log(err);
})
```

This time we didn't use the Promise immediately, because this would have started the function you pass inside. Instead, we put the Promise inside a function and we return it! So the function inside the promise will be called only when we call the main function! This concept also is very important, because using this technique (_return new Promise_) we will be able to use the method ".then" after the function call (readFile(...).then(...)).

So at this point, we need to understand how to use "resolve" and "reject". **THOSE ARE NOT FUNCTIONS THAT YOU HAVE TO FILL WITH YOUR FUNCTIONS!!!**

Those are more like callback functions that connect you with the two most important methods : ".then" and ".catch" **Actually what you put in "resolve()" will go in ".then" and what you put in "reject()" will go in "catch()".** 

But PAY ATTENTION, inside ".then" and ".catch" you don't put VALUES but FUNCTIONS!! You take the value passed by "resolve()" and you put into a function inside the ".then" that will be called immediately!

So, if now, inside the ".then" function we put a function that returns a new Promise we can chain them like this :

```
readFile("text1.txt","utf8").then( data=>{
    console.log(data);
    readFile("text2.txt","utf8").then(data => { //Using the same function im returning a Promise class object
        console.log(data);
    }).catch(err=>{
        console.error(err);
    })
}).catch(err=>{
    console.error(err);
});

//THIS WILL RETURN THE TEXT OF TEXT1.TXT AND AFTER TEXT2.TXT
```

Since we are using the same function that returns a Promise Class we can chain ".then" methods but we can also do this:

```
readFile("text1.txt","utf8").then( data => {
    console.log(data);
    return new Promise((resolve,reject)=>{ // To this .then method we return a Promise Obj
        if(data!=undefined){
            resolve("yeah");
        }else{
            reject("oh nooo");
        }
    })
}).then(data=>{  //Since we return a Promise obj we can chain the .then method here
    console.log(data);
})

//This will output :
the inside of the text1.txt
yeah
```

As you can see we chained the ".then" method with a ".then" since we return a Promise object to the first ".then".

But there is even a better way to chain functions using the method ".all" of Promise : (See comments for better understanding)

```
var readFile= function (path,type){
    return new Promise((resolve,reject)=>{
        fs.readFile(path,type,(err,data)=>{
            if(err){
                reject(err);
            }else{
                resolve(data);
            }
        })
    })
}

Promise.all([                     // You want to fill up an Array with the functions you want to chain (better if they have a return Promise)
    readFile("text1","utf8"),
    readFile("text2","utf8")
]).then(datas=>{                 // You will recive a SINGLE one .then method that contains an array with all resolve() results
    datas.forEach(element => {   // Use for each to see all the elements
        console.log(element);
    });
}).catch(err=>{                  // Une single .catch for all
    console.log(err);
})
```

This is not perfect for all the situations, you may want to chain different functions depending on the response of the first function, in that case the other methods up listed are perfect

And just for fun:

```
Promise.all([
    new Promise((resolve,reject)=>{
        resolve("Hello World!!!");
    }),
    readFile("text1.txt","utf8"),
    readFile("text2.txt","utf8")
]).then(datas=>{
    datas.forEach(element => {
        console.log(element);
    });
}).catch(err=>{
    console.log(err);
})

//Will Return: 
Hello World!!!
text1
text2
```

# Concatenations of the functions

Ok!! WE CONCATENATE EVERYTHING!!! Let's imagine that we want to concatenate the results of different promises, you may think you want to do something like this:

```
function chiamaAPI(testo){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
                    resolve ({id:1});
                },2000)
    });
}

function secondaApi(user){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            user["name"]="Carlo"
            console.log("Seconda Api +set timeout : "+user)
            resolve(user);
        },2000)
    })
}

function terzaApi(user){
    console.log("user é"+user) //user = undefined
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            user["username"]="pesce spada"; //Error user is undefined
            resolve(user);
        },2000)
    });
}

 chiamaAPI("Carlo")
 .then((user)=>{secondaApi(user)})
 .then((user)=>{terzaApi(user)})
 .then((user)=>{console.log(user)})
```

This code will actually make an error! In terzaApi, USER IS UNDEFINED!!!!

Here is how the code should be :

```
chiamaAPI("Carlo")
 .then(secondaApi)
 .then(terzaApi)
 .then((ruser)=>{console.log(ruser)})
```

```
 chiamaAPI("Carlo")
 .then((user)=>{
    secondaApi(user).then((user)=>{terzaApi(user)}) 
    .then((user)=>{console.log(user)})
})
 
 .then((user)=>{console.log(user)})
```

But why??? I struggled a little bit with this problem but I think I found the solution!! First of all let expermient with this:

```
function chiamaApi(){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            console.log(new Date)
            resolve();
        },2000)
    })
}

function secondaApi(){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            console.log(2,new Date)
            resolve();
        },2000)
    })
}

function terzaApi(){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            console.log(3,new Date)
            resolve();
        },2000)
    })
}

function quartaApi(){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            console.log(4,new Date)
            resolve();
        },2000)
    })
}

chiamaApi("Carlo")
.then(result=>{secondaApi()})
.then(result=>{terzaApi()})
.then(result=>{quartaApi()});

// 2018-11-11T10:47:20.590Z
// 2 2018-11-11T10:47:22.597Z
// 3 2018-11-11T10:47:22.600Z
// 4 2018-11-11T10:47:22.600Z
```

As the result is showing the 2 3 4 .then functions are called altogether after the first one! So we can call differently .then functions after the first promise has been fulfilled? Let's try what happens if we pass a common variable between them : 

```
function chiamaApi(){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            console.log(new Date)
            resolve("test");
        },2000)
    })
}

function secondaApi(test){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            console.log(test)
            console.log(2,new Date)
            resolve();
        },2000)
    })
}

function terzaApi(test){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            console.log(test)
            console.log(3,new Date)
            resolve();
        },2000)
    })
}

function quartaApi(test){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            console.log(test)
            console.log(4,new Date)
            resolve();
        },2000)
    })
}

chiamaApi("Carlo")
.then(result=>{secondaApi(result)})
.then(result=>{terzaApi(result)})
.then(result=>{quartaApi(result)});

2018-11-11T10:53:34.915Z
test
2 2018-11-11T10:53:36.922Z
undefined
3 2018-11-11T10:53:36.925Z
undefined
4 2018-11-11T10:53:36.926Z
```

As you can see only the first .then (secondApi) receive the variable, the other 2 don't receive anything... but WHY?? Well!! When you pass a function with the parenthesis () it is executed immediately! And that's why you just need to remove everything but the name of the function you want to pass in order to make it function sequentially, event the result variable!

```
function chiamaApi(){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            console.log(new Date)
            resolve("test");
        },2000)
    })
}

function secondaApi(test){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            console.log(test)
            console.log(2,new Date)
            resolve("test 2");
        },2000)
    })
}

function terzaApi(test){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            console.log(test)
            console.log(3,new Date)
            resolve("test 3");
        },2000)
    })
}

function quartaApi(test){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            console.log(test)
            console.log(4,new Date)
            resolve("test 4");
        },2000)
    })
}

chiamaApi("Carlo")
.then(secondaApi)
.then(terzaApi)
.then(quartaApi);

test
2 2018-11-11T11:01:35.253Z
test 2
3 2018-11-11T11:01:37.255Z
test 3
4 2018-11-11T11:01:39.256Z
```

That's because when you pass the function with no parentheses, it connects directly with the resolve of the promise, so its the resolve promise that calls that function and you don't call it inside .then method! A little bit tricky but this is how it works!
