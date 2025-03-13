---
title: "VUE 3 CLI"
date: "2021-12-28"
---

In this part, we will discuss what is the Vue CLI, why you may need the extra tools, and why we need a better setup for our big project.  
And of course how the CLI work.

## Why we may need a better setup for our project ?

The main advantage of using the Vue CLI is that, it will build your project for a test every time you hit save. Since we will use `.vue` files this is a real gain of time while developing.

Another advantage is that you can have a proper development web server.  
Why? If you don't have a web server to test your application, you just double-click your `.html` file and open it in the browser; And yes this works to test minor things, but since we use the FILE protocol (and not the HTTP(S) one) we miss out a lot of stuff (some JS will not work with the file protocol).

**Reloading the page**

Having a web server that is created for Vue.js development also has its advantage. You will no longer need to reload the page to see the differences that you have done on your page... I'm not talking about "automatic reload" of the page, I'm talking about the fact that the JS code modified is updated in your page live; This means that you will not lose your page STATE.

**Better IDE support**

With Vue CLI you will have better IDE support, better autocompletion when developing, and also warning and errors while coding.

**File Importing**

If you have a big application, and you are not working with one giant HTML and JS file (as a mentally stable person), you will need to manage all the imports of your files manually. And for multiple HTML files, it wouldn't even be that easy because you will lose the state of all the others changes you have done in your application.

## INSTALLING THE CLI

In order to use the Vue CLI, you need to have Node.js installed because it is used under the hood.  
And of course with Node.js comes also "npm" (node package manager).

In a terminal you can type the installing command.

```
npm install -g @vue/cli
```

The `-g` parameter make sure that you will install this globally.

Now that the CLI is installed, you can run :

```
vue create name-of-your-app
```

Vue CLI asks you what you want to install (vue2 or 3) I personally want to install things manually, so I select manually!

For the moment, just select the defaults values, and then if you want Vue 2 or 3 (of course 3)

Next, it will ask you all the information necessary for the configuration that you have chosen;

Now the CLI will install automatically "init" the project, and install all dependencies

Next, you can `cd name-of-your-app` && `npm run serve`

## THE CREATED PROJECT

Let's analyze what's inside the created folder :

![](images/image-1.png)

So, on the root level we have a bunch of configuration files : unless you know what are you doing, you can leave the defaults there.

The public folder will contain your app; for now, there is only an icon and an index.html file that will be the entry point of the application.

The src (source) folder will contain all the JS we are going to write; In `assets` you will find all the other files needed (images and other things), components contain all the components of your app (in the `.vue` format), `App.vue` is the main Vue file of your application, while `main.js` is where the `app.mount` code exists

## WHAT IS A .VUE FILE?

if you look inside the `main.js` file you will see this code:

```
//main.js
import { createApp } from 'vue'
import App from './App.vue'

createApp(App).mount('#app')
```

Other than the import statement, you can see that we are not passing an object to Vue before mounting, but we pass what we have imported from the `App.vue` file. But what is exactly this file? What does it contain?

```
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

`.vue` files are specials Vue.js feature, or if you want "vue-cli" project feature. `.vue` file allows us to write Vue apps and, to be precise, Vue components in a much nicer way. They allow us to split the component in : template (the HTML code that belongs to a component or an app), the script, and the style.

This file would not work in a browser, but thanks to the Vue CLI, the file is transformed behind the scenes to a code that works on a browser.

To make it work, by the way, we need to use the Building Workflow.

## THE BUILDING WORKFLOW

Using the Vue CLI, we create a project that uses a building workflow, which means that the code that we create is not the code that runs in the browser, because it needs to be converted, it needs to be built.

What are the benefits of this way to code? Well, for example, you can use babel, a tool that converts all your brand-new JS code ES17 to its basic form ES1, so that also old browsers can run and see your website.

The other benefit of course is that you can write `.vue` files that will be built into the public folder.

If you look inside package.json you will see the script to build your app : `npm run build`

## VS CODE EXTENSION

In vs code search for "`vetur`" plugin, now your `.vue` file will have highlighting; Also it helps with suggestions and errors.  
This plug-in is a must for developing VUE code.
