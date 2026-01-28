---
title: "JEST"
date: "2019-08-05"
tags: 
  - "jest"
  - "node-js"
  - "test"
  - "unity"
---

### Fast code:

```
npm i jest --save-dev
npm i @types/jest //Intellisense
npm run test
test('test of test :) ',()=>{});
describe("First", () => { it("should return Simone", () => {});}); //it() equal test()
jest.fn() 
.mockReturnValue(value) //Sync
.mockResolvedValue(value)// Async Resolved
.mockRejectedValue(value)// Async Rejected
```

Organizing tests:

```
it("should do some tests", () => {
    mail.send = jest.fn(); // All Mock functions needed here
    mail.try = jest.fn().mockResolvedValue(true) //All Mock functions here
    // Line break;
    let result = first.notifyCustomer("testmail", "message"); //Call testing functions
    // Line break;
    expect(mail.send).toHaveBeenCalled(); // All expectations here
    expect(mail.try).toBe(true)
  });
```

# JEST: What is Jest?

([https://jestjs.io/](https://jestjs.io/))Jest is a framework tester for NodeJs! Its a wrapper of Jasmine another popular NodeJs tester, and it's used by Facebook to test their React code (but of course it is not limited to react)

## NPM INSTALL

To install Jest we usually use this code:

```
npm i jest --save-dev
```

This will make sure that we install jest only in development env and not for production when we bundle the software! Now that we have installed jest we need to update "script" in our package.json :

```
"scripts":{
       "test":"jest" //Add --watchAll to test on file modifications saved or --verbose to get extra infos
}
```

And you can see that you can find jest inside the "/bin" directory!

Now that this is done we can easily run :

```
npm run test
```

To run our tests!

## NAMING CONVENTIONS

Its a good practice to put all the tests inside a "test" folder (maintainable), and also we use this naming convention: "modulename.test.js" JEST will look for all the files that have "test.js" (or "spec.js") and will consider them as test files to read!

## FIRST TEST

Well, to do the first test now we can simply create a file called "test.js" and put this code:

```
test('test of test :) ',()=>{});
```

This should work! How you can see we don't need to put a "require" for the test method, that's because this file is handled by JEST directly! And here we have a more complex test!

```
const double = require("../fortest.js");
test("testing the test :)", () => {
  let result = double(2);
  expect(result).toBe(4);
});
```

So in this case we load a function from another module to test it! This can be considered as unit testing! We pass to the expect method the result of our testing and, then we use "matchers" (JEST specific methods) to try and error our code! we can pass the result though multiples expect functions! So we can do:

```
const double = require("../fortest.js");
test("testing the test :)", () => {
  let input = 2;
  let result = double(input);
  expect(result).not.toBeLessThan(input);
  expect(result).toBeGreaterThan(input);
});
```

The testing will test each expect and "merge" them as a single test, so here is the result:

```
> jest

 PASS  tests/fortest.test.js
  √ testing the test :) (7ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        1.679s
Ran all test suites.
```

## MATCHERS

This is an important point of our testing framework, even if we can throw an error inside a test like this:

```
test("testing the test :)", () => {
  throw new Error("Something as gone REALLY REALLY BAD");
});
```

With this result:

```
> jest

 FAIL  tests/fortest.test.js
  × testing the test :) (2ms)

  ● testing the test :)

    Something as gone REALLY REALLY BAD

      1 | const double = require("../fortest.js");
      2 | test("testing the test :)", () => {
    > 3 |   throw new Error("Something as gone REALLY REALLY BAD");
        |         ^
      4 | });
      5 | 

      at Object.<anonymous>.test (tests/fortest.test.js:3:9)

Test Suites: 1 failed, 1 total
Tests:       1 failed, 1 total
Snapshots:   0 total
Time:        1.614s
Ran all test suites.
```

it better to let JEST decide how to treat all conditioning, instead of creating all condition by ourself, we should use the Matchers given with the JEST framework! Here is an intro on how to use those: [https://jestjs.io/docs/en/using-matchers](https://jestjs.io/docs/en/using-matchers) And here is the documentation with all of them : [https://jestjs.io/docs/en/expect](https://jestjs.io/docs/en/expect)

## HOW MANY TESTS?

Well, this is a good question! A basic guideline says that you should have the number of unit tests per function to be at least greater or equal to the number of that functions execution path: (practically how many "return" that function has), but this is up to you to think about how many cases are for the function to return a different result! But usually is ">return number"!

## TOO GENERIC? TOO SPECIFIC?

This is also very personal! The test code shouldn't be too specific nether too generic! If the code is too specific and you change the code in a way that the software will work but the test fails, this means the test is too specific If in the other way the result of our test could be wrong and broke our code and the test does not detect it, this is too generic! I think that examples are the best way to understand:

```
let first = function(name) {
  return "Hello " + name;
};

test("testing the test :)", () => {
  let result = first("Simone");
  expect(result).toBe("Hello Simone"); //Too Specific
  expect(result).not.toBeUndefined();  //Too Generic
});
```

In those cases, for example, those expectations are one too specific and the other too generic, let's see why: If we modify the code in this way :

```
let first = function(name) {
  return "Hello " + name + "!";
};
```

The first expect will return an error even if the result of the function will not brake our code! And if :

```
let first = function(name) {
  return null;
};

test("testing the test :)", () => {
  let result = first("Simone");
  expect(result).not.toBeUndefined(); // only this
});

```

The test will pass because "null != undefined"!!

So the test code must be not too much specific either too generic, like this:

```
let first = function(name) {
  return "Hello " + name + "!";
};

test("testing the test :)", () => {
  let result = first("Simone");
  expect(result).toContain("Simone"); //Those two are the same
  expect(result).toMatch(/Simone/); //Here we use regex
});

```

## GROUPING TESTING

For the moment we have not a lot of testing in our code, but in a larger application this will be very very intense to program and we need to structure our testing code! Fortunately, we have already separated all testing per module in different files (with name convention "module.test.js"), but if we need to organize the testing code in the same file what we do? JEST will come to help with the "describe" method! describe is useful to group related tests like this:

```
let first = function(name) {
  return "Hello " + name + "!";
};

describe("First", () => {
  it("should return Simone", () => { //it() equal test()
    let result = first("Simone");
    expect(result).toContain("Simone");
  });

  it("Should return Simone even with regex", () => {
    let result = first("Simone");
    expect(result).toMatch(/Simone/);
  });
});
```

And here it is the result:

```
> jest

 PASS  tests/fortest.test.js
  First
    √ should return Simone (3ms)
    √ Should return Simone even with regex (1ms)

Test Suites: 1 passed, 1 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        1.637s
Ran all test suites.
```

As you can see all the test in the group (describe) "First" results under the same group! So we don't have to repeat the name of the function, that for convention you should put en each test, ad we can convert "test()" to "it()"

## WORKING WITH :

### STRINGS

Here some useful matcher for strings:

```
//or not using .not before
.toBe("Simone") //Exacte Text
.toMatch(/Simone/); //regex for Strings
.toContain("Simone"); //It should contain Simone

```

### NUMBERS

```
//or not using .not before
.toBe(1) //Exacte Number
.toEqual(value) // exacltry like .toBe() for numbers
.toBeCloseTo(number, numDigits?) // For float values to eliminate the problem of rounding!
.toBeGreaterThan(number) //>number
.toBeGreaterThanOrEqual(number) //>=number
.toBeLessThan(number)//< number
.toBeLessThanOrEqual(number) // <= number

```

### NULL, UNDEFINED, NAN

```
// or not using .not before
.toBe(value) // This can be undefined, null, NaN
.toEqual(value); // You can use undefined, null, NaN
.toBeDefined() // The result is defined() (.not Undefined)
.toBeUndefined()// The result is expected to be Undefined
.toBeNull() //The result expected to be Null
.toBeNaN() // the result Expected to be NaN

.toBeTruthy() //the result is TRUE or not Falsy (In JavaScript, there are six falsy values: false, 0, '', null, undefined, and NaN.)
.toBeFalsy() //Ther result is FALSE or Falsy(In JavaScript, there are six falsy values: false, 0, '', null, undefined, and NaN).
```

### ARRAYS

```
//or not using .not before
.toBe() //use resilt[index] to access the result like expect(result[0]).toBe etc
.toEqual([array]) //in this case this will check if the two arrays are equals THIS NOT WORK WITH TO BE!
.toContain(value) //check if in the array there is the value we are looking for! (it doesn't matter where)
.toEqual(expect.arrayContaining([array])) //This will check if the element on the array are present in result
// here we are using expect methods! see documentation for more!
.toHaveLength(number)
 
//NOT SUPPORTED: those will not work!

.toBe([array]) 
.toContain([array]);
```

### OBJECTS

```
.toEqual({obj}) //This Will work because it test if the two objects have the same properties
                       // but the obj must be exaclty the same
.toMatchObject(obj) //This will check if the obj properties are inside result but 
                                  //result can have more properties
.toHaveProperty('key',value) //This will check if result have a property (key) set to value

//NOT WORKING: 
.toBe({obj}) // This will not work because is not the same memory reference
```

### EXCEPTIONS

The way we treat exceptions in Jest is really an exception :)

lets see the code:

```
 expect(() => {
     foo(value);
    }).not.toThrowError();
```

Since we can't store the Trowing error in a return variable, we use this method! we pass in the exception method the function we want to test for throwing errors, and then we use the Matcher ".toThrowError()", to be sure the error is/is not Thrown!

## MOCK FUNCTIONS

Mock Functions are a very important part of JEST! Let see why:

### THE OUTSIDE JEST CODE

This code is useful to understand the principles of what we will do using the JEST methods. So, in this situation we have an index.js like this:

```
//INDEX.JS
let first = require("./first");
console.log(first(1));
```

in the module "first" we have

```
//FIRST.JS
const second = require("./second");
module.exports = function(id) {
  let obj = second.dbGetUser(id);
  return "Welcome " + obj.name;
};
```

in module "Second" :

```
module.exports.dbGetUser = function(id) {
  return {
    id: id,
    name: "Simone"
  };
};
```

So, in the "Second" module we simulate a Synchronous call to a DB and we get back an obj with an ID and a NAME (let's just imagine that this is a real call), in the "First" module (The one that we want to test) we want to return a greeting for the user, so we use the "second.dbGetUser(id)" method to get the user and return a greeting with the user name, and then we just simply "console.log" our greeting!

The thing is that if we want to test the "first" module we need to have access to the database, and this is not a good way to UNIT TEST the module, because we need to disconnect it from the rest of his dependencies (DB, files, etc). So what we can do? Since the loading of each module (even from different modules) is done one time in memory, we can override his methods, and this will not make our code unusable because we are not running the code via its access point ("index.js") but via the testing script ("npm run test").

So we do:

```
let second = require("../second");
let first = require("../first");

//Overriding the method of second and making sure it will return what we want!
//This is a Mockfunction just not a JEST one!
second.dbGetUser = function() {
  return {
    id: 2,
    name: "Alex"
  };
};

describe("First", () => {
  it("should return A greeting for the User given", () => {
    const result = first(1);
    expect(result).toContain("Alex");
  });
});
```

In this way, we have created a "Mock function", a function that we override on the real one to serve our purpose! Did you notice that you don't need to change the "dbGetUser" method inside the "first" module? That's because when first will require the "second" method it will require it from memory where we have already modified the method we need!

But still we have a problem in our code, this is not realistic, normally calls for the DB are asynchronous, and this method is not, let's adjust this!:

"index.js":

```
//index.js
let first = require("./first");
(async () => {
  let greetings = await first.getGreeting(1);
  console.log(greetings);
})();
```

"first.js" module

```
//first.js module
const second = require("./second");
module.exports.getGreeting = async function(id) {
  let obj = await second.dbGetUser(id);
  return "Welcome " + obj.name;
};
```

"second.js" module

```
//Module second.js
module.exports.dbGetUser = function(id) {
  return new Promise((res, rej) => {
    res({
      id: id,
      name: "Simone"
    });
  });
};
```

And Finally the "fist.test.js" test:

```
let second = require("../second");
second.dbGetUser = function() {
  return {
    id: 2,
    name: "Alex"
  };
};
let first = require("../first");
describe("First", () => {
  it("should return A greeting for the User given", async () => {
    const result = await first.getGreeting(1);
    expect(result).toContain("Alex");
  });
});
```

## INTERACTION TESTING

Let's say that the result of a function is just what we call a specific function, a little bit like: "mail.send()" in this case we need to do so intercepting code to understand if the code is run:

"mail.js" module:

```
module.exports.send = function(email, message) {
  console.log("email sended to " + email + " with message: " + message);
};
```

"first.js" Module

```
const mail = require("./mail");

module.exports.notifyCustomer = function(email, message) {
  mail.send(email, message);
};
```

"index.js"

```
let first = require("./first");
first.notifyCustomer("simone.panebianco@gmail.com", "test message");

```

There we want to test if the function of the "mail" module is called or not, to do so we can override the function "send" check the result:

```
let first = require("../first");
let mail = require("../mail");
describe("First", () => {
  it("should run the functon to mail", () => {
    let mailSent = false;
    mail.send = function() {
      mailSent = true;
    };
    let result = first.notifyCustomer("testmail", "message");
    expect(mailSent).toBeTruthy();
  });
});
```

In this way, we override the "mail.send()" function and use that to check if its called or not!

But this is not the proper way to check if a function is called, let's see what is the JEST solution to the problem...

## THE JEST SOLUTION

JEST apport his solution to the problem and this means that you have a lot of features with it! To do the same thing but with JEST you can use :

```
jest.fn()
```

This method returns an empty function that we can fill with a response, let see:

instead of :

```
let second = require("../second");
second.dbGetUser = function() {
  return {
    id: 2,
    name: "Alex"
  };
};
```

We can more easily write:

```
second.dbGetUser = jest.fn();
second.dbGetUser.mockResolvedValue({
  id: 2,
  name: "Alex"
});
```

And instead of:

```
let mailSent = false;
mail.send = function() {
mailSent = true;
};
let result = first.notifyCustomer("testmail", "message");
expect(mailSent).toBeTruthy();
```

we can now use the method ".toHaveBeenCalled()";

```
 mail.send = jest.fn();
 let result = first.notifyCustomer("testmail", "message");
 expect(mail.send).toHaveBeenCalled();
```

As you can see, using the "Jest.fn()" method we have better control on the function that we pass in, like if the function is called, how many times,  with witch argument etc..

```
.toHaveBeenCalled()
.toHaveBeenCalledWith(arg1, arg2, ...);
.toHaveBeenCalledTimes(number)
.toHaveBeenLastCalledWith(arg1, arg2, ...)
.toHaveBeenNthCalledWith(nthCall, arg1, arg2, ....)
.toHaveReturned()
expect(mail.send.mock.calls[0][0]).toBe(value) // This return the first argument in the fest call [call] [arg]
// and more other, look documentation
```

## WHAT TO UNIT TEST?

Unit tests are good for evaluating small functions that have not a lot of dependencies, if you want to use this process for a routing of express you should crate a mock function for "res" "req" and all the method that they have, and this is not a practical way to do so! The best way to do so is to do an INTEGRATION TEST!
