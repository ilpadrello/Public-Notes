---
title: "CLASSES IN PHP"
date: "2020-01-29"
tags: 
  - "class"
  - "classes"
  - "php"
---

PHP now supports also classes(from version 5 on), this is very good for programming techniques.

## DEFINING A CLASS

To define a class PHP use the "class" keyword, followed by the name of the class:

```
<?php
class Fruit {
  // Properties
  public $name;
  public $color;

  // Methods
  function set_name($name) {
    $this->name = $name;
  }
  function get_name() {
    return $this->name;
  }
}
?> 
```

And then you can add properties, and methods.  
To refer to the same object you need to use the "$this" keyword, and to refer to a function or a parameter inside the class you use the arrow : "$this->$name".

## CONSTRUCTOR

In order to initialize the constructor function we use the "\_\_constructor()" (2 underscores) function:

```
<?php
class Fruit {
  public $name;
  public $color;

  function __construct($name) {
    $this->name = $name;
  }
  function get_name() {
    return $this->name;
  }
}

$apple = new Fruit("Apple");
echo $apple->get_name();
?> 
```

**Note**: Parent constructors are not called implicitly if the child class defines a constructor. In order to run a parent constructor, a call to **parent::\_\_construct()** within the child constructor is required. If the child does not define a constructor then it may be inherited from the parent class just like a normal class method (if it was not declared as private).

## DESTRUCTOR

A destructor is called when an object is destructed, the script is stopped or exited (At this moment i don't see a useful way to use this code but ok).  
The destructor method will be called as soon as there are no other references to a particular object, or in any order during the shutdown sequence  
To set the destructor use the "\_\_destruct()" function (2 underscore):

```
<?php
class Fruit {
  public $name;
  public $color;

  function __construct($name) {
    $this->name = $name;
  }
  function __destruct() {
    echo "The fruit is {$this->name}.";
  }
}

$apple = new Fruit("Apple");
echo "this text should be before the __destruct method <br>"; 
?> 
```

The "destruct()" function is called only when the script stops so at the end of the script, and not just when the object is destroyed, this means that if I use a webserver to run PHP each time my page is loaded, the object gets created, and destroyed, and the subsequent functions called.  
But not only, now observe this:

```
<?php
class Fruit {
  public $name;
  public $color;

  function __construct($name) {
    $this->name = $name;
  }
  function __destruct() {
    echo "The fruit is {$this->name}.<br>";
  }
}

$apple = new Fruit("Apple");
$apple = new Fruit("Apple");
$apple = new Fruit("Apple");
$apple = new Fruit("Apple");

echo "this text should be before the __destruct method <br>";
?> 
```

What do you think will be the output of this? There will be first the text, and then all the deconstruct methods? Well, this is the result:

```
The fruit is Apple.
The fruit is Apple.
The fruit is Apple.
this text should be before the __destruct method
The fruit is Apple. 
```

And this logic, if you look back at the code, is correct as we use the same variable ($apple) to rewrite a new object, so the object that existed before get destructed, is in fact different if we use differents variables:

```
<?php
class Fruit {
  public $name;
  public $color;

  function __construct($name) {
    $this->name = $name;
  }
  function __destruct() {
    echo "The fruit is {$this->name}.<br>";
  }
}

$applea = new Fruit("Apple");
$appleb = new Fruit("Apple");
$applec = new Fruit("Apple");
$appled = new Fruit("Apple");

echo "this text should be before the __destruct method <br>";
?> 

/*
 this text should be before the __destruct method
The fruit is Apple.
The fruit is Apple.
The fruit is Apple.
The fruit is Apple. 

*/
```

This time the results are as expected :)  
It is also worth notice that :

The destructor will be called even if script execution is stopped using [exit()](https://www.php.net/manual/en/function.exit.php). Calling [exit()](https://www.php.net/manual/en/function.exit.php) in a destructor will prevent the remaining shutdown routines from executing.

Note: Attempting to throw an exception from a destructor (called in the time of script termination) causes a fatal error.

Note: Destructors called during the script shutdown have HTTP headers already sent. The working directory in the script shutdown phase can be different with some SAPIs (e.g. Apache).

Note: Like constructors, parent destructors will not be called implicitly by the engine. In order to run a parent destructor, one would have to explicitly call **parent::\_\_destruct()** in the destructor body. Also like constructors, a child class may inherit the parent's destructor if it does not implement one itself.

More Infos and tricks:  
[https://www.php.net/manual/en/language.oop5.decon.php](https://www.php.net/manual/en/language.oop5.decon.php)

## Access modifiers (private / public)

Properties and methods of a class can have access modifiers, which control where they can be accessed. There are three access modifiers:

- public - can be access from everywhere (default)
- protected - can be accessed within the class and by his child classes
- private - can ONLY be accessed within the class (no child support sorry)

```
<?php
class Fruit {
  public $name;
  protected $color;
  private $weight;
}

$mango = new Fruit();
$mango->name = 'Mango'; // OK
$mango->color = 'Yellow'; // ERROR
$mango->weight = '300'; // ERROR
?> 
```
