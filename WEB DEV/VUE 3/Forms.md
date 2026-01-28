---
title: "VUE 3 FORMS"
date: "2022-08-21"
---

Now let's dive in the deep of forms for Vue 3 !

## SUBMIT A FORM

If you want to get all the infos of a form without sending it out:

```
<form @submit.prevent="submitForm">
```

Where the `prevent` modifier prevent the action of sending the form itself and `submitForm` is a method that collects the data.

## USING V-MODEL (Is a shortcut)

The usual thing that people do is to use the `v-model` to bind the value of the form to something in data, and there is nothing wrong with that. Just know that you cal also use `$ref.value` also to retrieve info.

Also, you need to know that `v-model` is a shortcut for manually listening to the `@input` and binding the value attribute:

## SELECT

```
<select id="referrer" name="referrer" v-model="referrer">
   <option value="google">Google</option>
   <option value="wom">Word of mouth</option>
   <option value="newspaper">Newspaper</option>
</select>
... 
//data
return {
   referrer:'google' // This can be null
}
```

Changing the select option will affect the data.

## CHECKBOX

A single checkbox could be considered like a true / false value :

```
<h2>Do you agree with terms?</h2>
<div>
  <input v-model="terms" type="checkbox" name="terms" id="terms"> //Value not needed
  <label for="terms">I agree</label>
</div>
...
//Data:
data(){

    return {

      terms:true

    }

  },
```

But if you want more checkbox for the same "group" of answers (like list of interests, or others) you can do that

## CHECKBOX GROUPING

If you use the same name, they create a group, and that group is transformed into an array of values when checked! Do not forget to give a value to each checkbox, otherwise, this will not work.

```
//Thoose are binded with a single data variable called interests
<div>
   <input v-model="interests" name="interests" id="interest-news" type="checkbox" value="news" /> //Value needed
   <label for="interest-news">News</label>
</div>

<div>
   <input v-model="interests" name="interests" id="interest-tutorials" type="checkbox" value="tutorials"/> //Value needed
   <label for="interest-tutorials">Tutorials</label>
</div>

<div>
   <input v-model="interests" name="interests" id="interest-nothing" type="checkbox" value="nothing"/> //Value needed
   <label for="interest-nothing">Nothing</label>
</div>

//Data:
data(){
    return {
      interests:[]
    }
```

try to see that in the Vue developer tool!

## GOOD PRACTICE

It is a good practice to use modifiers when using forms:

```
v-model.trim="userName" //Will trim the text before sending
```

```
v-model.number="age" //Will convert the value into number
```

## @Blur vs @Input vs @submit

Usually, developers validate data before sending the form, you can do that before submitting the form with `@submit`, or maybe you want to do that when the user changes input, then you can use `@Blur` (the item loses focus), Or you may want to check every time the user changes the value inside the input, then you want to use `@input`... The point is, you can do that easily using those;

## BUILD YOUR CUSTOM INPUT/CONTROL COMPONENT AND V-MODEL IT!

Sometimes inputs, radio, checkbox, and all the input that you need are not enough to fulfill your needs, maybe you want a rating 5-star button, or specific date input! For whatever reason, you can create your own input type and use it as an input.

Let's start with the first part: build your component! A three-button rating should do the trick.

```
//RatingController.vue
<template>
    <div>
        <button v-for="rating in ratings" v-bind:key="rating" :class="{active: choosenRating===rating}" type="button" @click="changeRating(rating)">{{rating}}</button>
    </div>
</template>



<style scoped>
button{
    padding: 0.5rem;
    border:2px solid;
    border-color: transparent;
    }
.active{
    border-color:green;
}
</style>



<script>
export default {
    data(){
        return{
            ratings:['Very Poor','Poor','Normal','Great','Super Great],
            choosenRating:null,
        }
    },
    methods:{
        changeRating(rating){
            console.log(rating);
            this.choosenRating = rating;
        }
    }
}
</script>
```

```
//Parent.vue
//...
<div>
  <rating-control></rating-control>
</div>
//...
import RatingControl from './RatingController.vue'

//...
  components:{
    RatingControl
  },
```

Using this technique, we will have as many rating buttons as we want just by adding and removing them from the array!

Now we have a component, that has logic and a layout! But how do we access the value that the users selected from the parent? Yes, I know you can emit an event, but there is a better way, we can use the v-model from the parent and reflect that in our child.
## PROPS: modelValue EMIT update:modelValue

By default, when using v-model to a component, Vue set a very specific prop in the component, and it will listen to a very specif event, that you can emit in that component. The props is `modelValue` and the event is `update:modelValue`. So doing this:

```
<rating-control v-model="rating"></rating-control>
```

Is the same as doing this:

```
<rating-control :model-value="" @update:model-value=""></rating-control>
```

The first way is much cooler! Ok, but since we have a prop and an event to listen to, we need to modify our custom component :
```
//RatingController.vue
export default {
    data(){
        return{
            ratings:['Very Poor','Poor','Normal','Great'],
            choosenRating:this.modelValue,
        }
    },
    props:['modelValue'], //Adding the prop
    emits:['update:modelValue'], //Adding the emits
    methods:{
        changeRating(rating){
            console.log(rating);
            this.choosenRating = rating;
            this.$emit('update:modelValue',rating); //Passing the value to the parent.
        }
    }
}
```

And of course we need to add the rating data in our parent :

```
//...
<rating-control v-model="rating"></rating-control>
//...
data(){
  return {
    rating:null
  }
},
```

When a change is done inside the component, that will be reflected to the parent! Are we done here? Well no...  
Structuring the code this way we have a problem, if we want to change the value of rating from the parent itself (like we want to reset the form), that will not be reflected on the component. Why? Because we have used a variable (`choosenRating`) to store our value, and the communication parent -> child is done only at the beginning! So it is good practice, when you want to use a `v-model` syntax to communicate with your child, to use just `modelValue` to manipulate your component! Like this:

```
//...
<button v-for="rating in ratings" v-bind:key="rating" 
:class="{active: modelValue===rating}" 
type="button" @click="changeRating(rating)">{{rating}}</button>
//... 

changeRating(rating){


        this.$emit('update:modelValue',rating);

}
```

Using directly `modelValue` in the component will do the linking directly, now when the `v-model` change from the parent, it will affect also the component `modelValue` prop.

Of course, if for some specific reason, you can not afford to link them directly, and you want still have this connectivity, you can use the a computed method:

```
export default {
    data(){
        return{
            ratings:['Very Poor','Poor','Normal','Great'],
        }
    },
    computed:{
      choosenRating(){
          console.log("computed called");
          return this.modelValue;
      }  
    },
    props:['modelValue'],
    emits:['update:modelValue'],
    methods:{
        changeRating(rating){
            console.log(rating);
            this.choosenRating = rating;
            this.$emit('update:modelValue',rating);
        }
    }
}
```

Note that if you use computed, you need to remove the value from data.
