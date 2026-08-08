---
title: "VUE 3 COMPOSITION API"
date: "2022-08-31"
---

Thus far, we used the Options API for building Vue apps/components, and this is absolutely fine, but you have to know that if you encounter some problems, or you have specific need that you can not solve with the Option API (fortunately this is rare), you can use the composition API.  
Also is good to know that you can mix the two-way of writing Vue code.

One advantage of the Composition API over the Option API is that code that belong togheter logically is not split up, we will see that later.

## MERGING DATA METHODS COMPUTED AND WATCH

The basic idea behind the composition API is that you merge together data, methods, computed, and watch options. And use just the `setup()` function. So, props, components, etc, will remain intact, and all the `v-model` `v-bind` staff also remain the same.
