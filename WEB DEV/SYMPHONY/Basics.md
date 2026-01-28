---
title: "Symfony"
date: "2020-08-26"
---

Video Tutorial where infos comes:  
[https://youtu.be/Bo0guUbL5uo](https://youtu.be/Bo0guUbL5uo)

Symfony it's a framework for php, the good thing of having a framework for your application is that you can focus on the important staff of your application (like the logic) and leave other stuff to be handled by the framework (like basic security features), so you don't have to reinvent the wheel and you can also reuse some code that is made by other persons.

### Why Symfony ?

Symfony is a framework that existed for about a decade and it is used by a lot of large organizations.  
Not only Symfony has a very large and well-grown community, and it is maintained extensively and updated frequently!

Before Symfony was used primarily for big applications, but this is not the case anymore since synfony 4 that is just a small framework like laravel, and it can be used for little as big projects.

### What We Need ?

Of course since is a PHP framework we need a PHP web server, you can use Apache, Nginx, or a Docker container.  
Also, we are going to need a database system, in this tutorial we are going to use MySQL.  
We need also COMPOSER which is something that we need to install separately, that will help install packages and even Symfony itself.  
And also we are going to need a visual editor ( PHPStorm or Visual Studio Code ).

### Docker-Compose

For this test/project I will use docker-compose for having php - mysql - composer on the same spot, also this is good training for a production version:

#### docker-compose.yml

This is the docker-compose.yml file im using:

```
version: "3.7"

services:
  php:
    build: ./docker/php
    ports:
      - "65432:80"
    volumes:
      - ./:/var/www/html/
    networks:
      - proxynet

  synfony-db:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: root
    networks:
      - proxynet

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    ports:
      - "8083:80"
    volumes:
      - ./docker/phpmyadmin/config.user.inc.php:/etc/phpmyadmin/config.user.inc.php
    depends_on:
      - synfony-db
    networks:
      - proxynet
networks:
  proxynet:
    name: custom_network
```

As you can see I'm using also PhpMyAdmin, and for the PHP composer part I create a specific image ( located inside a "docker/php" folder ).  
This is the docker file I'm using:

#### Dockerfile

```
FROM php:7.4-apache
RUN apt-get update && apt-get -y --no-install-recommends install git zip unzip \
    && php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer \
    && rm -rf /var/lib/apt/lists/*
```

This will install PHP + APACHE + COMPOSER

### Installing Symfony

Ok first, you can install two versions of Symfony, one version is going to install a skeleton website, or just a skeleton, but what is the difference?

Well when you use the website version, composer will install every that you are going to need: the views, the template engine, the annotation, etc. Everything that is related to an application to work, will be installed by default.

If you use just the skeleton installer, you are going to get the core functionality of Symfony, without all those features, and if you need one of those you need to install them.

For this tutorial we are going to install Symfony without with just the core functionality.

To do so, we need composer to run, but since composer is located inside our docker container, we need to modify the DockerFile and execute it one time and then change again the Dockerfile, or you can install composer into your machine to just run the installation command (i did the second).

So run:

```
 composer create-project symfony/skeleton sfcourse
```

(Note): composer create-project is a command that will look for a package (like a boilerplate) in packagist.org, and will download it. Is not like npm for node, more like doing an installation of a complete project, like cloning a project from GitHub and then install all dependencies.

![](images/image-1024x660.png)

As you can see we have most of the core functionality of Symfony installed, like flex, cache, container console, dot env etc.

While at the bottom it will say this:

```
Run your application:
1. Go to the project directory
2. Create your code repository with the git init command
3. Download the Symfony CLI at https://symfony.com/download to install a development web server
Read the documentation at https://symfony.com/doc
```

As you can see on point 3 you can download a server CLI from the Symfony web server, that lets you get rid of apache, or Nginx. but we don't need this.

### Symfony flex

Symfony flex is a plug-in manager for Symfony.  
If you go to [https://flex.symfony.com/](https://flex.symfony.com/) you can see all the "**Recipes**" that you can install in your project.

We have 2 types of recipes, the one that are created as contribution by the community (marked "contrib") and the official one made by the symfony dev team (marked "official").

### Symofony MVC pattern

Before starting to talk about the folder structure of Symfony we need to know that Symfony use the MVC model. What is the MVC model? Is a way to deconstruct information inside the code, is a logical separation between the view, and back of an application, or better, you distinguish the Model (databases, info and others), the View (front end), the Controller (the logic behind the application). (not sure if in the Model you also have the logic and the controller is just a connection between Model and View)

### Symfony Folder Structure

Let's talk about the folder structure that we just created:

![](image-1.png)

#### ./src (source)

The folder `src` (source) is where we are going to work the most, this is where we are going to have our controller and services (we discuss those later) and our services (the logic goes here).

### ./config (configuration)

The `config` folder contains all the configurations for the project:

![](images/image-3.png)

Let's have a look:

`bundles.php` is where we are going to add the bundles, normally you don't touch it manually because when you install as bundle or something this file will be updated automatically by symfony flex.

`packages/` is where you have your configurations for your packages

### ./templates

The `templates` is where most of the view code will live in

### ./public

This is the main entry to our application, if you go to this page right now you will see the entry point of the application.

### Creating a controller and a route

Ok, let's do some action, now that everything is installed and you have a basic understand of the files structure, let's open the project with an editor..

If we go in `/src/controller` we see that we don't have any files, and everything is empty, and this is a good moment to start talking about the console:

### The Symfony Console

In Synfony you have a console that generate code for you!  
If, like in our case, we want to create a controller, we need to create the file `MainController.php` and then you have to fill it up with the right information, so the correct namespace, you will need to create a class etc etc.

But if you use the console, you can use one line and you are done with it:  
So we go back to the terminal, go to the correct folder (the root of symfony project) and then you can type:

```
php bin/console
```

and the console will responds with help commands that we can use with the Symfony terminal.  
So, when Symfony comes, it comes with a binary software that you can run (using of course php itself) in command line! That is GREAT!

But unfortunately for us, the command that we need to create automatically stuff (make) is not included in this configuration that we have installed. So in order to install it we need to use flex!

We need to go to [flex.symfony.com](https://flex.symfony.com/) and search for a "recipe" called "make":

<figure>

![](images/image-4.png)

<figcaption>

maker- bundle

</figcaption>

</figure>

So we go back to our terminal and run:

```
composer require make
```

Even if we are using composer, the composer require is going to use flex to install this recipe.

Now if you run `php bin/console` again you will see that we have a longer list with the make command in it.

So now to finally create our controller we just need to run:

```
php bin/console make:controller MainController
```

`php bin/console` will call the Symfony terminal, then we pass the command we want to execute `make:controller` and at the end, we specify the name of the controller we want to make `MainController`

But when run the command an error is raised, and this is absolutely normal, since we miss a package called annotation, so we need to install it

```
composer require annotations
```

Let's rerun the command make and SUCCESS!!!  
Now we have a file inside the correct folder (without ever think which is it) and with all the correct information inside.  
Let's have a look at the file:

```
<?phpnamespace App\Controller;use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;use Symfony\Component\Routing\Annotation\Route;class MainController extends AbstractController{    /**     * @Route("/main", name="main")     */    public function index()    {        return $this->json([            'message' => 'Welcome to your new controller!',            'path' => 'src/Controller/MainController.php',        ]);    }}
```

This is what is called an annotation :

```
/**     * @Route("/main", name="main")     */
```

Inside Symfony you have 3 ways of configuring your stuff:  
1 You can use annotation like this which looks similar to a comment  
2 You can edit the corresponding `.yaml` file inside the `./config` folder  
3 Unknow

So this annotation is actually a configuration, but how? But more importantly why?  
Let's try to respond to the why question before!  
To understand annotations, we need to understand routing.

Beautiful URLs are a must for any serious web application. This means leaving behind ugly URLs like `index.php?article_id=57` in favor of something like `/read/intro-to-symfony`.

Having flexibility is even more important. What if you need to change the URL of a page from `/blog` to `/news`? How many links would you need to hunt down and update to make the change? If you’re using Symfony’s router, the change is simple.

A _route_ is a map from a URL path to a controller. Suppose you want one route that matches `/blog` exactly and another more dynamic route that can match _any_ URL like `/blog/my-post` or `/blog/all-about-symfony`.

Routes can be configured in YAML, XML and PHP. All formats provide the same features and performance, so choose the one you prefer. If you choose PHP annotations, run this command once in your app to add support for them:

```
 composer require annotations
```

Now you can configure the routes:

// src/Controller/BlogController.php
namespace App\\Controller;
use Symfony\\Bundle\\FrameworkBundle\\Controller\\AbstractController;
use Symfony\\Component\\Routing\\Annotation\\Route;
class BlogController extends AbstractController
{
/\*\*
\* Matches /blog exactly
\*
\* @Route("/blog", name="blog\_list")
\*/
public function list()
{
// …
}
`/** * Matches /blog/* * * @Route("/blog/{slug}", name="blog_show") */ public function show($slug) { // $slug will equal the dynamic part of the URL // e.g. at /blog/yay-routing, then $slug='yay-routing' // ... }`
}

Thanks to these two routes:

- If the user goes to `/blog`, the first route is matched and `list()` is executed;
- If the user goes to `/blog/*`, the second route is matched and `show()` is executed. Because the route path is `/blog/{slug}`, a `$slug` variable is passed to `show()` matching that value. For example, if the user goes to `/blog/yay-routing`, then `$slug` will equal `yay-routing`.

Whenever you have a `{placeholder}` in your route path, that portion becomes a wildcard: it matches _any_ value. Your controller can now _also_ have an argument called `$placeholder` (the wildcard and argument names _must_ match).

Each route also has an internal name: `blog_list` and `blog_show`. These can be anything (as long as each is unique) and don’t have any meaning yet. You’ll use them later to [generate URLs](https://symfony.com/doc/4.1/routing.html#routing-generate).

You can also configure the routing in a differents way:  
Using the YAML config file:

```
config/routes.yaml
blog_list:
  path: /blog
  controller: App\Controller\BlogController::list
blog_show:
  path: /blog/{slug}
  controller: App\Controller\BlogController::show
```

Or using PHP:

```
// config/routes.php
use Symfony\Component\Routing\RouteCollection;
use Symfony\Component\Routing\Route;
use App\Controller\BlogController;
$routes = new RouteCollection();
$routes->add('blog_list', new Route('/blog', [
'_controller' => [BlogController::class, 'list']
]));
$routes->add('blog_show', new Route('/blog/{slug}', [
'_controller' => [BlogController::class, 'show']
]));
return $routes;
```

Or even XML:

```
<route id="blog_list" controller="App\Controller\BlogController::list" path="/blog" > <!-- settings --> </route> <route id="blog_show" controller="App\Controller\BlogController::show" path="/blog/{slug}"> <!-- settings --> </route>
```

Now we need to try to make the code we have created works..  
So if we see the code we have:

```
/**
 * @Route("/main", name="main")
 */
public function index()
{
 return $this->json([
   'message' => 'Welcome to your new controller!',
   'path' => 'src/Controller/MainController.php',
  ]);
}
```

So in theory we are creating a rout for `/main` to run the `index` function and since our project is in `http://localhost/sfcourse/public` we should simply add `/main` to make it works : `http://localhost/sfcourse/public/main` but unfortunately this is not the case, and the browser will return us an error, and that is because by default when you do `/public` is looking for `index.php` file, so to see our controller works we have to do: `http://localhost/sfcourse/public/index.php/main`  
Like this the controller will fire up and will show our JSON in return.

Of course this URL is ugly as hell, and you can fix this if you set up virtual host, but this is out of the scope of Symfony and we will see this (maybe) later (how to set up a virtual host).

### BUILD OUR HOMEPAGE

Ok if we want to build our home page we have to do some changes, first we need to change the routing and switch `/main` with just a `/`  
Then we can change the name to "home".

Next, we need to return a "`Response`", every action within a controller need to return a `Response` (`Response` is the actual class name) and `Response` lives in `Symfony\Component\HttpFoundation\` so don't confuse things, this is a common mistake to make, to use another namespace.

Let's see our page:

```
<?php

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class MainController extends AbstractController
{
/**
 * @Route("/", name="home")
 */
  public function index()
  {
    return new Response();
  }
}
```

So, phpstorm actually added the namespace to the page:  
`use Symfony\Component\HttpFoundation\Response;`  
But now we are responding with an empty response, and we can see if we go to the public page that is empty! We need to give a response.

So let's add some content fastly: `return new Response(content:'Welcome');`  
This code doesn't work on my version but it does on video :(  
anyway, I don't think this is the correct way to do templating :)

### DINAMYC URL

Ok, let's imagine that we want to read information in the URL using routing how do we do? (we have seen this before):

Stopped a 42 min
