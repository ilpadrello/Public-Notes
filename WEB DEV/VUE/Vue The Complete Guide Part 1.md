---
title: "VueJS - The Complete Guide (part-1)"
date: "2019-08-22"
---

# WHAT IS VUE.JS

Vue.JS is a framework for front end development, it looks like react in some point but its more powerful and versatile, and it is very small in size (16kb)! the basic principle of vue (like react) it's that the framework itself react to a change, we are going to see this in detail later!

# GETTING STARTER

This is the easy peasy version of installing vue but it is still a good one: Go to the main web site in the installation page: ([https://vuejs.org/v2/guide/installation.html](https://vuejs.org/v2/guide/installation.html)) grab the vue code :

```
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

And now your vue.js is installed! Well done! Good boy!

## HELLO WORLD

Now that we know how to install vue lets try to do the hello world! First of all, we create an index.html file inside on the header we link(install) our vue.js CDN:

```
<html>
  <head>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
  </body>
</html>
```

Then we are going to add this code:

```
<html>
  <head>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
      <div id="app">
      <h1>
        {{ title }}
      </h1>
    </div>
    <script>
      new Vue({
        el: "#app",
        data: {
          title: "Hello World!!!"
        }
      });
    </script>
  </body>
</html>
```

We have created a new VUE instance, this instance is the core of each new vue application, each piece of code you want to use vue.js in, the job of this object is to control their own template of code, of HTML code with they render to the screen. And to do we need to pass an argument to this constructor, an argument that is a literal object, where we specified what we want it to do! And now we need to learn what we are going to put inside this obj: The element : ("el:"), Thanks to this VUE will know which part of our HTML should be under the control of this VUE instance Data: ("data:") this is used to pass the data that we need to show, as you can see we use "title:" and than we refer to this in our HTML using the two bracket syntax "{{title}}", vue will scan the page for this syntax and will replace the part inside!

# THE VUE.JS TEMPLATE MIDDLE LAYER

One important concept of vue.js is the middle template layer! In our code we see that we put "{{title}}" but this is disappeared, and if we inspect the code the brackets are gone, that's because VUE actually create a "middle layer" of template based on our HTML, stores that internally, and then he basically uses this template to create the real HTML code, which then is rendered as the DOM! This is important to understand because this allows us to use template syntax like "{{title}}" and like all the other things that we are going to see later! So VUE read our HTML code, create this middle layer template, and then render the template! Put that aside we can now see how to interact with the DOM using VUE.JS

# INTERACTING WITH THE DOM

## METHODS & FUNCTIONS

In the VUE object, we have more than the "el" and the "data" properties, one other and very important property of the VUE object is the "methods" property! With this, we can create functions that we can easily run in our app:

```
<html>
  <head>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      <h1>
        {{ sayHello() }} //HERE WE CALL THE FUNCTION 
     </h1>
    </div>
    <script>
      new Vue({
        el: "#app",
        data: {
         title: "Hello World!!!"
        },
        methods: { //HERE WE ADD ALL THE METHODS WE NEED!
          sayHello: function() {
            return "Howdy Partner!";
          }
        }
      });
    </script>
  </body>
</html>
```

As you can see we call directly in our template HTML the code (using parenthesis) and the function will return the thing to render! This is useful when we want to use some of the data in our code for example:

```
methods: {
          sayHello: function() {
            return this.title + "Im ready to learn!";
          }
        }
```

Here we use the "this" keyword to refer to the title we already have on our data! (VUE.JS make a connection between data and "this"). But there is much more that we can do with this method...

## BINDING ATTRIBUTES

What happens if we want to put our data into an attribute in our template? Let's imagine that we want to link dynamically to a web site and the link is stored in our data object what we do? You think that we do something like that:

```
//In our Vue Instance

data: {
          link: "http://www.simonepanebianco.fr"
}



//In our HTML code!

<a href="{{link}}">Simone Panebianco</a>
```

unfortunately, this method doesn't work, we can not put " {{ }} " wherever we want, we can only use this where normally text is! So, to solve this problem we use "v-bind"...

```
//In our Vue Instance

data: {
          link: "http://www.simonepanebianco.fr"
}



//In our HTML code!

<a v-bind:href="link">Simone Panebianco</a> //VUE BINDING
```

Now we use the v-bind method to bind the attribute "href", to our data! As you can see we do "v-bind" and then ":" and the name of the attribute, in this case "href"!

## DIRECTIVES

The v-bind method is really a VUE js directive, but what are those? A directive is basically an instruction you place in your code! Vue.js is shipped with a few build-in directives that pretty much covers all the case and the need you may have (but you can also write your own directives, please don't tell anybody!!) The v-bind, for example, instruct VUE.JS to bind something to our data!

## V-ONCE and RERENDERING

What will happens if we have this code?

```
    <div id="app">
      <h1>
        {{ title }}
      </h1>
      {{ sayHello() }}
    </div>
    <script>
      new Vue({
        el: "#app",
        data: {
          title: "Hello World!!!",
          link: "http://www.simonepanebianco.fr"
        },
        methods: {
          sayHello: function() {
            this.title = "Howdy Partner! ";
            return this.title + "Im ready to learn!";
         }
        }
      });
    </script>
```

It should render the "title" before and then when calling the method "sayhello()" the value of title should change... This is not going to work this way! Sorry! Remember the middle layer render? It calls the function before so when you render the page it will be as if the title is not as the original. So how we solve this very mindfu\*@"%% problem? Using directives! In particular, you guessed, the V-ONCE directive like this:

```
<h1 v-once> //HERE
   {{ title }}
</h1>
   {{ sayHello() }}
```

This will stop every re-render and apply the original value!

## RAW HTML!

Do you want to pass HTML to into data? a little bit like this?

```
data: {
          title: "Hello World!!!",
          link: '<a href="http://www.simonepanebianco.fr"></a>'
        },
```

So you need just to write "{{link}}" and HTML will be rendered? Nope! VUE consider all of this like text and this will be shown as planar text, but I think you already have an idea how to fix this... Using directives of course! What directives? "v-html" isn't that obvious?

```
<div v-html="link"></div>
```

## EDIT: NOT FAN
I'm not fan of this approach, it could potentially open the door to some cross site scripting....
## EVENTS

Now we want to increase a number by pushing a button... Easy javascript task... First of all, we need our counter variable in our data:

```
data: {         
          counter: 0
},
```

then we need to create the method that we are going to use for increasing the counter:

```
methods: {
          increase: function() {
            ++this.counter;
},
```

And of course we create the button itself:

```
<button >ADD</button>
```

Now we need to "bind" the button with the increase function! Do we use the "v-bind" directive? No, and that's because we are going to use the event of clicking the button so:

```
<button v-on:click="increase">ADD</button>
```

ok, ok this is not " v-event " but it is also true that "on" is used more often! As you can see you pass an argument on the "v-on" directive, and that argument is the event we want to listen, and we can pass all the events vanilla javascript can handle (onmousedown, input, etc)

## EVENT OBJECT

Every time we an event raise, javascript create an object that contains information about the event,Javascript not VUE, but VUE is capable of intercept that object and we can use it on our app:

```
//HTML
 <div v-on:mouseMove="getCoordinates">{{ x }} / {{ y }}</div>
//VUE.JS
new Vue({
        el: "#app",
        data: {
          x: 0,
          y: 0
        },
        methods: {
          getCoordinates: function(event) {
            this.x = event.clientX;
            this.y = event.clientY;
          }
        }
});
```

"clientX" and "clientY" are properties of the event object : [https://www.w3schools.com/jsref/obj\_mouseevent.asp](https://www.w3schools.com/jsref/obj_mouseevent.asp)

## PASSING YOUR ARGUMENT ($event)

Do you want to pass also your argument to a function? Easy peasy, just do like this:

```
<div v-on:mouseMove="function(argument)"></div>
```

This will override the event object! So you will not be able to use those parameters anymore... nice! Not nice? ok, ok... Well, you can use a second argument called "$event", this private variable (if you don't override it!!!!) contain the event object so you can pass all the argument you want AND the event object:

```
<div v-on:mouseMove="function(argument, $event)"></div>
```

## EVENT MODIFIERS

At the end of an event we can add an event modifier like this:

```
<div v-on:mouseMove.stop="getCoordinates">{{ x }} / {{ y }}</div>
```

And you can also chain them like this:

```
<div v-on:mouseMove.stop.prevent="getCoordinates">{{ x }} / {{ y }}</div>
```

More on event modifiers:  [https://vuejs.org/v2/guide/events.html#Event-Modifiers](https://vuejs.org/v2/guide/events.html#Event-Modifiers)

## KEY MODIFIERS

We have event modifiers but also KEY modifiers for KEYBOARD controls, we can for example specify that a specific function must be called only if the enter key is pressed, to do so you can use key modifiers (implemented by VUE) like :

```
<input v-on:keyup.enter="submit">
```

those can be chained also! More on the subject : [https://vuejs.org/v2/guide/events.html#Key-Modifiers](https://vuejs.org/v2/guide/events.html#Key-Modifiers) You can also [define custom key modifier aliases](https://vuejs.org/v2/api/#keyCodes) via the global `config.keyCodes` object:

```
// enable `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112
```

# WRITING JS IN THE TEMPLATE (MAGIC!)

Since now we have used Javascript code only inside  the vue instance, BUT, we can actually use the code directly inside the "{{}}" and other, look at this:

```
<div id="app">
      // The javascript code is inside the v-on directives
      <button v-on:click="counter++">ADD</button 
      ><button v-on:click="counter--">REMOVE</button>
      {{ counter }}
     //Javascript directly inside the {{}}
      {{
 counter < 10 ? "Counter Less than10" : "Content equal or more than 10" 
      }}
    </div>
    <script>
      new Vue({
        el: "#app",
        data: {
          counter: 0
        },
        methods: {}
      });
    </script>
```

SEE?  This is magical, if you don't have to do too complex javascript algorithms, you can put the code directly inside "{{ }}" or in the directives... This is the magic of VUE.JS

# REACTING PROPERTIES OF VUE.JS

## V-MODEL

We have seen how to update data using the "v-bind" or "v-on" directive, but what if we want a two-way binding? So we want to update the value in data if we modify the input text, but also we want that if somewhere else in the code, something changes the value of that data, this should be reflected inside our input field! This is called two-way data binding... How we do it? With the "v-model" directive! let see:

```
    <div id="app">
      <input type="text" v-model="counter" />
      <button v-on:click="counter++">ADD</button
      ><button v-on:click="counter--">REMOVE</button>
      {{ counter }}
      {{
        counter < 10 ? "Counter Less than10" : "Content equal or more than 10"
      }}
    </div>
    <script>
      new Vue({
        el: "#app",
        data: {
          counter: 0
        },
        methods: {}
      });
    </script>
```

When we update the value of counter using the button this is reflected in the input and vice versa! Even more magical!

## COMPUTED

until now we have seen data, el, and methods inside our Vue.js instance but we are going to add another one, but before we need to understand why: Let's imagine that we have simple "{{ render() }}" that will call a function called render every time we want to check if a data have certain conditions ( more than 10 or whatever ) but this function will be called also every time we Re-Render the page, even if we do it beacause of OTHERS data that have nothing to do with our RENDER function! Let me explain better: Every time there is a change inside the data object Vue does an update on the page, re-rendering all the result (see "v-model") etc, and if there are some functions those are re-executed as well, even if they dont have nothing to do with our change! This mean that if we have slow function for important stuffs, those will be re-called each time there is a change in data! To prevent this we can use the "computed" object:

```
//HTML
{{ check }}

//VUE
 computed: {
 check: function() {
 return this.counter < 10
 ? "Counter Less than10"
 : "Content equal or more than 10";
 }
 }
```

First of all, note that we don't need to use "()" to call the function, the computed object is like a mix between the method and data object, and the increase function will be called only when counter related actions will happen!

## WATCH

Even if using the computed object is a better way when dealing with updates, sometimes (especially when running async code) is useful to use the watch object, why? Well, the watch object looks for modification in specific data, and if there are modifications, it runs a function:

```
watch: {
          counter: function(value) {
            vm = this;
            setTimeout(function() {
              vm.counter = 0;
            }, 2000);
          }
        },
```

Note that inside the watch object we must use the same name of the variable we want to watch. As you can see in the function handler for the setTimout we need to use "vm" as "this", that is because using "function()", "this" is override, this is easily fixable if we use arrow functions that do not create a "this" object, but this will mean also that the code will work only in modern browsers

# SHORTCUTS:

To make the code more readable VUE.js give us some shortcut, let see:

```
//v-on: become @

<button v-on:click="increase">ADD</button>
<button @click="increase">ADD</button>
//v-bind: => :
<a v-bind:href=""></a>
<a :href=""></a>
```

"v-on" directive can be expressed as simply as @ char; "v-bind" directive just simply disappear ad leave just ":"

# VUE.JS AND CSS

Of course, vue.js give us some tools to work with CSS:

## :CLASS

Using the ":class" we can pass arguments, we can pass:

- A name of a class
- A reference to data that contain the name of a class
- An object that contains the name of the classes and true/false value if the have to been showed ({className:true})
- An array of class names and class objects (\[className,{alsoClassName:true}\])
- A reference to a function that returns our array or object

## :STYLE

When using the ":style" command the arguments are a little different:

- An object that contains the style ({backgroundColor:blue})
- An Array of such objects
- A reference to a function that returns such array or object

# CONDITIONAL AND RENDERING LISTS

Vue.js offer also useful tools for templating the page, with lists and conditions:

## V-IF  V-ELSE V-ELSE-IF

The directive "v-if" is very useful; if a certain condition is true or false the directive removes that part of the code (not hide, it removes from the dom!)

```
//HTML
<div v-if="condition">CIAO MAMMA</div>
//VUE
data: {
          counter: 0,
          condition: false
 },
```

```
//HTML
<div v-if="condition">CIAO MAMMA</div>

//VUE
data: {
          test: false
        },
        computed: {
          condition: function() {
            return !this.test;
          }
        },
```

The two way are possible! "v-else" on the other way refer to the last "v-if" directive and is like a normal else, it gets executed if the if is not true:

```
<div v-if="Math.random() > 0.5"> Now you see me </div> 
<div v-else> Now you don't </div>
```

The `v-else-if`, as the name suggests, serves as an “else if block” for `v-if`. It can also be chained multiple times:

```
<div v-if="type === 'A'">
  A
</div>

<div v-else-if="type === 'B'">
  B
</div>

<div v-else-if="type === 'C'">
  C
</div>

<div v-else>
  Not A/B/C
</div>
```

Similar to `v-else`, a `v-else-if` element must immediately follow a `v-if` or a `v-else-if`element.

## V-SHOW

As you know "v-if" don't even create the code inside the page, the code inside the v-if tag is completely gone  when the condition is false. Now if we want the HTML to rest in the page but not showed (like "display:none"), we use v-show. With"v-show" we can not use "v-else" anymore unfortunately

## V-FOR (Cycling thought lists)

Now, you have a list of (everything) that you have to show, that comes from a database or (anything else), you dont have to write a javascript code to cycle inside this list, vue.js provides you with an useful tool for the job: "V-FOR"!!!

```
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```

```
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Learn JavaScript' },
      { text: 'Learn Vue' },
      { text: 'Build something awesome' }
    ]
  }
})
```

in our data, we have stored an array of objects ( a to-do list), now we need to use our directive "v-for" that repeat the tag he is in (<li> in this case) cycling through the list! we specify of course what list this is (in this case todos) and what we want to call each element of the list (todo) using the pattern "element in list" (that is know also in other javascript code)! We don't need to use <li> or <ul>, we don't need to use a cyclable tag, but every tag will be work, he will be just repeated!

## V-FOR (index)

So, now that we can easily cycle through a list, we may want to use in our code the index of each iteration! To do so we need just to change a little bit our code:

```
<div id="app-4">
  <ol>
    <li v-for="(todo,index) in todos">
      {{ todo.text }}{{index}}
    </li>
  </ol>
</div>
```
```
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Learn JavaScript' },
      { text: 'Learn Vue' },
      { text: 'Build something awesome' }
    ]
  }
})
```

What is important here is to use the right position for each variable that we pass, first the object name and then the index name we want to use!

## V-FOR (Template pattern)

The best way to use "v-for" is inside a "<template>" tag, why? a template will not be rendered (unlike the div), and you can so group your code like a div:

```
<div id="app-4">
  <template v-for="(todo,index) in todos">
      <h2>{{ todo.text }}</h2>
      <h4>{{index}}</h4>
  </template>
</div>
```

```
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Learn JavaScript' },
      { text: 'Learn Vue' },
      { text: 'Build something awesome' }
    ]
  }
})
```

## V-FOR (Cycling an Object)

Now, what if you want to cycle through a literal object? You can already do that using the standard "v-for" loop that we have seen, but if you loop inside an object you have one more thing that you can add your code:

```
<div id="app-4">
  <template v-for="(value,key,index) in obj">
      {{key}} : {{value}} [{{index}}] //This will show as key:value [0]
  </template>
</div>
```
