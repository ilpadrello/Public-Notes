---
title: "PHP common use"
date: "2020-02-02"
---

Here some refresh on how to use php!

## COMMENTING

```
<?    
# This is a comment, and    
# This is the second line of the comment        
// This is a comment too. Each style comments only print "An example with single line comments"; ?> 

/* Multi line
Comment */
```

## MULTI LINE PRINTING

```
<?
   # First Example
    print <<<END
    This uses the "here document" syntax to output
    multiple lines with $variable interpolation. Note
    that the here document terminator must appear on a
    line with just a semicolon no extra whitespace!
    END;

   # Second Example
    print "This spans
    multiple lines. The newlines will be
    output as well";
?> 
```

## HEREDOC

With heredoc we can easly write multi-line out of PHP code, and using just the dollar value we pass a variable in it:

```
<?php
$var = "i'm a variable";
$text = <<<HTML
inside this text that is multi
line (at least inside the php document)
i cant put variables like this : $var;
HTML; 
echo $text;

?> 
```

I have used here the tag HTML but you can use another tag as well (like EOT or other)!  
Of course, you can treat all the text inside the heredoc as if it is a single string.  
If you want you can escape the $ sign to make sure it's not considered as variable:

```
\$var  // will output '$var' and not the value of $var
```

## NOWDOC

Using Nowdoc the variable are not converted in values, to create a Nowdoc instead of a heredoc, you just put the tag name inside ' ':

```
<?php
$var = "i'm a variable";
$text = <<<'HTML'
inside this text that is multi
line (at least inside the php document)
i cant put variables like this : $var;
HTML; 
echo $text;

?>  
```

## VARIABLES:

PHP use $ before the name of the variable  
Variables used before they are assigned have default values  
PHP convert automatically types of variables if necessary

### VARIABLE TYPES

- **Integers** − are whole numbers, without a decimal point, like 4195.
- **Doubles** − are floating-point numbers, like 3.14159 or 49.1.
- **Booleans** − have only two possible values either true or false.
- **NULL** − is a special type that only has one value: NULL.
- **Strings** − are sequences of characters, like 'PHP supports string operations.'
- **Arrays** − are named and indexed collections of other values.
- **Objects** − are instances of programmer-defined classes, which can package up both other kinds of values and functions that are specific to the class.
- **Resources** − are special variables that hold references to resources external to PHP (such as database connections).

### CONSTANTS

A constant can not change during the execution of the script.  
By convention constant are uppercase, and usually start with a letter or underscore.  
Unlike variables, you don't need to have a $ on the constant.  
you can define constants in 2 ways: using define() or const:

```
define("COSTANTE", 50)
echo COSTANTE;

const COSTANZA = 50;
echo COSTANZA
```

even if there are two-way of defining constants they are not the same, more info here ( [https://www.tutorialspoint.com/compare-define-vs-const-in-php](https://www.tutorialspoint.com/compare-define-vs-const-in-php) ) generally it is bettere to use define()

### MAGIC CONSTANTS

Magic constants are predefined constants that are always available in your code, those constants are case-insensitive. Here some of them: ( full list: [https://www.javatpoint.com/php-magic-constants](https://www.javatpoint.com/php-magic-constants) )

| Sr.No | Name & Description |
| --- | --- |
| 1 | **\_\_LINE\_\_**The current line number of the file. |
| 2 | **\_\_FILE\_\_**The full path and filename of the file. If used inside an include,the name of the included file is returned. Since PHP 4.0.2, **\_\_FILE\_\_** always contains an absolute path whereas in older versions it contained relative path under some circumstances. |
| 3 | **\_\_FUNCTION\_\_**The function name. (Added in PHP 4.3.0) As of PHP 5 this constant returns the function name as it was declared (case-sensitive). In PHP 4 its value is always lowercased. |
| 4 | **\_\_CLASS\_\_**The class name. (Added in PHP 4.3.0) As of PHP 5 this constant returns the class name as it was declared (case-sensitive). In PHP 4 its value is always lowercased. |
| 5 | **\_\_METHOD\_\_**The class method name. (Added in PHP 5.0.0) The method name is returned as it was declared (case-sensitive). |
