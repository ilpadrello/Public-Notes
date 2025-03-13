---
title: "SYMFONY TWIG"
date: "2020-12-26"
---

[https://twig.symfony.com/doc/3.x/](https://twig.symfony.com/doc/3.x/)

Twig is a template engine that is used in symfony.

This template engine is not the only one we can use in our system and also is not linked at symfony. You can use this engine outside symfony.

let's dive in

Let'see a simple page created when we launch the program:

`php bin/console make:controller`

This is what we see:

```
{% extends 'base.html.twig' %}

{% block title %}Hello CharacterController!{% endblock %}

{% block body %}
<style>
    .example-wrapper { margin: 1em auto; max-width: 800px; width: 95%; font: 18px/1.5 sans-serif; }
    .example-wrapper code { background: #F5F5F5; padding: 2px 6px; }
</style>

<div class="example-wrapper">
    <h1>Hello {{ controller_name }}! ✅</h1>

    This friendly message is coming from:
    <ul>
        <li>Your controller at <code><a href="{{ 'C:/xampp/htdocs/Study/Symfony4/firsttest/src/Controller/CharacterController.php'|file_link(0) }}">src/Controller/CharacterController.php</a></code></li>
        <li>Your template at <code><a href="{{ 'C:/xampp/htdocs/Study/Symfony4/firsttest/templates/character/index.html.twig'|file_link(0) }}">templates/character/index.html.twig</a></code></li>
    </ul>
</div>
{% endblock %}
```

First line :

```
{% extends 'base.html.twig' %}
```

This code allow you to extend(like in classes) the code that of the page `base.html.twig`  
This page is our standard page, which we are going to fill with our HTML.

Next:

```
{% block title %}Hello CharacterController!{% endblock %}
```

This will refill some block of HTML defined in the basic page (as said before), so we can rewrite those code with new code.

The good thing about this kind of templating is that you can define different "standard page", for example one for index and one for the articles etc etc, and you can fill its inside using your specific view.

### EXTEND VS INCLUDE

The `include` statement includes a template and returns the rendered content of that template into the current one while the `extends` statement is more like the "class" extended. That means that you can reuse, overwrite blocks that come from the parent page.

### SHOW A VARIABLE

`{{variable}}`

### FOR LOOP

```
{% for i in 0..10 %}
```

### ASSET

Using the command `asset` we can refer directly to the public folder that is used to store all the public images (like photos or other stuffs)

```
<img src="{{ asset("images/photo.jpg") }}"
```

As you can see we don't have to put `/public` in order to go to the public folder.
