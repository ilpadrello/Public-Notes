---
title: "JS IMPORT EXPORT"
date: "2021-12-29"
---

![](https://samanthaming.gumlet.io/tidbits/79-module-cheatsheet.jpg.gz)

Since ES6, there is a new way to organize your code... Importing it from a different file!  
The import-export syntax, is a little like the `require()` of node modules, but not exactly the same.  
It is supported on most big browsers (besides Internet Explorer (but on edge it works) ).

In order to test this code, you need to have a web server launched, otherwise a core error will be thrown; The live server of VSC works fine.

First let's see how to add the script on the page:

```
<script type="module" scr="./path.js"></script>
```

If you don't specify that this code can import modules via the `type="module"` attribute, the whole thing will not work.  
Also, it's better to use `.js` extensions when looking for the source file, some browsers may not support the automatic addition of the file extension.

Now we need to create 2 files: `main.js` that will be included in the HTML and `module.js` that will be our module.

```
<!-- HTML -->
  <body>
    <script type="module" src="./main.js"></script>
  </body>

//main.js
console.log("main executed!");
import { double } from "./module.js";
console.log(double(5));

//module.js
export function double(n) {
  return n * 2;
}
```

So we get the main.js via the script (`type="module"` !!!) and in that file, we import the double function from `module.js`.  
It is important to notice that we use the destructured way ( `{ double }` ) of declaring the variable double function, why?  
Because we import an object that contains the double function... so in this way, you can have already access to the function directly.

## Exporting more than one thing

You are not limited to on export one thing :

```
//module.js
export function double(n) {
  return n * 2;
}

export function triple(n) {
  return n * 3;
}

export let data = {
  number: 10,
};

//main.js
import { double, triple, data } from "./module.js";
console.log(double(data.number));
console.log(triple(data.number));
```

You can export almost anything, objects, classes, functions, constants, variables, etc...

## Using only one export

you can use only one export statement to export the same things, look:

```
function double(n) {
  return n * 2;
}

function triple(n) {
  return n * 3;
}

let data = {
  number: 10,
};

export { double, triple, data };
```

## Rename your import

You can also rename your import to fit your need :

```
//module.js
import {
  double as doppio,
  triple as triplo,
  data as informazioni,
} from "./module.js";

console.log(doppio(informazioni.number));
console.log(triplo(informazioni.number));
```

## Import EVERITHING

if you want to group all exports in one object you can do this:

```
import * as test from "./module.js";

console.log(test.double(test.data.number));
console.log(test.triple(test.data.number));
```

This way it doesn't matter how many exports you have on the other file, everything will be encapsulated inside a single object ( called `test` in our case).  
What is great about using this method is that you will be sure that you will not clash variables or function in your existing code (inside `main.js` in our case).

## Default Export

Another important concept, when writing import-export variables, is the "**default export**".

```
//module.js
export default function sayMyNameMotherFather(name) {
  console.log(name);
}

//main.js
import funcName from "./module.js";
funcName("Walter White");

```

The default export is the function that will automatically be attached to the name ( in this case `funcName` ) that we pass in.  
Does this mean that you can not export other things when you export a default value? Of course not:

```
//module.js
export default function sayMyNameMotherFather(name) {
  console.log(name);
}

export let data = {
  simone: "best",
};

//main.js
import funcName, { data } from "./module.js";
funcName("Walter White");
console.log(data); // output the object.
```

Or you could do something like this :

```
//main.js
import funcName, * as dati from "./module.js";
funcName("Walter White");
console.log(dati.data);
```

Notice that the default exported function will not be in the \* object.
