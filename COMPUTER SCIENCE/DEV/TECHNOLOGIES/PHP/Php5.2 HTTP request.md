---
title: "PHP:5.2 HTTP Request"
date: "2020-07-10"
---

In this file, we are going to see the how-to-make (not how to handle) HTTP request for an API software.

I have already created some functions to make this easy and doable: you can download the file with the functions here: FILE.\*

In order to make a request, we need to "build" the request object.  
This object is created using the`curl_init()` function, and then we change some options of the object to make it work.

### GET

```
<? php
$url="http://mywebsite.com/get_respond.php?id=1&verify=true"
$ch=curl_init();
curl_setopt($ch,CURLOPT_URL,$url);
//Make sure that the curl_exec returns the content rather that echoing it
curl_setopt($ch,CURLOPT_RETURNTRANSFER,true); 
$result = curl_exec($ch);
?>
```

So, the first thing is to get the URL you want with its parameters, then create the `curl` object using `curl_init()`.  
Once this is done, we can make some change to the object using `curl_setopt()` (set options). This function takes 3+ parameter, the first one is, of course, the object itself, the second one is not a string but what I imagine is a CONSTANT, that define which option we are changing, and the third parameter is how we want to change the option.

At the end `curl_exec($ch)` make the GET call

Here's a function I have created to simplify all this process:

```
function http_get($url=null,$parameters=null){    if(empty($url)){        return false;    }    //var_dump($parameters);    $ch=curl_init();    if(!empty($parameters)){        $url.="?";        foreach ($parameters as $index=>$param){            $url.=$index."=".urlencode($param)."&";        }    }    //echo "URL: ".$url."<br>";    curl_setopt($ch,CURLOPT_URL,$url);    //This will make sure that the curl_exec returns the content of the curl rather than echoing it.    curl_setopt($ch,CURLOPT_RETURNTRANSFER,true);    $result = curl_exec($ch);    return curl_close($ch);}
```

### POST

`POST` is not that different, we do the same things but with some different configuration for the `curl` object:

```
function http_post($url=null,$parameters=null){    if(empty($url)){        return false;    }    $ch =curl_init();    curl_setopt($ch,CURLOPT_URL,$url);    curl_setopt($ch,CURLOPT_POST,true);    if(!empty($parameters)){        curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($parameters));    }    curl_setopt($ch,CURLOPT_RETURNTRANSFER,true);    return curl_exec($ch);}
```

What is important here is `curl_setopt($ch,CURLOPT_POST,true)` that tell the object that it is a `POST` call, and also

```
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($parameters));
```

This will add the body, the payload of our POST call, with all the information we want to pass.  
It is very important to use `http_build_query` in order to make sure that node.js will find all the parameters
