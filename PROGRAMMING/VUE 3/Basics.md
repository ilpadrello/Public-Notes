---
title: "VUE 3 BASICS"
date: "2021-12-21"
---

Let's build this new adventure in Vue.js 3 !  
Even if there is no need to explain what vue.js is, ill do a simple introduction:  
Vue is a JavaScript front end framework, that makes building interactive and reactive web frontends easier.  
Also Vue 3 make easier the connection with an API to fetch and change data.

Vue can be used to a part of a page (like a widget) or you can use it to render all the page! In that case, we call it Single Page Application.

## INSTALLING VUE

To install vue refer to this page: [https://v3.vuejs.org/guide/installation.html](https://v3.vuejs.org/guide/installation.html)  
Fast CDN :

```
 <script src="https://unpkg.com/vue@next"></script>
```

We will see later the advantages to install via NPM and also installing Vue CLI.

## FIRST PROJECT DIFFERENCES

Since I already know how to do some basic stuff with vue.js (2), here are some basic differences when you pass to vue 3 :

```
const app = Vue.createApp({
  data() {
    return {
      goals: [],
      enteredValue: "",
    };
  },
  methods: {
    addGoal() {
      console.log(this.enteredValue);
      this.goals.push(this.enteredValue);
    },
  },
}).mount("#app");
```

- Data is no longer just an object, but a function that return an object !
- You can use `data() {}` to indicate that is a function with modern JS
- To mount the app, there is no more `el :` , now you use the mount function and you pass the ID inside the function

## FOR THE REST

The other thing, for now, are preatty much the same, methods are methods, and they works like always

## PASS THE EVENT AND AN ARGUMENT TO A FUNCTION IN VUE

When you call a method within vue, if that call is the consequence of an event (click, input, etc), you can do this :

```
//HTML
<input type="text" v-on:input="setName">
<p>Your Name: {{ name }}</p>
//JS
... //data obj with name variable
methods:{
   setName(event){
      this.name = event.target.value;
   }
}
```

Since you call the method `setName` using without `()` the event object is passed by default (the event object is a native js object).  
But what if you want to pass another parameter to this method? Usually, you should do something like this : `v-on:input="setName('param')"`, but the event parameter is lost this way!

The solution is using a special variable that Vue.js let you use to pass the event : `$event` :

```
//HTML
<input type="text" v-on:input="setName($event,suffix)">
<p>Your Name: {{ name }}</p>
//JS
... //data obj with name variable
methods:{
   setName(event,suffix){
      this.name = event.target.value+' '+suffix;
   }
}
```

## EVENT MODIFIERS

Imagine that you want to create a form, but, when you click to submit, the form will not submit, but the data must be validated before hand (using something like `e.preventDefault()` for example).  
In order to make this code happen, you could do something like this :

```
//HTML
<form action="" v-on:submit="submitForm">

//JS
methods:{
  submitForm(event){
    event.preventDefault();
    alert('Validating');
  }
}
```

This is a perfectly reasonable way to obtain your goal but, there is a better way (for the explaining reason), you could use event, modifiers.

Event modifiers are modifiers that you can connect to your event! Here is the syntax : `v-on:submit.prevent`

```
//HTML
<form action="" v-on:submit.prevent="submitForm">

//JS
methods:{
  submitForm(){

    alert('Validating');
  }
}
```

The result is exactly the same, it will prevent the default behavior of a form.

Modifiers can also be chained if needed : `<a v-on:click.stop.prevent="doThat"></a>`

For key pressing events, when you want to specify which key is being pressed, there are also "**key event modifiers**" :

```
<!-- only call `vm.submit()` when the `key` is `Enter` -->
<input v-on:keyup.enter="submit"

<input v-on:keyup.page-down="onPageDown">
```

You also have "mouse event modifiers" and "exact modifier"... More info here :

[https://vuejs.org/v2/guide/events.html#Event-Modifiers](https://vuejs.org/v2/guide/events.html#Event-Modifiers)

## METHODS IN HTML ARE ALWAYS CALLED WHEN SOMETHING CHANGE

When you create a method, and you call it from the HTML, if you have any changes in the data object (but not only) the method is always called! Even if that change is not affected (or will affect) the data in the method called.

And this is why you should use computed properties!

## COMPUTED PROPERTIES

I will do a fast explanation of computed properties:

Even if a computed property is set as a function, you should think of it as a property (a variable if you want).  
Vue.js will map what variables are needed for that function to execute and when one of these variables changes, it will re-run the function to get the computed result.

```
//HTML
<p> {{ fullName }} </p>

//JS
...
data(){
  return:{
    firstName:'Simone'
    familyName:'Panebianco'
  }
},
computed:{
  fullName(){
    return this.firstName+' '+this.familyName;
  }
}
```

As you can see, you don't put `()` in `{{ fullName }}` and that is because you need to treat the info as data and not a function.

Vue.js will map all the data needed to make the fullName and if something changes, it will automatically call the computed property and change the page.

## WATCHERS

Watchers are functions that are called when dependencies change, you could say that is the same concept of computed properties, and the basic concept is the same, but the big difference is that :

**Computed properties** changes if ANY of the dependencies changes

**Watchers** are functions that get executed only if the specific target variable change, no multiple dependencies

So when are watchers more useful than computed properties?  
Well, we can start with the fact that watchers are functions, they do not display something (you are not obligated to!), while the execution of CP is only called if you have your CP on your page.  
Also, you can modify variables or other stuff that are not directly dependent on your variable, this will not work in CP.

Those are 2 different ways of reacting to changes, both useful in different ways.

## UNDERSTANDING THE KEY IN V-FOR

When do a `v-for` in Vue.js and you remove one item from the list, you will probably occur into a "bug" that Vue.js has by default...  
Let's imagine that you are cycling for a series of inputs, and you put inside one of them a value, if you remove one object from the list (and you remove an input doing so) your input text will shift! Why? That's because Vue.js, do not actually "remove" the item but shift all the dynamic values, and the text that you put inside the input text is not a dynamic value.

If you want to prevent this kind of behavior is really a good practice to always use the `:key` binding. Don't make the error to use it on the index of your array btw because that will not work, since when you remove an item from the array the id will change too, try to use something that is always the same (like the id from the database, or the actual item you want to list).

## VUE UNDER THE HOOD / JS PROXIES

It could be interesting to understand how vue permorms it's magic! how does it know when you make a change to a variable? And then launche whatchers / computed properties ? It uses Javascript proxyes! More infos here : [http://www.simonepanebianco.fr/javascript-proxies/](http://www.simonepanebianco.fr/javascript-proxies/)

## UNDERSTANDING TEMPLATES

Since now, you have seen how to transfer information from your Vue obj to the DOM, you put some `{{ powder }}` in your html and the magic happens, but you need to know that you also have another way, you can use templates.

Easy theory, you just add `template:` in your Vue object and the content is automatically added to the mounted id :

```
//HTML
<div id="app">
</div>

//JS
const app = Vue.createApp({
   template: `<p> {{ infos }} </p>` //with backticks you can make multiline

});
```

Now, the Html that you put in your template will be automatically added to the app div

## REFS

Refs are just a way to easily refer to HTML items in your Vue object:

```
//HTML
<input ref="nameOfRef">

//JS
...
methods:{
   foo(){
       console.log(this.$ref.nameOfRef.value);
   }
}
```

In this way, you get the entire HTML obj.

## VUE LIFECYCLE

Vue has a "lifecycle" from the beginning, when you mount your app, to the closing of the page.  
For each lifecycle there is a "hook" (a function) that could be called if you need to!  
You create the function inside the object of Vue (using the correct name) and the code will be executed automatically.

### beforeCreate()

This is the first stop of the lifecycle, this method will be called before your app is even created (instantiated).

### created()

This method is called just after the app is instantiated! At this point we still have nothing on the screen, after created, Vue just knows its data properties, and it is aware of the general app configuration.

### beforeMount()

The template is compiled! All the dynamic placeholders, all the interpolations, and so on are removed, and replaced with concrete values. And we arrive at the `beforeMount()` function.  
Everything is translated, in the virtual DOM, but the swap is not already done. So right before we can see something on the screen.

### mounted()

Now the page is rendered on the real DOM. The mounted function is called.

## SOME CHANGES IN THE PAGE HAPPENS

When data change, a new life cycle is trigged.  
We have some changes to show in our page, so :

###   
beforeUpdate()

This is like `beforeCreate()`, the code gets executed before the change has been fully processed internally by Vue!

### updated()

This is triggered once the change has been processed. This means that the update effects are visible on the screen.  
We don't reach `mounted()` because the app was never "unmounted'.  
But sometimes the unmount action is needed...

### beforeUnmount()

When the app is unmounted, all its content is removed and the app comes to an end.  
This function is called before the unmount function is launched.

### unmounted()

This is called after the unmount action is executed. This could be used to do some cleanup.

###
