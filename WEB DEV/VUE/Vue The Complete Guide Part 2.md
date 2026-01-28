---
title: "VueJS - The Complete Guide (part-2)"
date: "2019-11-10"
---

(This is the second part of vue.js complete guide, too see the first part go to: [http://www.simonepanebianco.fr/vuejs-the-complete-guide-part-1/](http://www.simonepanebianco.fr/vuejs-the-complete-guide-part-1/)

# UNDERSTANDING THE VUE INSTANCE

In this part, we will try to understand the vue.js instance, the middleman between the DOM and our LOGIC!  
Can we have more than one Vue instance in our code? Can we access the data from outside the instance? Let's try to answer those questions!

## YOU CAN CREATE MORE THAN ONE INSTANCE:

Yes! we can create more that one instance of vue.js at the same time we just need to use a different wrapper! But we can not access the elements of on instance from inside another instance!  
And this is very helpful if you want only part of your app to run with vue.js, you can easily make a widget for your site with vue.js without vue.js being the core of your code!

## YOU CAN ACCESS THE DATA FROM OUTSIDE THE INSTANCE:

To access the data from outside the instance you just need to store the instance inside a variable:

```
<script>
    let app = new Vue({ //First instance called app
      el: "#app",
      data: {
        text: "hello world"
      }
    });
    let app2 = new Vue({ //Second instance called app2
      el: "#app2",
      data: {
        text: "this is the second isntance"
      }
    });
    setTimeout(() => {
      app.text = "this is the first instance"; //Here we can change the value of one instance from outside the instance
    }, 3000);
</script>
```

As you can see with this method we can easily change the value of the first instance from outside the code using just "instancename.data"..

## HOW VUE MANAGE YOUR DATA AND METHODS

When we create an instance of vue.js, we pass with it an object with our data and methods and then we can call the methods and the data for outside if we store the instance inside a variable, and that is possible thanks to the facts that the engine of vue.js take that object and incorporate it inside the instance! it does a lot more than that, it creates a watcher for some variables, updates the DOM, etc...

But this doesn't mean that is a two-way process, I can not do "**app.variable=**" and use it with vue.js, well it's not that is not valid code, is that Vue.js will not react with something like that, it will not watch on it, etc...

So vue.js works only with the data that we pass when creating the instance!

## $data, $refs

If you go and "console.log" an instance of vue.js you will see a lot of parameters, like "$el" that is the HTML of the element we attached, or "$data" that is the data object that we have passed.  
And we can also access those infos directly :

```
app.variable //those two are the same
app.$data.variable //those 2 are the same
```

In this way we can also easily use "$ref". To easily understand the code, the best way is to code it:

```
<button ref="mybutton">Click Me</button> //ref is not HTML STANDARD
```

Vue.js add this tag "ref" to help as register an element that we want to refer later in our code! Now that this element is in our register ("app.$ref") we can access it to retrieve information or other purposes.

```
//inside the instance
this.$ref.mybutton.innerText
//outside the instance
app.$ref.mybutton.innerText
```

Unfortunately, there is a catch using this method, if we change something w change directly the DOM, we do not change the template system that vue.js create for us this means that when we re-rend the page when something happens the code that we have changed will return as it is in the template (that is a kind of middle layer between how our page is created and how is rendered)

Others available property of the Vue.js API you can go to: [https://vuejs.org/v2/api/](https://vuejs.org/v2/api/)

## Mounting a template

When you use `el:'#app'`, you are actually saying to the vue.js where to mount the data you have just created. But you could also decide to change this value on the way, or mount the value outside, and to do so you just do:

```
vm1.$mount.('#app1');
```

## Creating a template

Inside Vue.js you can create a template (with some limitaions) and doing so you create html directly inside this template:

```
<div id="app3"></div>
 var vm3 = new vue({
  template:'<h1>Hello!</h1>'
})
vm3.mount();
document.getElementById('app3').appendChild(vm3.$el);
```

## Components

What if we want to reuse some part of the code? In this case we use components!  
If you try to mount one instance of vue.js to multiple HTML (using something like a class for example), you will see that only the first object will be affected, this is because you can only attach one element to an instance of vue.js

In order to reuse the code in multiples places, you use the components:

```
Vue.component('component_name',{data_for:'the_component'});
```

and stuff like:

```
<div id="app">
  <hello></hello>
  <hello></hello>
  <hello></hello>
</div>

Vue.component('hello',{
  template:'<h1>Hello!</h1>'
});
```

And in this way I will have 3 hello's in my page.
