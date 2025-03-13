---
title: "ZEND FRAMEWORK 1"
date: "2020-02-17"
---

This is the document im learning from: [Document](http://www.simonepanebianco.fr/wp-content/uploads/2020/02/Zend-Framework.pdf)

Zend framework is an old framework for PHP (especially the first version), it's based on the MVC paradigm, and I need to know it because I use it at work (yes is 2020 and still ZEND 1)

It is also based on the idea of reusable code, for fast coding. It is usefull when learning ZEND to have the right mindset, to learn in the right conditions.

A good comprehension of the Zend Framework is based on mastering a good number of notions, here is a list of some complementary notion we are going to see thought this tutorial:

- **The Framework**: Understanding the advantage of using a framework
- **Databases**: Abstractions, gateway, ORM, CRUD, those are necessary to understand the _Zend\_db_
- **OOP**: This is really necessary to master if you want to use Zend at its full power
- **Design Patterns**: they are very useful to understand some zend components, and POO programming
- **MVC Pattern**: one of the most important design and it deserve its own chapter
- **Unity testing**: This should be learned too, but I have no idea if I have the time.

## State of mind

- **Zend Framework does not impose anything**: it suggests, you are free to do as you like, architecture side, component side, or using your own norms.
- **Zend Framework is easy**: The idea is that you don't have to build something really really big, you can scale it up slowly

## THE PROJECT

To better understand the logic and the code behind Zend, we are going to develop an imaginary app that is used to reserve conference rooms for different companies.

So let's see what this application will be to do:  
First, there will be 2 parts of this software: a reservation part that will be in charge of making a reservation for the rooms, and a public part where will see what rooms are free.  
Also, our app will have an authentication system for making reservations.  
All users will not be the same, there will be admins accounts capable of making changes in the system.

The system should also be capable of exporting PDFs, CVSs, XMLs, JSONs

And it also should be able to communicate with other applications of the enterprise, and for this, we will need: SOAP access, REST access (API), JSON access.

And finally, one other very important task it should be able to evolve in time, add functionality and modules if needed.

We will work in a Linux or Windows Environment, with Zend Framework 1.6.2, PHP version 5.2.6, MySQL version 5.0.51 and Apache 2.28I

## Installing Zend Framework

- Install Apache and run PHP and MySQL
- Create a folder like c:\\www
- Download the software from the Zend website (of course it is impossible right now)
- Unzip it and move the library folder inside the precedent folder that we have created
- We need now to configure Apache:
- Open the _php.ini_ and modify the line called _include\_path_

```
include_path = ".;C:/www/library"
```

- Now we need to create a folder that will be visible by Apache:

```
C:/www/htdocs
```

- Then we need to tell Apache that the root folder for all HTTP requests is the one that we have just created, so edit the _httpd.conf_ file like this

```
DocumentRoot "C:/www/htdocs"

DocumentRoot "/www/htdocs" - this is for Linux
```

Your Zend Framework is now up and running and I now just realised that all this stuff is useless if you already put the files inside the correct folder... COOL (want to kill myself now).

If you want to test zend working:

- Create test.php inside the hotdocs folder
- Put this code in this file:

```
<?php 
// Affichage de la date courante require 'Zend/Date.php'; 
$date = new Zend_Date(); 
echo $date;
?>
```

This is important because we will use the Zend\_date module to run this code so if it is working, this should show the date of the moment (and of course the date inside the tutorial I'm following is freaking 2008!!!)

## BASIC COMPONENTS OF ZEND

Unfortunately, this looks very important because it looks that zend uses it own library and modules to do basic stuff, so...

Every section dedicated to a component is divided into 2 parts: The _example_ part, and the _app integration_ part.  
In the first part will be about the example of uses of the component and the second part will be about how to integrate this inside the code

We will use another folder for those examples, a subfolder of hotdocs called examples.

I will personally list here the ones that I think are the most important, and then list all pages inside the documentation I'm reading from (but unfortunately they are in French so French-up my friend)

### Zend\_Loader

_Zend\_Loader_ handles the loading of classes from the _library_ folder, it is used instead of the _include()/require()_ functions and there are 2 ways of using _Zend\_Loader_, you can call it every time you need a class, or you can configure it to call classes automatically when you want to use a specific class, this is called _autoload_.  
Auto load is actually used very often.

This is the method for manual loading

```
//This is the manual load of a class //Include Zend Loader include 'Zend/Loader.php'; //Use the Zend_loader to use Zend View Zend_Loader::loadClass('Zend_View'); //Create an instance of Zend view $view = new Zend_View(); var_dump($view)
```

As you can see we use the method of the class without creating an object when we use the loadClass method, and this works.

loadclass() can be used also for every class that respect the Zend Framework standard naming. This convention allow PHP to find the class inside the folders, you need to replace the underscore "\_" with a slash "/" and add ".php" suffix. This means that the Zend\_View class it's located in Zend/View.php( **This is useful to us to know if someone use this function** )

Now we will see the automatic loading of a class:

```
// This is the autoload method for classes
include 'Zend/Loader.php'; 

// Now we declare that we want to use autoload Zend_Loader::registerAutoload(); 

// We just need to call the class
$date = new Zend_Date(); 
var_dump($date); 
```

As you can see when we just use the new keyword and as we do that the loader will search for the class and use it (this, of course, will work only if we use the name system before explained)

Of course there are other uses for Zend\_Loader, like testing if a calss exist:

```
// Inclusion de Zend_Loader include 'Zend/Loader.php';
// Chargement avec vérification 
if (!Zend_Loader::isReadable('Zend/View.php')) {  
throw new Exception('Unable to use Zend_View.'); 
}
Zend_Loader::loadClass('Zend_View'); 
var_dump(new Zend_View()); 
```

or also:

```
// Utilisation d’une classe personnalisée pour l’auto-chargement Zend_Loader::registerAutoload('My_Loader'); 

// Utilisation de Zend_Date avec auto-chargement implicite var_dump(new Zend_Date()); 
```

Zend\_Loader is commonly configured in the bootstrap of the app. This is how we can declare Zend\_Loader of a manual use:

```
// Utilisation de Zend_Loader 
require_once 'Zend/Loader.php'; 
```

### Zend\_Config

The purpose of _Zend\_Config_ is to manipulate configurations files, with this component it is possible to stock different formats for configs.

- The .ini format
- XML format
- PHP format

The .ini is easy to read and edit but limited in its structure  
The XML format is good to organize data, but need memory  
The PHP format, is the best performing and allows to include in the configuration some dynamic procedures.

#### Zend\_Config\_Ini

To use the .ini format you need to use the Zend\_Config\_Ini class, here what is contained inside the _zend\_config\_ini.ini_ file

```
; Directives de configuration de la production 
[prod] 
database.host = dbserver 
database.user = dbuser 
database.pass = dbprodpass 
database.name = dbname ; 

Directives de configuration de la dev ; 
Ces directives héritent de la production 

[dev : prod] 
database.host = localhost 
database.pass = dbpass 
```

in order to use this file for config the app, we need to create an object allowing access to directives:

```
// Affichage en mode texte (pour la lisibilité de l’exemple) header('Content-type: text/plain; charset=utf-8'); 


// Utilisation de Zend_Config_Ini 
include 'Zend/Config/Ini.php'; 

// Création d’un objet contenant les directives de dev 
$configFile = dirname(__FILE__) . '/zend_config_ini.ini'; 
$config = new Zend_Config_Ini($configFile, 'dev'); 

// Utilisation de Zend_Config_Ini 
echo 'Database : ' . $config->database->name . "\n"; 
echo 'Hostname : ' . $config->database->host . "\n"; 
echo 'Username : ' . $config->database->user . "\n"; 
echo 'Password : ' . $config->database->pass . "\n"; 
```

As you can see, the file ".ini" will be converted in attributes of the "$config" Object and you can access it easily .

**MORE THEN ONE FILE:** The idea is that you can use more than one file to create your configuration! You just call it inside a new "Zend\_Config\_ini" every time you need new informations.

#### The Other methods (XML and PHP)

Got to page 36 (55) of the book for more info.

### Zend\_Log

Page 40(59)

### Zend\_Debug

Page 43(62)

### Zend\_Exception

Page 44 (63)

### Zend\_Registry

Page 45(64)

Allows having a collection of values, it can be considered as a way to have GLOBAL variables. A lot of methods are used to be able to create global variables without worrying, like the isRegistered() method that allows us to check if a variable already exist in the register. More on the book.

## DATABASE Access

page 50 (69)

Zend Framework creates a layer of discussion for a different type of databases, I think the best way to understand what the heck is going on in the project is to check the code.

First somewhere there must be all the Infos about the DB connection (we need to check if it is in an .ini file or something different)

### Request something from the database

Zend has a lot of different methods and classes that you have to learn in order to use databases in your project.

They introduce the concept of an adapter, which is like a layer of connection to the database, which means that one day we can change the DB, and we need just to change the adapter for that database and not the logic of the software to make it work.

Not only this but you have different kinds of adapter like the "Pdo" one (Php Data Object) that is available in all instances of Php running Zend.

So, for example, we use an adapter that is an instance of Zend\_Db\_Adapter\_Pdo\_Mysql (WTF!!!) that has all the methods we need to work with DB.

```
$db = new Zend_Db_Adapter_Pdo_Mysql(array(  
'host ' => '127.0.0.1',  
'username' => 'zfbook',  
'password' => 'secret',  
'dbname' => 'zfbook'
)); 
```

### Fetch for DB methods

All the Adapters have those methods

#### fetchAll()

Thi will return all the results

#### fetchRow()

This will fetch only the first row of the result

#### fetchAssoc()

récupère tous les résultats dans un tableau associatif

#### fetchCol()

This will fetch all result but only for the first column asked

#### fetchOne()

Is like fetchRow() + fetchCol(), it will return only the first row for the frist column

#### fetchPairs()

This will return results as an associative table, where column 1 is the index and column 2 is the result

### DATABASE QUERY EXAMPLE

```
$query = "SELECT lastname, firstname FROM user"; 
$result = $db->fetchAll($query)); 
Zend_Debug::dump($result);
```

And this is the result

```
array(2) {  
  [0] => array(2) {  
    ["lastname"] => string(5) "pauli"  
    ["firstname"] => string(6) "julien"  
  }  
  [1] => array(2) {  
    ["lastname"] => string(7) "ponçon"  
    ["firstname"] => string(9) "guillaume"  
  } 
} 
```

### BINDING USE

Every fetch instance have a parameter _bind_, this is link or an array of link to connect SQL to values look:

```
$query = "SELECT firstname, lastname  FROM user  WHERE id=:id AND is_admin=:admin"; 
$id_array = range(1, 10); 
foreach ($id_array as $id) {
  $binds = array('id'=>$id, 'admin'=>1);
  $result = $db->fetchRow($query, $binds);
  echo $result['firstname'], $result['lastname'];
} 
```

### DATABASE INSERTION

let's have a look at a DB insertion:

```
try {
  $data = array('name' => 'Salon', 'capacity' => 15);
  $count = $db->insert("room", $data);
  echo $count . " salle(s) insérée(s)";
}
catch (Zend_Db_Exception $e) {
  printf("erreur de requête : %s", $e->getMessage()); 
} 
```

As you can see we use the adapter $db method _insert()_ we do not pass through a specific SQL to insert data, I personally found this method horrendous, but what do I know...

### DATABASE UPDATE

Do you want to update data for a user? Did you spend time and money learning tools such as SQL? Zend don't give a Fu\*% about that:

```
$updated = $db->update("user", array('is_admin' => 0), 'id=1'); 
echo $updated . " enregistrement(s) affecté"; 
```

### DATABASE DELETE

I will not comment:

```
$conditions = array("creator=1"); 
$deleted = $db->delete("reservation", $conditions); 
echo $deleted . " enregistrement(s) supprimé(s)"; 
```

WTF IS THIS CODE????

More info page 57 (76)

### ADVANCED SELECT ON DB

WHY????????????? WHY IS THIS NEEDED?????

```
$select = $db->select(); 
$select->from(array("r" => "reservation"), "usage")
  ->join(array("s" => "room"),
  "r.id_room=s.id",  array("salle" => "name"))
  ->join(array("u" => "user"),
  "r.creator=u.id",
  array("personne" => "firstname"))
  ->where("s.id=3")
  ->where("u.id=2")
  ->limit(3); 
```

### USING GATEWAY

All the parts databases, and interface, will be substituted with the Model layer in the MVC paradigm. It must be isolated in global classes that will allow us to easily handle database data.  
Using Zend Framework, those classes use the logic existing in the Zend\_Db\_Table.

Zend\_Db\_Table is a sub-component of Zend\_Db that uses a "Data Table Gateway", this allows creating a gateway between a database and a PHP class.

Each table can be represented by a class, and the objects of that class make all the operations on that table possible.

- _Zend\_Db\_Table\_Abstract_ : contains all the logic behind table handling
- _Zend\_Db\_Table\_Row\_Abstract_ : represent a save of that table that we can modify, delete, or save
- _Zend\_Db\_Table\_Rowset\_Abstract_: represent un number of records returned by an SQL query, it is interactive and quantifiable and sometimes it is returned by _Zend\_Db\_Table\_Abstract_.
- _Zend\_Db\_Table\_Select_ extends _Zend\_Db\_Select_, it react like a class, but it will work inside the frame of its _Zend\_Db\_Table_

**(I'm really running out of Fu\*$% to give)**

```
/**
 * Modèle associé à la table user
 */ 
class TUser extends Zend_Db_Table_Abstract {
  protected $_name = 'user'; //Table name
  protected $_primary = 'id'; //Primary key
} 
```

The class TUser extend of Zend\_Db\_Table\_Abstract, and we give to it the name of the table (in this case user), and the primary key for the table(id).  
A primary key that is composed of more columns, should be specified with an array.

Now let's see how to make a database using this new **useless** method.

OF COURSE, every FREAKING class that extends from Zend\_Db\_Table\_Abstract, that we can also call Model (referring the MVC model) will, OF COURSE, need the adapter in order to pilot the database, and there is a very simple way to do so (OF COURSE VERY SIMPLE WAY)

```
// $db est l’objet de connexion Zend_Db_Adapter Zend_Db_Table_Abstract::setDefautAdapter($db);
```

Zend\_Db\_Table\_Abstract in nothig more than another layer of Zend\_Db\_Adapter, that is used to manipulate data by the concept of TABLE.

Methods like insert(), delete(), or even update(), are really similar to the one that we have already seen (but **OF COURSE** not the same) with the difference that they do not accept name table parameter since this is already encapsulated in the object.

So **AGAIN** we have the methods fetchAll(), fetchRow(), find() and createRow(), that gives back one or more record

A record is represented by an object, instance of _Zend\_Db\_Table\_Row\_Abstract_, By default the class Zend\_Db\_Table\_Row is used

More records can be encapsulated in an object _Zend\_Db\_Table\_Rowset\_Abstract_ that is by default a _Zend\_Db\_Table\_Rowset_, this object _Rowset_, is iterable and quantifiable (page 61(80) to see the methods)
