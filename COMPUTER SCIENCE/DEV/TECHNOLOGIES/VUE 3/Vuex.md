---
title: "VUEX"
date: "2022-08-27"
---

## WHAT IS VUEX, AND WHY IS IT USEFUL?

Vuex is a state manager, what is a state manager? To better understand what it is, it is better to understand what problem it solves.  
One of the big problems, when you have a big application, is where you store all the important data of your app, like your cart, your personal info, and more. Those things should be available everywhere in your app since if you change something, you will have some reacting stuff that should go on.

So you can group all important info into a big object that every component of your application can use, and modify. And this is pretty much what a Vuex does. But why is called a "state" manager? The idea behind it is that when the data in your application change, the "state" of your application is not the same, so you are in another state. Vuex is a state manager, it manages the changes of state and therefore the data of your app.

## INSTALL VUEX

```
npm install --save vuex
npm install --save vuex@next (latest version) (not sure)
```

## ONE STORE PER APP

You can have one store per application.

## LOCAL STATE VS GLOBAL STATE

Having a state manager does not mean that ALL your data represent the state of your app. There is a big difference within a variable used to signify that you have clicked on a button and the list of your cart. Those infos don't have the same weight. Local state usually affect one component, while global affect all the app. This distinction is important! Do not put local state in a global state manager!

## BASIC USE OF VUEX

```
//main.js
const store = createStore({
  state() {
    return {
      counter: 0,
    };
import { createApp } from 'vue';
import { createStore } from 'vuex';

import App from './App.vue';

const store = createStore({
  state() {
    return {
      counter: 0,
    };
  },
});

const app = createApp(App);

app.use(store);

app.mount('#app');

```

This is how you create a basic store, after importing the creating function from the module `vuex`.

Now you store the store (got it? :D ) by calling the `createStore` function in which you pass an object that will contain the data.  
Like the `data()` function, here you have the `store()` function, and you return the data you want with it. Let's see how you use it in a component.

```
//App.vue
<template>
  <base-container title="Vuex"></base-container>
  <h3>{{$store.state.counter}}</h3>
  <button @click="addOne()">ADD 1</button>
</template>

<script>
import BaseContainer from './components/BaseContainer.vue';

export default {
  components: {
    BaseContainer,
  },
  methods:{
    addOne(){
      this.$store.state.counter++;
    }
  }
};
```

As you can see, you have now access to another variable: `$store`. This variable will have a state variable inside, and inside the state variable, you have our counter.

Right now we have the method and the display of the counter on the same component, but you could use the same technique on another component it will do the same thing. Because you change one obj, the state obj. You don't need to add props or inject them.

## MUTATIONS - A Better way of changing data

Updating the store from inside a component to see the reacting differences in other components is cool. But is this really the best way of doing so?  
If you want to add one to your counter, and you want to take this action in a different part of your app, you will have to rewrite the `addOne` function in each component you want to fire it! And that is not good. Also, it is more prone to error, because if you want to change the logic of an action (`addOne` also log something now) you will have to change the code inside all the components you have made. Not good at all.

In Vuex we have this concept: mutations! A mutation is like `methods` in a normal Vue object. Those are functions that you use to mutate the state, and you can call them from wherever in your app. So you don't have to modify all the code from everywhere when you want to change something.  
In other words, mutations can be considered as setters++!

```
//main.js
const store = createStore({
  state() {
    return {
      counter: 0,
    };
  },
  mutations: {
    addOne(state) { //This is our mutation function, you need to pass the state object
      state.counter++;
    },
  },
});
...
```

```
//App.vue
//...
export default {
  components: {
    BaseContainer,
  },
  methods:{
    addOne(){
      this.$store.commit('addOne'); //You commit the mutation you want to call.
    }
  }
};

```

Note that :

You have to pass the state attribute to the mutation you make!  
You dont call the function directly but you use `commit('mutationName')` to call the mutation.

## PASSING DATA TO MUTATIONS

Do you really believe that mutations are functions, and you can not pass arguments? LoL  
Of course, you can, but you can only pass one single argument, so let it be valuable (like an object):

```
mutations: {
    addOne(state, payload) {
      state.counter += payload.value;
    },
  },
```

```
//Verions 1
methods:{
    addOne(){
      this.$store.commit('addOne',{value:10});
    }
  }
```

```
// Version 2
 methods:{
    addOne(){
      this.$store.commit({
        type:'addOne',
        value:10
      });
    }
  }
```

You can use one of those two versions, or pass as the first argument the name of the mutation, put everything in an object, and add the name of the mutation you want to call into a `type` variable.

## GETTERS

If mutations are like setters, of course you can also have getters, with them, you can centralize the logic of getting the value:

```
//main.js
getters: {
    finalCounter(state) {
      let counter = state.counter;
      if (counter > 100) {
        return 100;
      }
      return counter;
    },
  },
```

```
App.vue
<h3>{{$store.getters.finalCounter}}</h3> //We use $store.getters!
<button @click="addOne()">ADD 1</button>
```

As you can see, using getters let you decide how you want to return things. In this case even if the number if more than 100 it will return only 100. 
You use `$store.getters` to access the getters.

## CALLING A GETTER FROM A GETTER

You could need to access another getter while writing another getter (like name and surname), well you can do that in this way:

```
main.js  
  state() {
    return {

      name: 'simone',
      familyname: 'panebianco',
    };
  },

  getters: {
    getName(state) {
      return state.name.charAt(0).toUpperCase() + state.name.slice(1);
    },
    getFamilyName(state) {
      return (
        state.familyname.charAt(0).toUpperCase() + state.familyname.slice(1)
      );
    },
    getFullName(_, getters) { //The second parameter is getters
      return getters.getFamilyName + ' ' + getters.getName;
    },
```

As you can see, in the last getter, you can pass a second parameter, that is the list of your getters! You then can access those and use them.  
Note that since we don't need to use state, we call it underscore `_` so that view understand that we will not use it and will not throw a warning.

## ACTIONS

Time to get some action! No, is not a movie, is just the asynchronous code of your app :D

In Vuex, it is not a good practice using asynchronous code in mutations! Why? Mutations only care about actually changing the state and should be always sync. So you can always compare the before/after states of a mutation once it's called. Also, you can call more than one mutation in one action, and you will be sure that all mutations are called in the right order.

But don't worry, actions are very similar to mutations. You call them in the same way, you can have a second argument payload etc:

```
//main.js
 mutations: {
    addOne(state, payload) {
      console.log(payload.value);
      state.counter += payload.value;
    },
  },
  actions: {
    addOne(context, payload) {
      console.log('Add One action called');
      setTimeout(() => {
        context.commit('addOne', payload);
      }, 1000);
    },
  },
```

```
//App.vue
methods:{
    addOne(){
      this.$store.dispatch({
        type:'addOne',
        value:10
      });
    }
  },
```

So:

- The name of the action can be exactly the same as the mutation if you want!
- Instead of `state`, it is conventional to call `context` the first parameter
- You access the `mutation` the same way you do in the component, using `commit()`
- From the component you use `$store.dispatch()` and you can add a payload if you want.

As you can see, there is no really a big difference with mutations, but it is better now because you can have asyn code now.

**IT IS GOOD PRACTICE TO ALWAYS HAVE AN ACTION BETWEEN A COMPONENT AND A MUTATION**

## CALLING AN ACTION FROM AN ACTION

It is possible to call an action from an action, for example a HTTP request gave us some error, so you want to call another error.  
You can do that from the context itself :

```
actions: {
    addOne(context, payload) {
      console.log('Add One action called');
      setTimeout(() => {
		addOne(context, payload) {
		    setTimeout(() => {
				context.dispatch('logTheThing', payload);
			    context.commit('addOne', payload);
		    }, 1000);
		},
	    logTheThing(_, payload) {
	    console.log('logTheThing called with a payload', payload);
    },

```

## MORE ON CONTEXT

What can we access from the context? Let's try `console.log` it:

![](image.png)

Well, we can access mutations via commit, we can call actions via dispatch, we can have getters if we want, or access the state directly (not a good idea to modify it directly tho)

## YOU GOT THEM ALL

With STATE, MUTATIONS, ACTION, GETTERS we got all the core concepts that vue uses :D

## MAPPERS - mapGetters()

Now let's check some utility function that make create code faster! You can have a mapper that let transform this :

```
computed:{
    firstGetter(){
      return this.$store.getters.firstValue;
    },
    secondGetter(){
       return this.$store.getters.secondValue;
    },
    thirdGetter(){
       return this.$store.getters.thirdValue;
    }
  }
```

Into this:

```
import { mapGetters } from 'vuex';
//...
//V1
computed:{
    ...mapGetters(['firstGetter','secondGetter',' thirdGetter'])
  }

//V2 Let you rename the getter as you want.
computed:{
    ...mapGetters({
       first:'firstGetter',
       second:'secondGetter',
       third: 'thirdGetter',
})
  }
```

`mapGetters()` is a function that you get from `vuex` that give you an object that contains all the getters you want (you specify their names in an array), and let you just use the getter name in the component, instead of connecting them manually like in the first example. Using the three dots `...` let you combine the computed object with the one that came from the mapper.

## mapActions()

The same way you can map actions if you want:

```
<button @click="addOne({value:1})">Add One</button> //Here you pass your payload

//V1
methods:{
  ...mapActions(['addOne'])
  }

//V2 Let's you rename the action.
methods:{
  ....mapActions({
  newFunctionName:'ActionName' //Like 'addOne 
});
```

## ORGANIZE YOUR STORE IN MODULES

Let's talk real, if you can have a single store in your app, and your app in pretty big, will you get all your store in a single file? in an enormous, super long file? I don't think this is a good idea.

Vuex help you organize your store in a very clever way, you don't just have a file for all your mutation and one for all actions, no no, you can organize them by logic! You can create an object that as store, mutations, actions and getters that are all about one feature (AUTH for example) and plug it in the main store!

Here is an example :

```
//counterStore.js
export default {
  state() {
    return {
      _counter: 0, //The underscore is my personal solution
    };
  },
  getters: {
    counter(state) {
      if (state._counter > 100) {
        return 100;
      }
      return state._counter;
    },
  },
  mutations: {
    addValue(state, value) {
      console.log('action called');
      state._counter += value;
    },
  },
  actions: {
    addValue(context, value) {
      context.commit('addValue', value);
    },
  },
};
```

```
//main.js
import counterStore from './stores/counterStore';

const store = createStore({
  modules: {
    counterStore,
  },
});
```

`main.js` contains the global store (more on that later), and you can add module store using the module attribute! Vuex will merge anything together.

Now the app will work as it should be.

## GLOBAL STATE VS LOCAL STATE

We have said that module stores are merged with the global store (the root store), well that is correct but... That is correct only for mutations, actions and getters, the scope of the state is available only in the module you have created. You can not access the state from the root store, you can not access the state from a sibling store! But you can access, actions, getters and mutations (those are global).

Inside your module state, you don't have access even of the root state and vice versa! But there is a way to gain this access:

Sometimes you may really need access to the root state, well you can use `rootState` of the context :

```
getters:
  myGetterFunction(state,getters,rootState,rootGetters){ ...
```

## NAMESPACING MODULES

namespacing is the practice of creating spaces for names, in order to avoid name conflicts (like in php).

Since, actions, mutations and getters are all shared globally, it is possible to overwrite one of them in a big application, to avoid this you can add namespace in your module, but not without consequences:

```
export default {
  namespaced: true, //you set namespaced as true.
  state() {
    return {
//...
```

Unfortunately, if you want to use namespaces you have to change your code (that will not longer work).

The first thing to notice is that you are not specifying a space name here, you are just activating it. How you specify a spacename? You have already done when you load the store in the root store :

```
modules: {
    counterStore,
},

//This is the same as doing this:
modules: {
    counterStore:counterStore
},


```

You already gave a spacename in there, but you never used it because you have never activated it. Now how to use it ?

```
computed:{

     counter(){
         return this.$store.getters['counterStore/counter']
     }
}
//using mapGetters
computed:{
   ...mapGetters('counterStore',['counter'])
}
```

Same for `dispatch`, `mapActions` etc.

## GET ORGANIZED!

Ok, what is the best way to organize the store? How to subdivide files?

![](image-1.png)

One common way to do it is to have a store folder, `index.js` represent the root store, while you can import the rest (actions, getters and mutations) from the other files:

```
//index.js
import actions from './actions';

import getters from './getters';

import mutations from './mutations';



const store = {

  state() {

    return {};

  },

  actions: actions,

  mutations: mutations,

  getters: getters,

};



export default store;


```

```
//actions.js
const actions = {};

export default actions;
```

And if you have a sub-store (for a component, for example), you do the same in a subfolder. But this is overkill for little applications.
