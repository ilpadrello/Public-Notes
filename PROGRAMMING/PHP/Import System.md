---
title: "PHP Import System"
date: "2020-01-29"
---

Here some notes about PHP code import system

## THE INCLUDE METHOD

In PHP you can just include the other code you want to use using just the include method:

```
// index.php
<?php
include "page.php" 
?>

//page.php
<?php
echo "Ciao Mamma!";
?>
```

Using this method when we are going to run the index.php page the php code of the other page will be executed, and "Ciao MAmma" will be displayed.

But not only this! The code that is inside page.php is like it gets executed in index.php let's have a look:

```
//includer.php
<?php
$phrase = "Ciao Mamma";
include "included.php";
?>

//Included.php
<?php
echo $phrase;
?> 

//This will show "Ciao Mamma";
```

As you can see, we incorporate the code like is already on the page, this means:

A: If you call included.php directly this will generate an error (since $phrase do not exist inside this code)  
B: You can override variables

```
//Includer.php
<?php
$phrase = "Ciao Mamma";
include "included.php";
echo "<br>Ho detto ".$phrase;
?>

//included.php
<?php
$phrase = "Ciao papà";
echo $phrase;
?>
// Result:
//  Ciao papà
//Ho detto Ciao papà 
```

As you can see the variable is overrode and "Ciao Mamma" never gets printed.

## USING NAMESPACES

Namespaces are not a way to import PHP code but, it makes sure that you will not override your data this is how:

First, I have just discovered that NAMESPACES are just for classes, functions, and constants. **No variables are bounded in the namespace**.

But this will still work! (More I test, and MORE I think I prefer NodeJS way to handle this problem).

So the first thing to know is that namespaces are like folders! you can not have 2 files with the same name in the same folder but you can have 2 files in different folder with the same name. In the same way you can separate names.

Since the idea behind namespace is very similar to the idea of folders, people usually use the name of the folder as convention, but this is not the default and you can use the name that you want (this is what I do).

Also you can have subfolders (sub namespaces) and use them like folder. ex: `namespace app/config/base`

So let's experiment with this :

Let's create 2 pages index.php and test.php. :

```
//index.php
namespace index;
class test{
    public function __construct(){
        echo "CIAO MAMMA";
    }
}

function allacatalla(){
    echo "ALLACATALLA STILL RULES";
}

const A_CONSTANT = "CIAO CONSTANT";
```

```
//test.php
require'index.php';

echo index\A_CONSTANT;

$a = new index\test();

index\allacatalla();
```

We put the test class, the allacatalla function and the A\_CONSTANT constant inside the index folder and declare them inside the namespace index.

Inside the test.php page then we can call now the constant A\_CONSTANT doing : "`index\A_CONSTANT`", call the function doing "`index\allacatalla();`" and implementing the new class object like this: "`$a = new index\test()`"; Convenient don't you think? No? You say that you would have done those things using just the "`require`" that is still needed with the advantage that you can also use variables? mmm... Maybe you are right, but we have not finished yet !

Using the namespaces, you can not use the variable as if you use the require and that's it! So, in the example before, if you remove the "index\\" you can not use the functions but why is that ?

Well, that's because, by default when declaring a variable, the variable is declared in the GLOBAL scope (or folder if you want).  
If you don't specify the namespace, is like you put "\\" before the variable name, so you are on the "ROOT" folder.

But you are correct, right now, the way this work is useful (you will not mistake the variables names) but tedious!  
This is why I introduce to you the USE command :

```
// same index.php

//This is test.php

require'index.php';
use index\test as test;

$a = new test();

```

As you can see :

- You still have to use require to get the file.
- You can use the "use" statement to get rid of the "index\\" part!

Now you are actually saying "USE THIS NAME SPACE" !

So, we are using `index\test as test` so that you can use sipmly test!  
In our index.php we didn't change the value of namespace, but since we have declared "test" inside it (like a file), we can do use "index\\test". This means that you have to know how your class is named in order to use it :D (not cool I know! );

But let's try out what happens with our constant :

```
// same index.php

//This is test.php

require'index.php';
use index\test as test;
use index\A_CONSTANT;

echo A_CONSTANT;
$a = new test();

```

This will give us a fatal Error! YOU CAN NOT USE CONSTANTS WITH THE USE STATMENT! THANK YOU PHP!

Let's try something else, we will add another constant to the index.pho file, this time like this :

```
define("B_CONSTANT","CIAO B CONSTANT");
```

And test it in test.php

```
require'index.php';
use index\B_CONSTANT as B_CONSTANT

echo B_CONSTANT;
```

WOW! THIS WORK! So if you declare a constant this way this works ? WELL NO!  
This actually work just because when you use the "declare" statement the variable is always putted inside the GLOBAL SCOPE.

In fact if you try to remove the `USE` statement and just go as `index\B_CONSTANT` this will fail! THAT'S GREAT!

I'm going to try with the function this time :

```
use index\allacatalla as allacatalla;
allacatalla();
```

SORRY : "**Fatal error**: Uncaught Error: Call to undefined function allacatalla()" ! You can't do that for functions too! But you can still do "index\\allacatalla()". Everybody together we can say : THANK YOU, PHP ! :D

So, resuming :

- namespaces are like folder where you put variables in, you can use subfolder if you want to.
- requiring the file with the name space require ( :D ) to use the namespace to access variables
- not all variables are included in namespaces, only classes, functions and constants (but not with the define command)
- the `USE [namespace] AS [name]` command let you avoid writing the namespace for each class (but you will have to use it with constants and functions)

## IMPORTING VIA AUTOLOAD

Autoload is used only for classes, and that's because the function that makes autoloads possible, is called when you want to initiate a new Object. That said it is pretty interesting to know how this works, so let's dive right in.

Ok first thing to do is to create a file called autoload.php in the root of our project :

```
//autoload.php
spl_autoload_register(function($className){
    echo $className;

});
```

If what I said is correct, this function will trigger when I try to instantiate a new class that doesn't exist.  
And if I do :

```
include "autoload.php";
$c = new NewClass();
```

This is of course what happens, with of course the error that he don't find a class.  
Let's implement a better loading function :

```
spl_autoload_register(function($className){
   $file = $className.".php";
   if(file_exists($file)){
       include_once $file;
   }
});
```

Ok first we have to explain this code, the rest will be easy to understand :  
spl\_autoload\_register is a register of functions that are called when you want to instantiate a new object. As you can see we pass a function inside the register, this anonymous function will be called when you want to instantiate a new object.

In this case the function will scan for a file that has the same name as the className in the root folder.

But what if you keep your classes in more than one folder ? You could do something like that :

```
spl_autoload_register(function($className){
   $file = $className.".php";
   if(file_exists($file)){
       include_once $file;
   }else{
       if(file_exists("./subfolder/".$file)){
           include_once "subfolder/".$file;
       }
   }
});
```

This way, it first tries to find the file in the root (or where the script is called), and if the file do not exists in that folder, it tries in the subfolder ! This totally works, but as I said before, spl\_autoload\_register is actually a register for functions, so you can just simply do :

```
spl_autoload_register(function($className){
   $file = $className.".php";
   if(file_exists($file)){
       include_once $file;
   }
});

spl_autoload_register(function($className){
    $file = $className.".php";
    if(file_exists("./subfolder/".$file)) {
        include_once "./subfolder/" . $file;
    }
});
```

As you can see, you can pass more than one function to the register! This function will collect all the functions to call when you try to create a new object from a class, but this is not in the same file.

Those functions are not great by the way, and that's because the research for the classes is not always for the same folder.  
Let's imagine that you have a subfolder where you have `secondtest.php` and in this file we put this code :

```
// subfolder/secondtest.php
include "../autoload.php";

$a = new Myclass();
```

Even if we have a file called `myclass.php` in the root folder, we can't reach it! Why? Because in the autoload function that is incorporated inside the `secondtest.php` we look for the files that are in the same folder as the folder the code run.  
So we will not be able to reach the parent folder to collect the file. To solve this we can do this :

```
//This will look for the same folder as we call the autoload
spl_autoload_register(function($className){
   $file = $className.".php";
   if(file_exists($file)){
       include_once $file;
   }
});

//This will look base on the root of the folders.
spl_autoload_register(function($className) {
    $file = __DIR__ . '\\' . $className . '.php';
    $file = str_replace('\\', DIRECTORY_SEPARATOR, $file);
    if (file_exists($file)) {
        include $file;
    }
});
```

Using this trick we can now look from the root folder, but also from the folder we are in. And if we have the file with the class in the same folder it will be loaded

## ORGANIZE CLASSES LIKE A PRO

With the autoload function that we have created, we also have another advantage, we can easily use subfolder if we want to (using namespaces).

If we have a large application, we probably want to divide classes inside different folders, and this means that we need to create a function to pass to the register for each folder we create. Well this is not really necessary ! Why ? You can do something like this :

```
// subfolder/Mysecondclass.php
namespace subfolder;

class Mysecondclass{
    public function __construct(){
        echo "This is an object of my second class";
    }
}


// test.php
include "autoload.php";
$b = new subfolder\Mysecondclass();
```

The class will be automatically loaded!  
There is a need for this to work, and that is that you need to use the namespace subfolder in the `Mysecondclass.php` file.

If you don't do it this way, the file will be loaded (Mysecondclass.php), but since you are looking for classes inside the `subfolder\Mysecondclass` namespace and this namespace is empty, it will not found the class.

## AUTOLOAD PSR-4 WAY

This work only for Classes, and only if you have composer installed and running in your application.

This link explain it well : [https://phpenthusiast.com/blog/how-to-autoload-with-composer#:~:text=Autoloading%20allows%20us%20to%20use,Composer%20autoload%20non%2Dpackagist%20classes.](https://phpenthusiast.com/blog/how-to-autoload-with-composer#:~:text=Autoloading%20allows%20us%20to%20use,Composer%20autoload%20non%2Dpackagist%20classes.)

When I will have more time, ill be researching better this topic too.
