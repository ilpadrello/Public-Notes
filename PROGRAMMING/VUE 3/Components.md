---
title: "VUE 3 COMPONENTS"
date: "2021-12-28"
---

Components are a system to separate and enclose part of your application.  
It's also good to build your application with "modules" (components) that you can reuse.  
A component is like a custom HTML element / tag  
But it is also like just another app, that belongs to another app, you could say is a mini-app.  
So they can have data, they can have methods... but they are not "mounted" as the main app.

Let's create a component.

```
//HTML
<div id="app">
   <test></test>
</div>

//JS
app.component("test", {
  template: `
    <button type='button' @click="showAllacatalla()"> CLICK ME </button>
    `,
  data() {
    return {
      allacatalla: "allacatalla",
    };
  },
  methods: {
    showAllacatalla() {
      alert(this.allacatalla);
    },
  },
});
```

So the first thing to notice is the way we show the component on the page. Since a component is a sub-app of your main app, you should put it inside the HTML that is handled by the main app. And now you can just put a new, never-existed-before tag called `<test>` !  
This is very important to understand, because, since this is the way you show your component, you don't want to use an existing HTML tag name as your component name. That would be bad. A rule of thumb could be to use always at least "twodashed-words" for your name.

Next, since you don't mount your component in your app (using the mount function), you need a template (or an HTML code) to render when de component is called. For now, we are using the template parameter of the Vue object, but we will see how to make this part differently.

For now, also remember that the component can't access the father object's data, we will see how to transfer data later.

Do not forget that you have to mount the main app AFTER the components are created! If you do something like this :

```
const app = Vue.createApp({...datas}).mount('#id'); // This will mount
app.component({...datas}); //This will not work
```

This will not work, **you need to declare all components before mounting the main app**.

## WORKING WITH THE CLI AND .VUE FILES

Before continuing this page, I suggest you check the explanation of why ? How? What? CLI is ... right here: [http://www.simonepanebianco.fr/vue-3-cli/](http://www.simonepanebianco.fr/vue-3-cli/)

To work with components, we will use the import-export modules system of node, if you need a refrehs : [http://www.simonepanebianco.fr/js-import-export/](http://www.simonepanebianco.fr/js-import-export/)

Since we use the CLI, we can put components in `.vue` files. First use the CLI of Vue to create a new project (as explained in the link) and now we can analyze the code that is created:

You will have a public folder, and a source (src) folder, in the public folder there will be the index.html (the entry point of your app).

Inside the source folder, you will have a `main.js` (that is called by the index.html), a `App.vue` file that contains the trio (HTML JS CSS) for your index.html template, and a folder called `components` (where of course you will store all your components). Let's have a look at what is inside the `index.html`, `main.js`, and `app.vue`.

```
//HTML
...
<div id="app"></div>
...
```

```
//main.js
import { createApp } from "vue";
import App from "./App.vue";
createApp(App).mount("#app");
```

```
//App.vue
<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App"/>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

The first thing that I have noticed is that in the index.html there is no `<script>` tag to import the `main.js` file.  
Even tho, if you run the server, the code is loaded on the index page? Why so? I'm not sure, but I think that when you make some modifications to your page and save, the server "build" the app, and make it ready to test it out. And so the code is added to the index.html. I have to verify that.  
In fact, if I change the name of `main.js` we got immediately an error:

![](images/image-2-1024x129.png)

I'm pretty sure that is like I said, or at least you have to configure the name of `main.js` somewhere

Next, if we take a look inside, `main.js` we see that `App.js` is imported (as App) using the import command, and inside the App.vue there is `export default` of the object, you want to pass to the `main.js`

But it doesn't end there, inside `App.vue` there is also the code to render the component `HelloWorld.vue`, you find the import statement and also inside the template, the tag to print that.

## PascalCase FOR COMPONENTS NAMES

It's common to call components with Pascal Case, which means the first letter of each word in capital. No dashes (-) or underscores ( \_ ), and each name is composed at least of 2 words: this way you can not clash any HTML original tag.

## PASS DATA TO A COMPONENT

[https://vuejs.org/v2/guide/components-props.html](https://vuejs.org/v2/guide/components-props.html)

Often a component is a structured empty shell! It has an HTML structure and logic, but the data to work with are not declared inside the component, so the component could work with any data you pass in.  
Let's imagine you want to make a specific component for a title (for whatever reason), you will not create a component for each title of your page, you will create a template of your title, and pass the title content as a variable inside the component.

The data you pass in are called `props`. Let's create this new component :

```
//TitleComponent.vue
<template>
    <h2 @click="alertTitle">{{ titleValue }} </h2>
</template>

<script>
export default {
    name:"TitleComponent",
    props:['titleValue'],
    methods:{
        alertTitle(){
            alert(this.titleValue);
        }
    }
}
</script>

<style scoped>
    h2 {
        background-color: red;
    }
</style>
```

```
//App.vue
<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App"/>
  <TitleComponent title-value="This is a title"></TitleComponent>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import TitleComponent from './components/TitleComponent.vue'

export default {
  name: 'App',
  components: {
    HelloWorld,
    TitleComponent
  }
}
</script>
```

We create a new component called `TitleComponent`, inside the object of this component, there is a specific parameter( `props:['title']`) to make vue.js understand that we are waiting for this data to be passed.  
For now, you can just use an array with the name of the data that will be passed from outside.

To pass this information (title) while outside the component, you just add it to the "tag" as attributes :

```
<TitleComponent title-value="This is a title"></TitleComponent>
```

**Note** that the attribute name is `title-value` while in the component we recall it as `titleValue`, that is because Vue automatically makes the conversion. It is better to use dashed style while writing on HTML and you need conversion in JS because dash means subtraction.

It is also worth it to mention that the style inside the component, since there is the `scoped` keyword, will be executed only for that component! No other elements in the page outside the component will be affected by this style.

Also, you need to declare that you are using this new component inside the `App.vue` :

```
...
  components: {
    HelloWorld,
    TitleComponent
  }
....
```

## NOT MUTABLE PROPS

This is one important thing about props that you have to learn... **PROPS SHOULD NOT BE MUTATED**. This is very important, all the props that you pass inside the component should state the same and never mutate; Let's try to mutate one.

```
<template>
    <h2 @click="changeTitle">{{ titleValue }} </h2>
</template>

<script>
export default {
    name:"TitleComponent",
    props:['titleValue'],
    methods:{
        changeTitle(){
            this.titleValue = "New Value";
        }
    }
}
</script>
```

this way, we change the `titleValue` with a custom method of our object, if I press save :

![](images/image-3-1024x233.png)

As you can see, we can not mutate this, we can not even try :D! We can not mutate the value of our props... from inside the component at least; That is because, all communication between Parent and child are like the one in the real life: Parent talk, child listen (poor child :D ), if parent say something, the child can not change that statement! It's a one way communication. What if we change the value from outside? Before trying that, as every child knows, there is always a way to manipulate in our favor what a parent said: we can stock the communication inside an internal variable, and change the internal variable. Look :

```
<template>
    <h2 @click="changeTitle">{{ title }} </h2>
</template>

<script>
export default {
    name:"TitleComponent",
    props:['titleValue'],
    data(){
        return {
            title: this.titleValue
        }
    },
    methods:{
        changeTitle(){
            this.title = "New Title from inside";
        }
    }
}
</script>
```

Using this shortcut, we can effectively change the value of our title inside the component itself, but doing that we store only the initial value of the props, if that value changes that would not affect our component because we have stored only the initial value...  
But is it possible to change the value from the outside?

```
//App.vue
...
<button @click="changeTitle"> CHANGE VALUE </button>
<TitleComponent :title-value="title"></TitleComponent>
...

data(){
    return {title:"This is the Old Title"}
  },
  methods:{
    changeTitle(){
      this.title = "This is the new Title"
  }
}
...

//TitleComponent.vue
<template>
    <h2>{{ titleValue }} </h2>
</template>

<script>
export default {
    name:"TitleComponent",
    props:['titleValue'],
}
</script>
```

Like this, we can communicate changes to the child component; We use the variable `title` to pass the title value to the component and then with a button we change that value when we want it. To make understand Vue that we want to pass a variable inside `title-value` we prefix it with `:` and so we have `:title-value`. Without that, Vue won't understand that we want to use a variable.

Note that, since we don't store only the initial value (as we did before) we can change the value of title, but if we want to use that way to modify stuff, well you can not receive communication from outside anymore.

## VALIDATING PROPS

To make Vue understand that you want some props inside your component, you add to the object of your component:

```
props:['titleValue'],
```

but there is also another way, a way in which you can specify what kind of data should be passed, and have a "validation" :

```
props:{
        titleValue: String
},
```

But we can go even deeper than that, here some example:

```
props: {
     // Basic type check (`null` and `undefined` values will pass any type validation)
     propA: Number,
     // Multiple possible types
     propB: [String, Number],
     // Required string
     propC: {
       type: String,
       required: true
     },
     // Number with a default value
     propD: {
       type: Number,
       default: 100
     },
     // Object with a default value
     propE: {
       type: Object,
       // Object or array defaults must be returned from
       // a factory function
       default: function () {
         return { message: 'hello' }
       }
     },
     // Custom validator function
     propF: {
       validator: function (value) {
         // The value must match one of these strings
         return ['success', 'warning', 'danger'].indexOf(value) !== -1
       }
     }
}
```

When prop validation fails, Vue will produce a console warning (if using the development build).

### [Type Checks](https://vuejs.org/v2/guide/components-props.html#Type-Checks)

The `type` can be one of the following native constructors:

- String
- Number
- Boolean
- Array
- Object
- Date
- Function
- Symbol

In addition, `type` can also be a custom constructor function and the assertion will be made with an `instanceof` check. For example, given the following constructor function exists:

```
function Person (firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName
}
```

You could use:

```
Vue.component('blog-post', {
  props: {
    author: Person
  }
})
```

to validate that the value of the `author` prop was created with `new Person`

## SENDING EVENT FROM CHILD TO PARENT

Even if you can't modify the value of props from inside the component himself, if you want to modify something from the click of a button you could do so using events...  
JS event are a very powerful tool that should always be used :D

```
//TitleComponent.vue
...
<button @click="changeThisTitle"> CHANGE VALUE </button>
    <h2>{{ titleValue }} </h2>
...
    emits:['change-title'],
    methods:{
        changeThisTitle(){
            console.log('Emmitting event');
            this.$emit('change-title');
        }
    },
...

//App.vue
<template>
<TitleComponent
  @change-title="changeTitle" 
  :title-value="title"></TitleComponent>
</template>
...

methods:{
    changeTitle(){
      this.title = "This is the new Title"
    }
  },
....
```

In order to emit a custom event from the component (child) to the parent, you need to use the internal Vue.js keyword `$emit` inside the component (when you want to emit the event of course), now you listen to this event in the `App.vue` using `@event-name` syntax, and now you can attach this to the code that you want to execute.

Note that it is also good practice to add `emits:['change-title']` inside your component, otherwise, you will have a Vue warning in your console.

Remember that all events in Vue.js must be in kebab-case.

## PASSING VARIABLES WHEN EMITTING EVENT

Of course, it is also possible to pass arguments to the event:

```
//TitleComponent
changeThisTitle(){
            console.log('Emmitting event');
            this.$emit('change-title','This Title Name Comes From The Component');
        }

//App.vue
changeTitle(title){
      this.title = title;
    }
```

The first argument inside the `$emit` function is always the name of the event, but starting to the second parameter you can pass as many parameters as you want, they will be passed to the function you bind in the parent.

## EMITS, AND VALIDATING EVENTS

There are more than one reason to add `emits:` in the component if you are thinking of using events to communicate with the parent component. The easiest to understand: that array makes the component way more understandable, since you put every event listener name in one place, you don't have to look for them inside the code. They are listed there, and you can easily know how many events are there and which ones.

But it doesn't have to be an array, you can also pass an object :

```
emits:{
        'change-title':function(title){
            console.log("checking if this is called and when");
            if(title){
                console.log(title);
                return true;
            } 
            return false;
        }
    }
```

Passing an object you **must** use the name of the event you are passing, and it **must** return a function:  
you can then validate the value of what is passed before it is passed.

If the value doesn't pass the validation, using false the event still fired, but you could warn the user with a `console.warn`.

Let try something here, can we modify the value of the parameters we are passing? No, and Yes, if you pass a variable for example, well the function will not change the variable since it will use its original value, but if you pass an object as an argument, since the object is passed as a reference, well it will change the value.

```
 emits:{
        'change-title':function(title,obj){
            console.log("checking if this is called and when");
            if(title,obj){
                title = "New Title"
                console.log(title);
                obj.value = 'secondValue';
                return true;
            } 
            return false;
        }
    },
```

Executing this code, the title will not change, but the value of `obj.value` does.

Prop / Event Fallthrough & Binding All Props

There are two advanced concepts you also should have heard about:

- **Prop Fallthrough**
- **Binding All Props on a Component**

## Prop Fallthrough

You can set props (and listen to events) on a component which you haven't registered inside of that component.

For example:

**BaseButton.vue**

```
<template>
    <button>
    <slot></slot>
  </button>
</template>
 <script>
export default {}
</script>
```

This button component (which might exist to set up a button with some default styling) **has** **no special props** that would be registered.

Yet, you can use it like this:

```
<base-button type="submit" @click="doSomething">Click me</base-button>
```

Neither the `type` prop nor a custom `click` event are defined or used in the `BaseButton` component.

**Yet, this code would work.**

Because Vue has built-in support for **prop (and event) "fallthrough"**.

Props and events added on a custom component tag **automatically fall through** to the **root component** in the template of that component. In the above example, `type` and `@click` get added to the `<button>` in the `BaseButton` component.

You can get access to these fallthrough props on a built-in `$attrs` property (e.g. `this.$attrs`).

This can be handy to build "utility" or pure presentational components where you don't want to define all props and events individually.

**You'll see this in action** the component course project ("Learning Resources App") later.

You can learn more about this behavior here: [https://v3.vuejs.org/guide/component-attrs.html](https://v3.vuejs.org/guide/component-attrs.html)

#### Binding all Props

There is another built-in feature/ behavior related to props.

If you have this component:

**UserData.vue**

```
<template>
  <h2>{‌{ firstname }} {‌{ lastname }}</h2>
</template> 
<script>
  export default {    props: ['firstname', 'lastname']  }
</script>
```

You **could** use it like this:

```
<template>
  <user-data :firstname="person.firstname" :lastname="person.lastname"></user-data></template>
 <script>
  export default {
    data() {
      return {
        person: { firstname: 'Max', lastname: 'Schwarz' }
      };
    }
  }
</script>
```

But if you have an object which holds the props you want to set as properties, you can also **shorten the code a bit**:

```
<template>
  <user-data v-bind="person"></user-data>
</template> 
<script>
  export default {
    data() {
      return {
        person: { firstname: 'Max', lastname: 'Schwarz' }
      };
    }
  }
</script>
```

With `v-bind="person"` you **pass all key-value pairs** inside of `person` **as props** to the component. That of course requires `person` to be a **JavaScript object**.

This is **purely optional** but it's a little convenience feature that could be helpful.

## PROVIDE AND INJECT

There are sometimes some scenarios where you pass props and get back events to/from a child component that bridges those with another child component! You could do this by simply catching those manually with `props:` and adding the differents `@event=` to return the event. But there is a simple way to do so if you need it: `provide:` and `inject:` :

Reading this sentence after a while, I can see that it is not clear at all. I'll try to explain better: When you want to pass data from a parent to a child, it is easy using props, but if you want to pass data from parent to a grand grand grand grand grand grand son... Well that is of course possible, but also tedious, cause you have for each step, take all the props and manually pass them to the child. This is very time-consuming and prone to error, so using provide and injections you can skip all those steps. Provide from the Ancestor, and inject to the nth child.  
This works only in a Ancestor -> Child relationship, not in brotherhood, so pay attention to that.

[https://v3.vuejs.org/guide/component-provide-inject.html](https://v3.vuejs.org/guide/component-provide-inject.html)

Using provide, you can provide the function that will be executed when the event is called.

Also, remember that if you need reactivity for the data you provide, you should use one of this two :

// This goes to the parent, like the data().
// Also using this syntax, you make sure you can use data that are on your component.
`provide() {`
     `return {`
       `todoLength: this.todos.length`
     `}`
   `},`

This lets you access the data in your object, Vue.js doesn't automatically convert `this` in provide.  
Also, this should give you access to some reactivity, but not in this case, for this you need to :

```
provide() {
```

**You can also pass functions using this method, this way, you can fire a function or an event form down the chain but being executed at top level. That is a good thing.**

## PROVIDE/INJECT VS PROPS/EVENTS

Provide and Inject are valid and valuable ways to code, but it is preferable to use normal props and events, and this is why:

- When your code grow, you will not know where your data will be used, so it could be messy.
- It is not easy to read code, and not easy to follow
- Reactivity is not easy to assure sometimes

That said, it is up to you if you want to use them or not.

## DECLARING COMPONENTS: GLOBAL/LOCAL

When developing your app, at some point you need to declare your components, you can do it in the `main.js` file and that would mean that the component will be available for all the components down the line, that is why we call those "global" components :

```
import { createApp } from "vue";
import App from "./App.vue";
import ComponentOne from "./components/ComponentOne.vue";
import ComponentTwo from "./components/ComponentTwo.vue";
import ComponentThree from "./components/ComponentThree.vue";

let app = createApp(App);
app.component("component-one", ComponentOne);
app.component("component-two", ComponentTwo);
app.component("component-three", ComponentThree);
app.mount("#app");
```

If we register the component this way, as I said, you will have all your component available everywhere in your code, you could use component-one in component-two.

Even if this means less effort while coding and less effort deciding how to organize your code, this is not the best way to register your component. Your browser would load all the code (even useless code) and this could really slow down your application. You would have an enormous list of components in one file. You will not understand where your components would be used.

A better way to implement register a component is to declare it in the component you need:

```
<script>
import HelloWorld from './components/HelloWorld.vue'
import TitleComponent from './components/TitleComponent.vue'

export default {
...
  components: {
    'HelloWorld':HelloWorld,
    'TitleComponent':TitleComponent
  }
</script>
```

When registering the components this way, you impose the use of that component inside that component/parent, the registered component is available only in that component, not even in the child components.

## WHAT ARE SLOTS?

To understand what slots are, it is better to understand what problem they solve.

Let's imagine that you have a section on a tag inside your code, that section tag should have a specific look, (like a card, or a specific shape or whatever), but its content may vary, you could put different things inside each section, a calendar, a form, a button it doesn't matter, what matter is that all sections should have all the same aspect.

You may think that you will just add the needed CSS in the `App.vue` style part, so that the CSS would be global for all the project, but that would not be good, you may also break other things. Instead what you could do, is use a slot.

A slot is an empty space that must be fulfilled with something, in our case the component that we want to use inside our slot.  
This "section" component should act as a wrapper to the component we want to add.

```
//SectionComponent.vue
<template>
    <section><slot></slot></section>
</template>

<script>
export default{
}
</script>
<style scoped>
    section{
        box-shadow: red;
        height: 400px;
        background-color: salmon;
    }
</style>


//App.vue
...
<section-component>
    <TitleComponent
    @change-title="changeTitle" 
    :title-value="title"></TitleComponent>
</section-component>
...
```

The first thing to notice is the `<slot>` tag in the `SectionComponent.vue` file, this particular tag let you reserve a place, create a slot, that you can use to put code inside it? How? Like we did in the `App.vue` file, wrapping the title component around the section component.

Usually, this kind of structure (component inside a component) would not work, but since we have added to our template the `<slot>` tag, Vue.js knows out to compile this.

This feature is very important when you think about how to organize your code.

## NAMED SLOTS

Can you put more than one slot on the same component ? Yes you can, but Vue need to separate then, that is why you should name the slots :

```
//ComponentWrapper.vue
<template>
  <div>
    <slot name="header"></slot>
    <slot></slot>
  </div>
</template>

//NewComponent.vue
<template>
  <component-wrapper>
    <template v-slot:header>
      <h2>{{ title }}</h2>
    </template>
    {{ description }}
  </component-wrapper>
</template>
```

In the component wrapper, we name one slot as a header, in the new component then, you can specify what goes in the slot named header using `v-slot:header or #header` code on a template tag. Notice that the rest of the HTML goes automatically into the second slot that has no name, and therefore will be considered as the default slot.

You can also explicitly say to Vue.js that you want the default code into the default scope like this :

```
<template v-slot:default> //defalt is a reserved name
```

Of course, you can name as many slots as you want, but if you name a slot and you don't specify the name in the `v-slot:` code, it will not be shown

## STYLING IN SLOTS

The scoped style of each element of your wrapped code affect only the tag the you can see, for example if you have this situation:

```
//ComponentWrapper.vue
<template>
  <div>
    <slot name="header"></slot>
    <slot></slot>
  </div>
</template>

<style scoped>
h2 {
  color: red;
}
</style>
```

Even if you pass an H2 element inside the slot, that would not be affected by the style, but, if you do something like this :

```
 //ComponentWrapper.vue
<template>
  <div>
    <slot name="header"></slot>
    <slot></slot>
  </div>
</template>

<style scoped>
div { //here's the change
  color: red;
}
</style>
```

That would affect everything inside that div. So, easy rule: style affects only what you can see in your `.vue` file, If you don't see it you can not style it.

## DEFAULT SLOT VALUE

You can add `Vue` code inside the `<slot>` tag. You can add what you want to be shown if nothing is passed into the slot! Of course, if you pass "something" the "default" value (the placeholder if you want) will be erased.

Also if you can use `this.$slots` code to access information about your slot, so if you dont want to render something you can do:

```
<header v-if="$slots.header">
<slot name="header"></slot>
</header>
```

This will check if we pass something for the header slot, and if that is not the case, it will not render the slot!

## SCOPED SLOTS

Scoped slots are a cool feature of slots that let you pass data from the scope to the component…  
here is more Info : [https://v3.vuejs.org/guide/component-slots.html#destructuring-slot-props](https://v3.vuejs.org/guide/component-slots.html#destructuring-slot-props)

To make it simple, it allows you to handle the data of a component (with a slot) inside the component, while the formatting is done outside the component using a slot.

I'll try to explain better: it distinguishes the data and the formatting of the component itself. Data handling is done inside the component, while using slots, the formatting is done at parent level. Let's have a look:

For this example, we have a `ScopedComponent.vue` that is loaded on the `App.vue` main component.

```
//ScopedComponent.vue
<template>
    <div  v-for="todo in todos" :key="todo">
        {{ todo.title}} <br>
        {{ todo.description }}
    </div>
</template>

<script>
export default {
    data(){
        return {
            todos:[{
                title: "Finish studing",
                description: "You need to focus and finish study Vue 3"
            },{
                title: "Buy Roses For Rosi",
                description: "Since you are studing, Rosi is taking care of the house, buy her some roses!"
            }]
        }
    }
}
</script>
```

```
//App.vue
<template>

  <scoped-component></scoped-component>
</template>

<script>


import ScopedComponent from  './components/ScopedComponent.vue'
...
```

In this case data and formatting are in the same component ! Using slots, you can move the formatting on the parent, but you will lose the data because is defined in the child component and you can't use the v-for directive.

To leave data inside the component while deciding about style outside, you can use scoped components:

```
//ScopedComponent.vue
<template>

    <div  v-for="todo in todos" :key="todo">

        <slot :todo-title="todo.title" :todo-description ="todo.description"></slot>

    </div>

</template>
... /the data is the same
```

```
//App.vue
<scoped-component>

    <template  #default = "slotProps">

      <h2>{{slotProps.todoTitle}}</h2>

      <b>{{slotProps.todoDescription}}</b>

    </template>

</scoped-component>
```

So, to pass data from the slot, to the parent, you specify what data are passed to the parent using `<slot :variable-name="data">` you can pass as many data you want, even an object that contain everything.

You need also to make some changes in the parent to make the code functional, first you need to wrap everything you want to pass inside the slot into a `template` next, in the tag itself, in the name part you specify a name for the object that will receive the data from the component, in this example I choose `slotProps`. You can access the data using this object !

Note that `:todo-title` is converted in `camelCase` automatically!

This process useful, but not very often used. But still good to know if you need it

## DYNAMIC COMPONENTS (How to switch component)

Do you want to switch component with a press of a button ? You could use v-if in each component, but Vue offer you an easy way to do it:

```
//ComponentOne.vue
<template>

    <h2>This is component 1 !!!</h2>

</template>
```

```
//ComponentTwo.vue
<template>
    <h2>This is component 2 !!!</h2>
</template>
```

```
//App.vue
<template>
  <button @click="changeComponent('component-one')">Component 1</button>

  <button @click="changeComponent('component-two')">Component 2</button>

  <component :is="selectedComponent">
....
<script>
import ComponentOne from './components/ComponentOne.vue'

import ComponentTwo from './components/ComponentTwo.vue'

export default {

  name: 'App',

  data(){

    return {


      selectedComponent: "component-one"

    }

  },
  components: {


    ComponentOne,

    ComponentTwo,

  },
  methods:{


    changeComponent(cmp){

      this.selectedComponent = cmp

    }

  }
```

This is pretty much self-explanatory ! You "save" witch component you want to show in a variable `selectedComponent`, then you use what ever method you like to change component, in this case the press of a button, but this could be an event or the result of an external query, a function, or what ever you want. Changing the variable will automatically change what component is shown.

But what happens to the others components ? The component that is not active, is destroyed from the DOM! And that means that if we put some infos in a form inside the component, that would be lost!

To ensure that the component is not destroyed but just hide, and you will retreive all the data that you may have modified in its DOM, you need to wrap the `<component>` inside a `<keep-alive>` tag !

```
...
<keep-alive> 
<component :is="selectedComponent">
</keep-alive>
...
```

## TELEPORTING COMPONENTS

Vue has a feature, that let you teleport part of the code inside your component, wherever you want in your page.

```
<template>

    <teleport to='body'> //Use a CSS Selector

    <h2>This is component 1 !!!</h2>

    </teleport>

</template>
```

The first question is WHY ? Let's imagine that you are creating an alert controller, that pop up an alert for whatever reason, if you put the component inside another component you may lose some CSS or some JS, so if you want to teleport part of your code you can! Use a CSS selector to specify where should your code go.
