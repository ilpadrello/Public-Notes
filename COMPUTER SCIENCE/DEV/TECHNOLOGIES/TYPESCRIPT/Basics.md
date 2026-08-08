---
title: "Typescript"
date: "2022-10-17"
---

Typescript is a layer on top of JavaScript. And it lets you add Typing to JS. It makes JS behave! (Good boy)

Technically speaking, TS is a programming language that runs on top of JS, so every JS file is a valid TS file. But TS adds some useful benefits:

## Why?

- Static Typing
- Code completition
- Refactoring
- Shortand Notations

Ok, but what is a statically-typed programming language? Well, we have two kinds of PL: Statically-Typed and Dynamically-Typed.

In Statically-Typed PL (like C++,C#, Java) you know the type of variables at compile time or while coding, for example, you can declare a variable of type INTEGER, and this variable can only have integer values! This was very useful at the beginning of programming because it let you use way less memory (a string takes way more memory than an integer).

In Dynamically-Typed PL, the type of variable can be anything and change over time.

While this is great and gives us a lot of flexibility, it is also prone to errors! For example, if you pass the wrong type of variable in a function.

Typescript is essentially JavaScript with type checking, you explicitly set the type of our variables upon declaration, like in Static-Typed Languages, and then we pass the code to the TS compiler, and the compiler will tell you if there is something wrong.

## Wait, there is more...

Typescript is more than that, modern code editors have great support for TS, and can also help you with productivity-boosting features like: code completion, refactoring, but also TS have additional features that help us write more cleaner and concise code.

## Why Not ?

TS has also some drawbacks: There is always a compilation step, you can not run TS on your browser, it needs to be converted to JS first!

A simple rule of Thumb is that JS is more suitable for small, simple projects.Where you go fast! But for complex, and not easy to maintain projects, it is usually better to use TS.

## SETTING UP A DEV ENVIRONMENT

In order to install the TS compiler, you need NPM!

```
npm install -g typescript
```

## CONFIGURING TYPESCRIPT

A powerful tool, while using TS, is the possibility to configure the transpiler. There are a lot of configurations that you can add, and TSC (TypeScript Compiler) helps you find what you need with:

```
tsc --init
```

This command will configure a typescript basic configuration, putting everything you may add in comments, so you need only to uncomment the configuration you need.

## HELLO WORLD!

Let's take this simple TS file:

```
//helloworld.ts
let message: string = "Hello World!"; //Note the Typing
console.log(message); 
```

Note that we added the variable type!

Now let's convert this into JS:

```
run tsc helloworld //This will run the compiler
```

Now you have a file called helloworld.js in the same folder of the TS file.

## CONVERT ALL JS FILES

Of course, you don't need to convert your files one by one, using just the command TSC, the compiler will find all the JS in that folder and convert them into JS files (it will not work if you don't have a `tsconfig.json` file). This will also affect all the JS files in subfolders (I think you may configure TS to exclude some folders)

## CONFIGURING THE TRANSPILER

One big advantage of using a transpiler is that you can configure it. You can specify a lot of stuff, and TS makes things easy for you by delivering a pre-created config file, that has most of the configuration commented. In this way, you will just have to uncomment and change the configurations that you need.

In order to get the Configuration file run :

```
tsc --init
```

This will create a tsconfig.json file. Now let's have a look at some of those configs:

### target

Let you choose the version of JavaScript that the compiler will generate. By default, is set to `"es2016"` which is an old standard and it's been implemented in all browsers. Depending on the project you are working in, it may be better to change to a newer version that keeps the transpiled code shorter. If your remove the value and press `CTRL + SPACE` you can see all valid values (in VS Code)

### rootDir

Specify where the TS code is located! By default, is the root folder, but usually is inside a `src` folder.

### outDir

Specify where you want your JS compiled files to be (e.g `./dist`)

### removeComments

If your code has a lot of comments, this will automatically remove the comments and have the files lighter.

### nnoEmitOnError

This will stop the transpiler if it found errors! By default, even if it find errors, the JS will be generated.

## DEBUGGING

How to debug TS? To be able to debug TS code, it is better to enable the `sourceMap` feature in the config. A source map is a file that specifies how each line of our typescript code maps to the generated JS code! This is not for humans to understand, this is for machines!

In order to use breakpoints in VS, insert a breakpoint and then, in the debug panel, click on **_create a launch.json file_**, from the dropdown you select node.js. This creates a new file called launch.json in our project, that tell VS Code how to debug the application. You can change it to make work better for you, and in our case we need to add a pre-launch task to this JSON, so that we build all the TS before launching the debugger:

```
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "node",
            "request": "launch",
            "name": "Launch Program",
            "skipFiles": [
                "<node_internals>/**"
            ],
            "program": "${file}",
            "preLaunchTask": "tsc: build - tsconfig.json" //Here
        }
    ]
}
```

Now you can start debugging like normal in the debug panel.

## BUILT-IN TYPES

So, JavaScript has his native types:

- number
- sting
- boolean
- null
- undefined
- object

But Typescript introduces some more of them, like:

- any
- unknow
- never
- enum
- tuple

Before going in detail on how to use those, let's play a little bit with primitives:

```
let number: number = 123_456_789;
let course: string = 'TypeScript';
let is_published: boolean = true;
```

Note that we use the name of the variable + ':' and the type you want to put inside.  
Also, note that in TS for large numbers, you can separate with an underscore to be more readable.

Well, it is also important to know that you can remove all the types in your code, and TS will understand the type with your declaration.

Also, you could create an empty variable that would be of type `any`, but a `any` type lose the strict control of TS, and can become anything, so in general, it is better to avoid this!

```
let number= 123_456_789;
let course = 'TypeScript';
let number= 123_456_789;
let course = 'TypeScript';
let is_published = true;
let level; //any -> This can change type
let lesslevel: number; //Empty number ->  This can not change type
```

## ARRAYS (Hurray)

While in JS you can put any Type of value in the same Array, TS thinks that is no good, so you can specify that your Array must contain only one kind :

```
let numbers: number[] = [1,2,3] //The annotation can be removed cause everything is a number
```

You can also have an array like in JS, to do so they must be of type `any`.

## TUPLES

TS has a new type called Tuple which is a fixed-length array where each element has a particular type:

```
let user:[number,string] = [1,'Simone']
```

Even if you can have more than two values in a Tuple, it is better to have just two!

## ENUMS

Enums are a list of related constants, like in Mysql or JS. You specify the possible values in something that looks like a prototype:

```
enum Size { Small = 1, Medium = 2, Large = 3};
```

And now you can set a variable of this Type:

```
enum Size { Small = 1, Medium = 2, Large = 3};
let mySize: Size = Size.Medium;
```

What do we have in particular here? Well, `enum` is a Type, but the Size becomes a kind of Type too! In fact, when you declare `mySize`, you specify that will be of Type `Size` (noticed that we use PascalCase?!).

## FUNCTIONS

Like in other Statically-Typed programming languages, you can specify the type of value that the function will return:

```
function returnZero(numero:number):number{
    return 0;
}
```

Here the benefit is that you forget the return, or if you return a different value, you will have an error immediately.

### Making Paramters Optionals

It is usually better to not have optional parameters in your code, but if it is needed you can add a `?` after the name of the parameter :

```
function returnZero(numero?:number):number{
    return 0;
}
```

What if there is an if statement in your code then? You could do like this:

```
function foo(numero?:number):number{
    if((numero || 10) < 10 )
        return 10;
    return 100;
}
```

Or you could do something like this :

```
function foo(numero:number = 50):number{
    if(numero < 10 )
        return 10;
    return 100;
}
```

## OBJECTS

You know that in JS objects are Dynamic, their shape can change throughout the lifetime of the software, and you can add info and remove info. Unfortunately, in TS this is not valid, the compiler will not be happy about that! In TS before you create an object, you have a prototype of that object:

```
let student:{
    id: number,
    name: string,
    fax?: string //optional
} = { 
    id: 1, 
    name:''
}
```

See that we create a template for the object that we want to create, you can not add more key to the obj, but you can set the values are optional.

Did you know that you can specify values that are read only?

```
{
readonly id: number,
...
```

Now we add some functions:

```
let student:{
    readonly id: number,
    name: string,
    fax?: string,
    foo: (date:Date)=>boolean
} = { 
    id: 1, 
    name:'',
    foo: (date:Date)=> {
        return true;
    }
}
```

This is how you add a function to your prototype... There must be a better way to work with those...

## ADVANCED TYPES

Ok, we will learn about:

- Type aliases
- Unions and intersections
- Type narrowing
- Nullable types
- The `unknown` type
- The `never` type

## TYPE ALIASES

Ok, see how you have to treat objects in TS, i think it is a minimum feature to save the prototypes as Types... Let's see what the tutorial say!

Ok so, with the Type aliases we can create custom Types and reuse them more than one time.

```
type Student = {
    readonly id: number,
    name: string,
    fax?: string,
    foo: (date:Date)=>boolean
}

let student: Student= { 
    id: 1, 
    name:'',
    foo: (date:Date)=> {
        return true;
    }
}
```

Now we can have more than one variable with this custom Type.

## UNION TYPES AND NARROWING

With union Types, we can give a variable or a function parameter, more than one type:

```
function KgToVolume (weight:number|string){ //Here this can be more than one kind.
    if(typeof weight === 'number'){ //This is called narrowing
        weight.toString
    }else{
        console.log(weight);
    }
}
```

Using `|` we can specify more than one Type.  
If you want to make sure that you can use the right function based on the type, you should do some narrowing and make sure of the type!

## INTERSECTION TYPES

You can combine two or more types... this could be useful with objs:

```
type AA = {
    talk:()=>void
};

type BB = {
    listen:()=>void
}

type CC = AA & BB;

let cC:CC = {
    talk:()=>{},
    listen:()=>{}
}
```

## LITERAL TYPES

Literal Types let you limit the values that you can assign to a variable:

```
let weigth: 50 | 100 | 150 | 200;
weigth = 50;
```

Or you can do somthing like this:

```
type Weight = 50 | 100 | 150 | 200; //Reusable.

let weight: Weight;
weight = 50;
```

## NULLABLE TYPES

Waist of time, just remember that if you want to pass null or undefined type they must be declared as Type (e.g. `number | null | undefined`)

Remember that you can use `?.` to make sure that the value is not null:

```
customer?.birthday?.getFullYear();
customers?.[0];
```

## NULLISH COALESCING OPERATOR

By default falsy data are `undefined, null, '', false, 0` but sometimes you want 0 to be considered as 0 and not a falsy value.  
For this reason TS introduces the `??` operator that will check only for `null` or `undefined`.

```
let first: number = a_value ?? 30;
// This is the same as doing this:
if(a_value !== null && a_value!== undefined){
  first = a_value;
} else{
  first = 30;
}
```

## TYPE ASSERTION

This is an important one!  
Sometimes we know more about the Type of an object that TypeScript, how? For example when working with external data, like the DOM of a page.  
Let's look a this example:

```
let elem = document.getElmentById('elem'); 
```

By default, TS knows that we are going to receive an `HTMLElement`, which is a kind of `HTMLObject`.

But you could also receive an `HTMLInputElement` using the `getElementById`, and that would give as different functions and property that we can call, but TS seems to not remember that! So we can force the TYPE we are going to receive (not mutating it, just letting TS know the Type) using this two syntax:

```
let elem = document.getElementById('elem') as HTMLElementInput; //note the as keyword
//or 
let element = <HTMLElementInput> document.getElementById('elem'); //we use the < >
```

This way we get all the features we need to work, and TS will not scream at us!

## THE UNKNOW TYPE

The Type `any` we have seen that could be everything, and we should never use it, but sometimes don't know what type we will get, so what to do?  
For these cases, you have the `unknow` type!  
If the `any` type stops every control of on types, the `uknow` type just leave the control (or I should say the narrowing) to the programmer.

Let's look at this code:

```
function render(document: unknow){
  document.toUpperCase(); //This will give us an error;
}

//Narrowing it
function render(document: unknow){
  if(typeof document === 'string'){ //Use instance of for custom objects e.g. instanceof WordDocument
    document.toUpperCase(); //Now TS is happy!
  }
}
```

As you can see, by narrowing the type in our code, we make sure that we can access all the correct functions only if they are really there.

## THE NEVER TYPE

Let's you indicate that a function will never return something and make then the code that comes after unreachable (like if you want to throw an error in the function).

TS now, know about that and can help you while coding.

## WORKING WITH CLASSES

```
//Example of class
class Account {
    id:number; //Properties
    owner: string;
    balance: number;

    constructor(id:number, owner:string, balance:number){ //Constuctor always return an istance of the obj.
        this.balance = balance;
        this.owner = owner;
        this.id = id;
    }

    deposit (amount:number):Number{
        if(amount<0){
            throw new Error('Invalid Amount');
        }
        this.balance+=amount;
        return this.balance;
    }
}
```

This is how you create classes in TS (look comments for more infos).

The properties of the class are not for JS, if you transpile the code you will find that those were not defined before, but they are useful for TS to understand what you want to do and make sure you don't get any errors.

There is also a much more concise way of writing a constructor, but it is now always good because it lets you declare properties inside the construction function. This will result in properties that you don't see at the top, and this can lead to confusion for large objects.

```
class Account2{
    faxNumber?: string;
    constructor(
       private readonly _id:number, 
       public owner: string, 
       private _balance: number ){

  }
}
```

Now let's create an Object from this class:

## READ-ONLY

With this use of the class Account, we can at each moment change the id of the account from everywhere in the code, and obviously, this is no good!

Fortunately, we can have read-only property:

```
//Example of class
class Account {
    readonly id:number;
//...
```

Now, if you try to change it you will get a compilation error!

## OPTIONAL PROPERTIES

What if you want a property to be optional? Like a fax number? You can add a `?` to the property and you will not have errors in your code:

```
class Account {
   faxNumber?: string;

```

## OTHER MODIFIERS

In TS, we have also other modifiers that let you create a more robust code, let's have a look:

## ACCESS MODIFIERS (PUBLIC - PRIVATE - PROTECTED)

This is straightforward: public means accessible from outside the class, private only inside the class, and protected are accessible from extensions of the class.

## GETTING AND SETTERS

In PHP, getters and setters are not special functions, we name them that way just because we want to understand easily what they do.

A getter is a function that gives us the value we are looking for (e.g. `getName` will give us the name), and setters functions lets us change the value of a property (e.g. `setName` = 'Simone'). This has the advantage that we can protect the value, and perform some validation before changing the value.

Now, in TS, the concept is the same, but by using the `get` and `set` keywords we can write and read the value as if it was not a function:

```
class Account2{
    faxNumber?: string;
    constructor(
        private readonly _id:number, 
        public owner: string, 
        private _balance: number){

    }

    get balance(): number{
        return this._balance;
    }

    set balance(amount:number){
        if(amount>0)
            this._balance = amount;
    }
}

let account2 = new Account2(1,"Simone",1_000_000_000_000);
console.log(account2.balance)
```

## INDEX SIGNATURES

While in JS you can add and remove properties of objects, this is not the case in TS.  
Sometimes, unfortunately (maybe not) you need to add properties dynamically, in those cases you can use `index signatures`:

```
//Index Signature
class SeatAssignment{
    [seatNumber:string]:string;
}

let seats = new SeatAssignment;
seats.A1= 'Simone';
```

Now you have type control even for dynamic properties.

## STATIC MEMBERS

Static Members act like the static property in PHP.

The property is not of the class but of the class itself, so it is shared between the objects. Changes are reflected on all objects.

```
//Static Members - property is shared between obj of the same class.
class Ride{
    static activeRides: number = 0;
    start() { Ride.activeRides++};
    stop() {Ride.activeRides--};
}

let ride1 = new Ride();
let ride2 = new Ride();

ride1.start();
ride2.start();

console.log(Ride.activeRides); //2
```

Ok, let's have a look: first, even if the two rides are two different objects, the value of `activeRides` is 2, because the value is shared between objects, also you can see that we have used the `static` modifier to make the property static, next we see that if we want to change the value, we don't use the keyword `this` because that would refer to the object itself, while we need to refer to the class, so we use the Class name.  
That applies even if we want to log the value!

Of course, you could have this value `private` and create getters, or just use the `readonly` modifie

## CLASS INHERITANCE

Of course, you can extend classes:

```
//Class inheritance
class Person{
    constructor(public name:string,public surname:string,public email:string){
    }
    get fullName(){
        return this.surname+' '+this.name;
    }
}

class Teacher extends Person{
    constructor(private Id:number, name:string, surname:string, email:string){
        super(name,surname,email);
    }

    override get fullName(): string {
        return 'Professor '+super.fullName;
    }
}
```

Notes:

Use `super` to call the parent `constructor`, also you don't need to have the same parameters in the object (note that in the child we don't have modifiers)

Use the `super` keyword to call a parent function.

Using the keyword `override` to override a parent method.

## POLYMORPHISM

This is more of a concept. Poly = Many and Morph = form. The concept is like in PHP when you have interfaces.

Since you can have a `superclass` like "`Person`" and extend 3 `subclasses` (`Student`, `Teacher`, `Personnel`) you can now iterate from a list of `Person` objects and include all the child classes. They have the same methods, even if you override them! That means that you can have many forms of the same parent... Polymorphism! And this is a perfect way to introduce the:

## OPEN CLOSED PRINCIPLE

The idea behind this principle is simple, think twice, even thrice, and test your code and if it works don't change it anymore (don't change methods name or other things, but please correct bugs). If you want to change something, create a new class:

> Classes should be **open** for extension and **closed** for modification
> 
> A very smart programmer (uncle bob)

Of course, this is not always possible, but is a good practice.

## ABSTRACT CLASSES

An abstract class is a class that you can not create as an object. It lets you create a blueprint of the class, but you will have to create a child class in order to create an object.

Abstract classes can have real methods and properties even if you can't create an object, but if you need a method that the programmer MUST implement in the child, you can use abstract methods:

```
//Abstract class and methods
abstract class Shape {
    constructor(public nLines:number) {
    }

    getNLines():number{
        return this.nLines;
    }

    abstract render():void;
}

class Square extends Shape{
    constructor(nLines:number){
        super(nLines);
    }

    render(): void { //This MUST be implemented
        console.log("[]");
    }
}
```

## INTERFACES

Do you have a class where everything is `abstract`? Use `interface` instead!

```
interface Idea{
    name:string;
interface Ideas{
    name:string;
    getIdea(idea:string):void;
    evalueIdea():void;
}

class thought implements Ideas{
    public idea?:string;
    constructor(public name:string){
    }

    getIdea(idea:string): void {
        this.idea = idea;
    }

    evalueIdea(): void {
        console.log("Eh.... so so");
    }
}

```

Interfaces are implemented, not extended!

No need to write "`abstract`" All this time. Either way, not `abstract` or `interface` are JS valid code! Those are just concept pushed by TS to make your software behave better!

### **Also, interfaces can be extended into other Interfaces**!

## INTERFACES VS TYPES

In TypeScript, interfaces and type aliases can be used interchangeably. Both can be used to describe the shape of an object:

```
//Interface
interface Person {
  name: string;
}

let person: Person = {
  name: 'Simone',
};

//Type
type Person {
  name: string;
}

let person: Person = {
  name: 'Simone',
};
```

A class can also implement an interface of a type alias:

```
class MyCalendar extends MyInterface {} 
class MyCalendar extends MyType {}
```

It’s more conventional to use an interface in front of the extends keyword, though.

## GENERIC CLASSES

What problem this will solve? If you need to pass the type of single or multiple values when instantiating a class, this will solve the problem:

```
/**

 * Generic classes are used to specify the type of a memeber of the class at instantiation time

 */



class KeyToValue<T>{

    constructor(public key: T | number, public value:string){}

}



let pair = new KeyToValue<string>("1","value");
```

Typescript uses `T` as placeholder because this feature came from C and there this is called a template class.

Of course you can have more than one type placeholder if you want :

```
class KeyToValueTwo<TKey, Tvalue>{

    constructor(public key:TKey, value:Tvalue){}

}



let pair2 = new KeyToValueTwo<number,string>(1,"ciao");
```

You could also not pass `<number,string>` because the compiler will automatically understand the type via the parameter you pass.

```
class KeyToValue3<TKey, Tvalue>{

    constructor(public key:TKey, value:Tvalue){}

}

let pair3 = new KeyToValueTwo(1,"ciao");
```

## GENERIC FUNCTION

The same thing can be done with functions, (and I think, that, is way more useful), let's imagine that you want to create a function what wraps a value in an array, it does not matter what the value type is! So we can do something like this:

```
function wrapInArray<T>(value:T){

    return [value];

}

wrapInArray("ciao");
```

## GENERIC INTERFACES

In TS you can also make generic interfaces:

```
//let's imagine that you will call an api and wait for the response. You don't know by default what the reposnse is so:

interface Result<T>{

    data : T | null,

    error: string | null

interface Result<T>{

    data : T | null,

    error: string | null

    

}



function fetch <T>(url:string): Result<T>{

    return {data: null, error: null}

}



interface User{

    username: string

}



interface Products{

    name:string

}



let result = fetch<Products>("url");

let result2 = fetch<User>("url");

 

result.data.name

result2.data.username


```

In this way you there is no need to specify what you are waiting for when creating the interface but you can specify that later, when calling the function.

## GENERIC CONSTRAINT

Generic constraint let's you specify a list of possible type that you want to use as generic.

```
function echo<T extends string | number >(value:T){

    return value;

}



//echo(true); //Not working
```

Of course, this is not limited to generic types, you can use interfaces, classes, etc

I'm not very sure about the usefulness of this, but hey, if they invented it...

## EXTENDING GENERIC CLASSES

I'm not awake enough for this right now.

Since you can create Generic Classes / Interfaces can you also extends those classes and intefaces? Yes! And you have two ways of doing this: You can say "The user will define what type this will be when using it" or you can say "Ill tell you now what type will be, the user will Stick with that".

```
class Generic<T>{

    constructor(private value:T){}

}

class NotSpecific<T> extends Generic<T>{}

class Specific extends Generic<string>{}



interface Generic2<T>{

    name:T

}
interface NotSpecific2<T> extends Generic2<T>{}

interface Specific2 extends Generic2<string>{}
```

In a way we can say that we are going to "transfer" the information about the type later (piping the decision), or you can choose right now what you need.

And of course you can use Generic constraints for futher define your `T`

## THE KEYOF OPERATOR

The `keyof` will trasnform the key values of a type (interface, or even generic) into a "x | y | z" that will be usable for defining a type!  
For example we can do something like this:

```
type Point= {x:number, y:number}



type P = keyof Point;



function test(key:P){

    console.log(key);

}



test("x");
```

Using this methods we say that the value of the type P can be only the keys of the Type Point.

If the type has a `string` or `number` index signature, `keyof` will return those types instead:

```
type Arrayish = { [n: number]: unknown };

type A = keyof Arrayish;

//A will be of type number



type Mapish = { [k: string]: boolean };

type M = keyof Mapish;

//M will be of type string
```

## TYPE MAPPING (Utility Type)

Ok, this is big, and I'm not very sure about it, and I really would like to improve my knowledge of this...  
You can map an Type to manipualte the type itself (make it readonly or optional), this can be useful but since the code is pretty much always the same, TS created speciific Utility Type that you can use to do the job without mapping things out:

[https://www.typescriptlang.org/docs/handbook/utility-types.html](https://www.typescriptlang.org/docs/handbook/utility-types.html)

```
interface Properties{

    name:string,

    email:string

}



type ReadOnlyProperties = {

    readonly [PropertiesKeys in keyof Properties]:Properties[PropertiesKeys]

}



let product: ReadOnlyProperties = {name:"Simone",email:"email"}



product.name="ciao"; //This will be an error because now is readonly



type Two = {

    [key:string|number]:any

}



let obj:Two = {

    "alla":"catalla",

    3:true

}





type Test = {

    [P in keyof Two]: Two[P]

}



let testing:Test = {

    "alla":"catalla"

}
```

## MODULES

Use `CONTROL+.` to have help in Vscode (very useful)

```
//shapes.ts
export class Circle{ // ...

export class Square{ // ...

//Anywhere.ts
import {Circle as MyCircle, Square] from "./shapes" 
//or
import * as Shapes from "./shapes" //This will create a oject containing everything Shapes.Circle or Shapes.Square

```

Use Re-exporting to make numerous imports in a signle file.

## MODULE FORMATS

Historically, JS did not have modules, and peoples found ways of implementing them via code.  
JS understood the need for the model architecure and implemented them into JS directly.

In config you can choose how you want JS to import the modules when transpiling the code ("CommonJs" or "ES2015" for example).

## MIXING JS AND TS

You can mix TS and JS but you need to configure TS in order to do so:

```
"allowJs": true //You need to uncomment that
```

Now you can import from JS without problems. but if you want to force some Type checking in the JS file you can also uncomment the "checkJs" that will perform a basic type checking.

Also if you want to shut the f\*\*\* up the compiler, you need just to add a comment `//@ts-nocheck`

You can also help TS understands what it is going on using JSDoc!

```
/**

 *

 * @param { number } value

 * @returns { number }

 */

export function calculateTax(value) {

  return value * 0.3;

}

```

Now TS will not yell to us, and will throw the correct errors when needed.

Or you can create declaration file, this is useful if you dont want to modify your JS file. First you need to create a `.d.ts` file with the same name as the file you want to add typing, and then you can type stuff like this:

```
export declare function calculateTax(value:number):number
```

Exporting the declaration in this file, TS will understand the connection with the JS file and make the adjustments

## DEFINITELY TYPED

Even if in these day, people usually put `.d.ts` declaration files for populars repository, if you want to use a popular JS module but unfortunately it is not typed for TS you can try to use a famous library that does that for you:

[https://github.com/DefinitelyTyped/DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped)

Let's imagine that you want to add `lodash` you can just run:

```
npm install lodash //not usable for TS but needed for JS after conversion
npm install --save-dev (or -D) @types/lodash
```

Now you can import `lodash` normally

```
import * as 'lodash'
```
