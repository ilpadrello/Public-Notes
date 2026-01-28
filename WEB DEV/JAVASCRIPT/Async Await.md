---
title: "JAVASCRIPT Async and Await"
date: "2018-11-11"
tags: 
  - "async"
  - "await"
  - "javascript"
  - "js"
---

# ASYNC and AWAIT

"Async and await" helps you write asynchronous code like synchronous code :)

every time you have a function that returns a Promis you can "await" that result

```
let result = await functionWithPromise() // Thanks to await the result of the function with the promise will
                                         // be putted inside the function when the result get back
```

but where is ASYNC ??

First of all, the Javascript Engine requires that you use a function that is decorated with Async so :

```
async function test(){
    let user= await chiamaAPI(id);
    console.log(user);
}
```

Let's try to make the same algorithm that we did with the promises

```
function chiamaAPI(testo){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
                    //console.log("Questo é un possibile call back di una chiamata di una api con "+testo);
                    resolve ({id:1});
                },2000)
    });
}

function secondaApi(user){
    console.log("seconda Api user = "+user.id)
    return new Promise((resolve,reject)=>{
        //console.log("Since user id is "+user.id+" we can look for its name on db");
        setTimeout(()=>{
            //console.log("we found a name that correspond to this id:"+user.id +" and we pass it ");
            user["name"]="Carlo"
            console.log("Seconda Api +set timeout : "+user)
            resolve(user);
        },2000)
    })
}


function terzaApi(user){
    console.log("user é"+user)
    return new Promise((resolve,reject)=>{
        //console.log("now we do know its name : "+user.name+" let's find out its username");
        setTimeout(()=>{
            //console.log ("We found a name that correspond to "+user.name+" and we pass the usename");
            user["username"]="pesce spada";
            resolve(user);
        },2000)
    });
}

async function test(){
 let user= await chiamaAPI("Carlo");
 user = await secondaApi(user);
 user = await terzaApi(user);
 console.log(user);
}

test();

RESULT:
seconda Api user = 1
Seconda Api +set timeout : [object Object]
user é[object Object]
{ id: 1, name: 'Carlo', username: 'pesce spada' }
```

So Await and Async actually uses promises to work. In reality, the Javascript engine will transform the code inside the function to be a ".then" function! But in this way, the code is more clear and way more easy to understand!

The only catch now is that we have not a ".catch" function for errors so we use the try and catch structure :

```
async function test() {
    try {
        let user = await chiamaAPI("Carlo");
        user = await secondaApi(user);
        user = await terzaApi(user);
        console.log(user);
    }
    catch (err) {
        console.log(err)
    }
}
```

In this way, we also can handle errors!
