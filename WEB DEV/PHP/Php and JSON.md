---
title: "PHP and JSON"
date: "2020-02-02"
---

jsonmapper: [netresearch/jsonmapper](https://packagist.org/packages/netresearch/jsonmapper)

We already know what a JSON is so let's dive on how to encode JSON in PHP! We can use json\_encode() and json\_decode() to achieve this:

## ENCODING JSON

In PHP the json\_encode() function is used to encode a value to JSON format, the value being encoded can be any PHP data type, except a resource, like a database or filehandle.

```
<?php 
// An associative array 
$marks = array("Peter"=>65, "Harry"=>80, "John"=>78, "Clark"=>90);   echo json_encode($marks); 
?>

//{"Peter":65,"Harry":80,"John":78,"Clark":90}


```

simiraly you can ecode indexed array into a JSON like this:

```
<?php 
// An indexed 
array $colors = array("Red", "Green", "Blue", "Orange", "Yellow");   echo json_encode($colors); 
?>

// Json output:  ["Red","Green","Blue","Orange","Yellow"] 
```

You can also force json\_encode() to return a PHP array as a JSON object using JSON\_FORCE\_OBJECT :

```
<?php 
// An indexed 
array $colors = array("Red", "Green", "Blue", "Orange");
echo json_encode($colors, JSON_FORCE_OBJECT); 
?> 

//Json output: {"0":"Red","1":"Green","2":"Blue","3":"Orange"} 
```

Now this is how to encode a nested JSON:

```
<?php
//This is the result we want: 
/*
 {"Peter":65,
    "altro":[
        {
            "Simone":"Beddu",
            "Rosi":"Laria"
        }
    ],
    "oggetto":{
        "luca":"senior",
        "simone":"mid junior",
        "giuseppe":"junior"
    }
} */

//This is how we encode it:
$altro = array(array("Simone"=>"Beddu","Rosi"=>"Laria"));
$oggetto= array("luca"=>"Senior","simone"=>"mid senior","giuseppe"=>"junior");

$tojson=array("Peter"=>65,"altro"=>$altro,"oggetto"=>$oggetto);

var_dump(json_encode($tojson));

?>
```

for each object (even nested) we create an array, this is not very difficult as you can see.

## DECONDING JSON

Decoding JSON is not too difficult, let's have a look at this code:

```
<?php 
// Store JSON data in a PHP variable 
$json = '{"Peter":65,"Harry":80,"John":78,"Clark":90}';   var_dump(json_decode($json)); 
?>

//output:   object(stdClass)#1 (4) { ["Peter"]=> int(65) ["Harry"]=> int(80) ["John"]=> int(78) ["Clark"]=> int(90) } 
```

As you can see a JSON object is decoded as an Object, this means that we can do stuff like this:

```
$json->Peter //65
```

And now let's have a look at differents scenarios:

```
<?php
$json = '{"Peter":65,
    "altro":[
        {
            "Simone":"Beddu",
            "Rosi":"Laria"
        }
    ],
    "oggetto":{
        "luca":"senior",
        "simone":"mid junior",
        "giuseppe":"junior"
    }
}';
$json=json_decode($json);

var_dump($json->oggetto->luca); //senior
//This is how we get to luca
echo "<br>";
var_dump($json->altro[0]->Simone); //beddu
//This is how we get inside an array and inside Simone
```
