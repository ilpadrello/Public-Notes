---
title: "PHP 7"
date: "2020-01-29"
---

Php7 is a major release of PHP with a lot of new features, a new faster way of executing the code, and a ton of new features :

There are dozens of features added to PHP 7, the most significant ones are mentioned below −

- **Improved performance** − Having PHPNG code merged in PHP7, it is twice as fast as PHP 5.
- **Lower Memory Consumption** − Optimized PHP 7 utilizes lesser resource.
- **Scalar type declarations** − Now parameter and return types can be enforced.
- **Consistent 64-bit support** − Consistent support for 64-bit architecture machines.
- **Improved Exception hierarchy** − Exception hierarchy is improved.
- **Many fatal errors converted to Exceptions** − Range of exceptions is increased covering many fatal error converted as exceptions.
- **Secure random number generator** − Addition of new secure random number generator API.
- **Deprecated SAPIs and extensions removed** − Various old and unsupported SAPIs and extensions are removed from the latest version.
- **The null coalescing operator (??)** − New null coalescing operator added.
- **Return and Scalar Type Declarations** − Support for return type and parameter type added.
- **Anonymous Classes** − Support for anonymous added.
- **Zero cost asserts** − Support for zero cost assert added.

PHP 7 uses new Zend Engine 3.0 to improve application performance almost twice and 50% better memory consumption than PHP 5.6. It allows to serve more concurrent users without requiring any additional hardware. PHP 7 is designed and refactored considering today's workloads.

In order to develop and run PHP Web pages, three vital components need to be installed on your computer system.

- **Web Server** − PHP works with virtually all Web Server software, including Microsoft's Internet Information Server (IIS) but most often used is Apache Server. Download Apache for free here − [http://httpd.apache.org/download.cgi](http://httpd.apache.org/download.cgi)
- **Database** − PHP PHP works with virtually all database software, including Oracle and Sybase but most commonly used is MySQL database. Download MySQL for free here − [http://www.mysql.com/downloads/](http://www.mysql.com/downloads/)
- **PHP Parser** − In order to process PHP script instructions, a parser must be installed to generate HTML output that can be sent to the Web Browser. This tutorial will guide you how to install PHP parser on your computer.

## PHP Parser Installation

Before you proceed, it is important to make sure that you have proper environment setup on your machine to develop your web programs using PHP. Store the following php file in Apache's htdocs folder.

### phpinfo.php

```
<?php
   phpinfo();
?>
```

Type the following address into your browser's address box.

```
http://127.0.0.1/phpinfo.php

```

If this displays a page showing your PHP installation related information, then it means you have PHP and Webserver installed properly. Otherwise, you have to follow the given procedure to install PHP on your computer.

This section will guide you to install and configure PHP over the following four platforms −

- [PHP Installation on Linux or Unix with Apache](https://www.tutorialspoint.com/php7/php7_installation_linux.htm)
- [PHP Installation on Mac OS X with Apache](https://www.tutorialspoint.com/php7/php7_installation_mac.htm)
- [PHP Installation on Windows NT/2000/XP with IIS](https://www.tutorialspoint.com/php7/php7_installation_windows_iis.htm)
- [PHP Installation on Windows NT/2000/XP with Apache](https://www.tutorialspoint.com/php7/php7_installation_windows_apache.htm)

## Apache Configuration

If you are using Apache as a Web Server, then this section will guide you to edit Apache Configuration Files.

Check here − [PHP Configuration in Apache Server](https://www.tutorialspoint.com/php7/php7_apache_configuration.htm)

## PHP.INI File Configuration

The PHP configuration file, **php.ini**, is the final and immediate way to affect PHP's functionality.

Check here − [PHP.INI File Configuration](https://www.tutorialspoint.com/php7/php7_ini_configuration.htm)

## SCALAR TYPE DECLARATION

In PHP7 a new feature, Scalar type declarations, has been introduced, and they have two options:

- Coercive - the default mode, it converts values if necessary from a type to another just to make sure the code will run
- Strict - This mode must be explicitly hinted using the "declare" statement, and will force a major control of the type of data.

So those type for function declarations can be enforced using the above mode (Coercive / Strict )

- int
- float
- bool
- string
- interfaces (class like object but i'm not sure)
- array
- callable (like functions PHP couldn't pass functions before)

So that the code here will result in 2 different way if you use Coercive or Strict mode:

```
<?php    
// Coercive mode    
declare(strict_types=1); //this change the method (Strict or Corecive)
function sum(int ...$ints) 
{
       return array_sum($ints);
}
print(sum(2, '3', 4.1));

//This will print 9 in Coercive mode
//Strict mode: Fatal Error:
//  Fatal error: Uncaught TypeError: Argument 2 passed to sum() must be of the type integer, string given, ... 
?> 
```

## PHP 7 - Return Type Declarations

In PHP 7 a return type declarations have been introduced, now you can specify the type of data that a function should return (not get in input, RETURN). The types are the same as above (int, float, bool etc):  
Here an example of a valid return type

```
<?php    
declare(strict_types = 1);    
function returnIntValue(int $value): int 
{       
     return $value;    
}    
print(returnIntValue(5)); // print 5
?> 
```

As you can see we use ":" and the type we want to return to specify it! Now let's have a look at an invalid return type:

```
<?php    
declare(strict_types = 1);    
function returnIntValue(int $value): int 
{       
     return $value + 1.0; //This will convert int into float    
}    
print(returnIntValue(5)); // print Error
?> 

// Output Error:
// Fatal error: Uncaught TypeError: Return value of returnIntValue() must be of the type integer, float returned... 
```

## Null Coalescing Operator (??)

Another new feature of PHP7, Null coalescing Operator, is used to replace the **ternary** operation in conjunction with "isset()" function.  
Let me explain better, as you know (you should know) to check if a variable exists in PHP we use the "isset()" function, now let's imagine that you want to get the username passed via a GET, and if the username doesn't exist ($\_GET\['username'\]), the value of the variable $username will be set to 'not passed', to do this the old way you do:

```
$username = isset($_GET['username']) ? $_GET['username'] : 'not passed'; 
```

You use the ternary operation (? and : ) that is like an if statement, now there is a operator that make this code easier:

```
$username = $_GET['username'] ?? 'not passed'; 
```

The "??" automatically check if the first value is Null (or not existing) and if that's the case it passes to the next value. Why is this a better way to performing this task? Because you can chain them:

```
 $username = $_GET['username'] ?? $_POST['username'] ?? 'not passed'; 
```

Cool!!

## Spaceship Operator <=>

Light side? Dark side? Here we go!!  
The Spaceship operator is used to compare two expressions, it returns -1,0,1 when the first expression is less than, equal to, greater than the second expression:

```
print( 1 <=> 1); //0
print( 1.5 <=> 2.5); //-1
print( "b" <=> "a"); //1
```

## Constant Arrays

A Array constants can now be defined using the **define()** function. In PHP 5.6, they could only be defined using **const** keyword.

```
<?php    
//define a array using define function    
define('animals', [
       'dog',
       'cat',
       'bird'
]);
print(animals[1]); 
?> 
```

## Anonymous Classes

Anonymous classes can now be defined using "new class". Anonymous class can be used in place of a full class definition:
