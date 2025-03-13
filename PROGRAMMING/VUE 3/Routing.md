---
title: "VUE ROUTING"
date: "2022-08-24"
---

First of all, just remember that you don't have to build SPA (Single Page Applications), you can have multiple HTML files if it fits better in your project. That sais

Vue routing lets you convert what is in your URL into specific pages of your application.

## INSTALLING VUE ROUTING

Routing doesn't come with Vue directly, it is a package you want to install:

```
npm install --save vue-router
```

## BASIC ROUTER

On your `main.js` you can register your routes :

```
import { createApp } from 'vue';
import { createRouter } from 'vue-router';

import App from './App.vue';

const router = createRouter({}); //This obj will contains routes info

const app = createApp(App);

app.mount('#app');

```

Not let's configure our router:

```
const router = createRouter({

  routes: [],
});
```

The configuration object of our router accepts different parameters, one of them is of course routes, that will contain all the routes we want to use.

But there are other configurations that we can use too, like history (let's pause for a sec and have a look)

## HISTORY

The history option specifies how to manage the "history" in the app. For example, when you press the "go back" button on your browser.  
There are two different kinds of history that you can use here: Browser Memory and Emulated Browser Memory.

Historically, JavaScript was not able to manipulate the "memory" of the user browser actions (going back page, etc.), and the router had to emulate this behavior, and we didn't use the browser history. But since a couple of years there is no problem, and using the `createWebHistory` will use the browser memory

This is `createWebHistory` function:

```
//...
import { createRouter, createWebHistory } from 'vue-router';

//...
const router = createRouter({
  history: createWebHistory(),
  routes: [],
});
```

## CONFIGURING ROUTES

Of course routes are objects in our configuration:

```
const router = createRouter({
  history: createWebHistory(),
  routes: [{
      path:'/route',
      component: TheComponent
  }],
});
```

`Path`: will indicate for what URL we want to show the component, and `component` will specify the component you want to show (do not forget to import it)

Remember that the order of your routes is very important. Catchall routes for example should be at the very end.

Now, we need to add a piece to this puzzle, first, we need to attach the router to the app :

```
//...
const app = createApp(App);
app.use(router);
```

`app.use()` is a function to connect externals plugins, and in this case, we added our router.

Ok, but, what actually would happen here? Will the component chosen for the router be loaded instead of `App.vue` ?  
No, you can specify where your content will be loaded, how ? Using a specific component that comes with `router` attached to `app` :

```
<main>
  <router-view></router-view>
</main>
```

Using this component, the rest of the page will stay the same, while what's inside `router-view` will change based on the route.

## ROUTER LINK - CREATING A MENU

Now, I think you want to create a menu for your application, with all the link at the pages of your app, but it is actually better, instead of using normal links to use the component that prevents the reach for the page and will swap your page directly. This component is called `<router-link>`:

```
<li>
    <router-link to="/teams">
      Teams
    </router-link>
</li>
<li>
    <router-link to="/users">
    Users
    </router-link>
</li>
```

With the `to` attribute we specify the route, you want to go to!

Note that `<router-link>` will be translated as a `<a>` tag, and therefore if you want to style it, you can use `a {}`

## STYLING ACTIVE LINK

Also, note that two classes will be added to the `<a>` tag that you have selected : `router-link-active` and `link-exact-active`.  
The other parts of the menu will not have this class. So you can style this link based on the page you are.

What is the difference between those two classes? `router-link-active` is applied even if you are in some nested route, while `link-exact-active` is created only if the exact path is followed.

Do you know that you can change those class names? In the configuration of your router :

```
const router = createRouter({
  history: createWebHistory(),
  routes: [...],
  linkActiveClass: 'active'
});

//From now on the class added will be just active
```

## NAVIGATION VIA CODE

Do you want to change page, so go to another route via code ? you can use the `$route` special variable and `push()` the new route in the history.

```
//...
this.$router.push('/url'); //This is going to the url
this.$router.back(); //Emulate a push of the back button of the browser
this.$router.foward(); //Emulate a push of the foward button  of the browser
```

## DYNAMIC NAVIGATION

```
<router-link :to="/teams/"+id> //Binding the to attribute to id (in data)
```

You can also bind the `to` attribute of `<router-link>` so that the navigation is dynamic

## ROUTING PARAMETERS

Routing parameters in Vue are like the one in express, you set them in the route path like this :

```
{
   path: '/teams/:teamId',
   component:TeamList
}
```

Now to access this parameter, inside the component that we called (`TeamList` in this case) you can do :

```
created(){
    console.log(this.$route.params.teamId); //$route, not $router
}
```

Note that `$route` is called and not `$router` ! `$route` give you access to different parameters relative to the route you have.

Ok, now, why calling it in the `created()` function? If you try to do that before the routing plug-in runs in, you will have un undefined variable, but if you have all the code available, you can access the parameter, and that is when the component is created (more infos : [https://lavalite.org/blog/created-and-mountedin-vuejs](https://lavalite.org/blog/created-and-mountedin-vuejs) )

## ROUTER CACHING PROBLEM AND HOW TO SOLVE IT

The view router does not destroy and rebuild the components that were loaded when you navigate around. It cache it so that it is more efficent.

So let's imagine that have a site that handles different teams and members:

```
http://localhost:8080/team/t1  //t1 is the ID of the team
```

```
created(){
  //get the id from the $route
  const teamId = this.$route.params.teamId;
  //With this value we collect data from the db or in memory and pass it to the data();
}
```

Where `t1` is the ID of your team, and inside the component that you have loaded you want to put a link to the next team `t2` !  
You may think that something like this would work :

```
<template>
  //Loop the data collected and show it
  //...
  <router-link to="team/t2"></router-link> //it doesn't matter if it is not dynamic
```

And in principle this is going to work, but since, as I said, the component is the same, and it is not destroyed and recreated, the `created()` function is not called, and even if `$route.params` now have the new values, no loading of data is executed and therefore the page will show the same thing.

How do we work around this ? We can use watcher :

```
methods:{
  loadData(route){
    //get the id from the route
    const teamId = route.params.teamId;
    //With this value we collect data from the db or in memory and pass it to the data();  
  }
},
created(){
   this.loadData($this.route)
},
watch:{
  $route(newRoute){
     this.loadData(newRoute);
  }
}
```

Watching the `$route`, in case of changes (so when we have another route) the loading data is recalled and the page is updated.

## PARAMS AS PROPS

You may want to get rid of `$route.params` in your component and have props instead... why? Because, when you load the component with the route, you get the `params` via this special variable `$route`, and that means that you are forced to use the component only for routing! You can not use the same component to be loaded into another component because you will not have the special variable `$route`.

How do we fix this ? When configuring the route, you can specify that you want all the parameters as props! That simple:

```
{
   path: '/teams/:teamId',
   component:TeamList,
   props: true //here we go
}

//in the compoenent
props:['teamId'] //and now you have access to teamId
```

Now the parameters will behave like it is a prop.

This will not change the lifecycle of the component, even if you change the parameter, so the workaround we have discussed before is still needed.

## QUERIES INFO

You can have query in your URL (like `?ciao=mamma&...`) and access those query in vue:

```
this.$route.query.yourvarible
```

Note that **queries not be passed as props**, but that should not be a big problem since queries are optional by nature.

## REDIRECTING ROUTES

Do you want to redirect routes ? Easy-peasy ! Instead of specify the component, you do :

```
//...config routes
{ path: '/', redirect: '/teams' },
```

## ALIAS INSTEAD OF REDIRECTING

If you don't want a simple redirect, but you want to create aliases for your route (the URL do not change), you can use alias in the configuration :

```
{
      path: '/teams',
      component: TeamList,
      alias: ['/', '/allacatalla'],
},
```

Aliases can be a single value and an array of values.

## CATCHALL ROUTES

Do you want a 404 page ? Want to catch all routes? At the end of your routes' config, you should put :

```
{
  path: '/:notFound(.*)',
  component: NotFoundComponent,
},
```

What is `(.*)` ? It means that is a dynamic segment, and this here is a regular expression (supported by view router), that say that any character combination, should \[redirect | use specific component | etc.\]

For this reason, it is very important to put this route at the end.

## NESTING ROUTES (like iframes?)

In Vue you can nest routes, what does that mean? It is more than just nesting URLs like `/team/:teamId/user/:userId` !  
Nesting routes make you create like iframes inside your app. Let's imagine that in your app, you have a sidebar menu, and in this menu there is a group called invoices, with some submenu like, paid, pending, draft, etc. When you go to invoices general, you will be prompt with a page where on the top of the page you have a dashboard, that summarize your invoice data, and in the lowest part, you have all the invoices not categorized. By clicking in the left submenu of the group, let's say paid, the lower part of the page will load another component that will show only the paid invoices, same if you click on pending, or draft. That lower part of page have a `<router-view>` tag ! is a nested `<router-view>` if you want.

So, how do we proceed to make this beautiful tool that can help us create very interesting layouts ? The first thing to do is to nest the routes you need:

```
routes: [

    {

      path: '/teams',

      component: TeamList,

      children: [

        {

          path: ':teamId',

          component: TeamList,

        },

      ],

    },
```

Note that:

- We use `children`, to specify nested routes, that accepts other route object config.
- You dont need to rewrite the parent path in the children, `'/teams/:teamId'` become just `':teamId'`
- This route will not work on the MAIN `<router-view>` ! What does that mean ?

You already know that you have to put `<router-view`\> in your `App.vue` file if you want to access the routing space, but that is just because we attach the route directly to it with `app.use(router)`. When you're using children (or nested routes) you need to create a new `<router-view>` tag inside the component that you specify in the parent. And that is what make this powerful !

## NAMING ROUTES

Like twig, you can name your routes. That helps a lot if you have a lot of nested routes or if you want to change on route, you don't have to change every link you have created for that route. Let's have a look

```
routes: [

    {

      path: '/teams',

      component: TeamList,

      alias: ['/', '/allacatalla'],

      name: 'teams',

    },
```

```
<router-link :to="{ name: 'team', params: { teamId: 't1' }, query:{order:'asc'}}"> 
//Do not forget the :
```

Ok so, the first thing to notice is that we added `name` to the config object of our route.  
The second one is that, instead of passing just the path to the `router-link` we passed an object with more options :

- The name of the route
- Some parameter that we specify
- query parameter

This way the result path will be `/teams/t1?order=asc`

## ROUTER VIEW AT THE SAME LEVEV (Whaaaaat?)

YES, you can have more than one `<router-view>` at the same level! Why is this interesting ?  
Let's imagine that you have a specific footer for a group of pages, and another for another group of pages, those pages share the same footer and you want to specify it in the router which footer component goes in which page. Well, you can do that! Instead of using just `component` in the route parameter, use `components` with an `s` ! This one accept an object! Ok, but how we do specify what goes where? We need named `<router-view>` :

```
//We have created 2 component for the 2 footers
//App.vue
<template>

  <the-navigation></the-navigation>

  <main>

    <router-view></router-view>

  </main>

  <footer>

    <router-view name="footer"></router-view> //Named router-view

  </footer>

</template>
```

```
//main.js
routes: [

    {

      path: '/teams',

      components: {

        default: TeamList,

        footer: TeamsFooter,

      },

      alias: ['/', '/allacatalla'],

      name: 'teams',

    },

    {

      path: '/users',

      components: {

        default: UserList,

        footer: UsersFooter,

      },

    },
```

Note that:

We used components with an s at the end, and we pass an object in it. This object contains all the components that we want to use in our application.  
`default` is the unnamed `<router-view>` while we use the name of the second `<router-view>` to target our footer. The name must be the same!

## ScrollBehavior

[https://router.vuejs.org/guide/advanced/scroll-behavior.html](https://router.vuejs.org/guide/advanced/scroll-behavior.html)

## NAVIGATION GUARDS

Navigation guards are functions that are called in specific moments during navigation, and can be really useful for stop loading, or redirecting if the user is not authenticated:

```
//After creating the router
router.beforeEach((to, from, next) => {

  console.log(to, from);

  next();

});


```

This guard `beforeEach` is called before each navigation, and the callback takes 3 parameters `to` `from` and `next`.

`to` and `from` have informations about the page we came from and the page we are going in, of course, while next is, a little bit like express, the function to call to get to the page. `next(false)` will stop the router from rendering the pages, and this is why this can be really useful, because here you can check if you have authorization to go to that page for example.

using `next()` you can also pass a path, or a path object to redirect the call to a specific route (login for example)

```
next({name:'login', params:{error:'no access'}})
```

Check the console.log() to see what information you have in to and from, and remember to make sure that you will not create an infinite loop.

## GUARDS ON SPECIFICS ROUTES

You can put guards in different spots on your code: the main.js file, the router config, or the component itself. The guards names change a bit but the overall features are the same. Why you want to do that ?

Normally, you don't need to check for authentication for each route you have, that is why having global guards (like the one we have seen before) is not always a good idea. You want guards to fire only for specific routes, and you can do that, calling the guard in the route configuration :

```
routes: [

    {

      path: '/teams',

      components: {

        default: TeamList,

        footer: TeamsFooter,

      },

      alias: ['/', '/allacatalla'],

      name: 'teams',

      beforeEnter(to, from, next) {

        console.log('teams before enter');

        next();

      },

    },
```

Note that :

The guard is called `beforeEnte`r and not `beforeEach` and this is logical, since you don't have more than one route.

The `beforeEnter` is fired only after the global `beforeEach`, you can still use it so.

Would you like to put the guard inside a component ? Sure can do! You just add the function in the export default obj :

```
components: {

    UserItem,

  },

  inject: ['users'],

  beforeRouteEnter (to, from, next) {

    console.log("User Guard Called");

    next();

  }
```

Note that this will be fired after the router one.

## BeforeRouteUpdate (on component)

This guard is a very useful one to use in one component! All the shenanigan that we have done before to make sure that we re-update a component if new data are changed ([look here](#router-caching-problem)) can be undone with `beforeRouteUpdate` inside the component! This guard will be fired when the same component is updated with new data :

```
beforeRouteUpdate(to,from,next){

  this.loadData(to.params.teamId);

  next();

}
```

## OTHER GUARDS

There are some interesting other guards that you can use like `beforeRouteLeave()` that let you run code before you change page (like: "save the data before you leave you moron!") or `afterEach()` that lets you run some code after the page is loaded, useful for analytics for example, since you can't modify anything when it is fired.

More infos here : [https://router.vuejs.org/guide/advanced/navigation-guards.html#using-the-composition-api](https://router.vuejs.org/guide/advanced/navigation-guards.html#using-the-composition-api)

## ADD INFOS TO ROUTES / ROUTE METADATA

A useful trick is to add info to the route, and those info will be present in the `$route` variable or in the `to` parameter in guards:

```
routes: [

    {

      path: '/teams',

      components: {

        default: TeamList,

        footer: TeamsFooter,

      },

      alias: ['/', '/allacatalla'],

      name: 'teams',

      beforeEnter(to, from, next) {

        console.log(to.meta.needAuth); // This will work (don't use $route here)
        console.log('teams before enter');

        next();

      },

      meta: {

        needAuth: 'ofcourse',

      },

    },
//...

//Component:
created(){

    console.log("$route",this.$route);

    console.log('using $route',this.$route.meta.needAuth)

  }

```
