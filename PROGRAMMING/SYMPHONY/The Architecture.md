---
title: "SYMFONY 4 - ARCHITECTURE"
date: "2020-12-26"
---

This article will explain the why and how of the SYMPHONY architecture.  
Folder structure and MVC.

## THE MVC OF SYMFONY

Ok, let's fast recap what is MVC: Model View Controller.  
Model = DB, View = Front, Controller = Data Handler.  
In Symfony we also have another concept that is the ROUTER.

### What is the ROUTER?

The router understands the request of the client (usually because listen to the URL request) and dispatch the code to execute.  
So for example if we visit www.mywebsite.com/students the router understands that we want to see the list of all students.

Here is an image that ewplain a little better

![](images/image-1024x631.png)

Routing in symfony is done using **comments** on the page (this is not very intuitive to me but this is how he does).

Here another example

## SRC (source) folder

Globally the src folder is where you stock your "MODEL" and "CONTROLLER" but also other things.

### CONTROLLER Folder

This folder contains all the controllers for our application.

### \>ENTITY Folder

This folder will contain all the classes of the MODEL that will be the link with our DB

### \>Repository Folder

This folder is like the Entity folder is useful to connect the application to our database, this folder contains all the requests to our database.

### \>MIGRATION Folder

This folder contains all the files that let you have coherence between your model and the database. (we will understand this concept better later)

## TEMPLATES Folder

This folder contains all the view of our application.

## CONFIG folder

This is where you can configure your project.  
You can apply a personal configuration but also you can configure your package in this folder.

## PUBLIC Folder

The public folder is used to store everything that is necessary for the front.

## BIN Folder

The binary folder, this contains all the binary code that symfony can use to accomplish tasks, like creating a new page or other things.  
If you want to list all the action that the console have:

`php .\bin\console list`
