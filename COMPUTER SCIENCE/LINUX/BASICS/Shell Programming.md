---
title: "LINUX SHELL PROGRAMMING"
date: "2018-09-18"
---

This is a resume of this course : [https://www.shellscript.sh/index.html](https://www.shellscript.sh/index.html)

#### WHAT SHELL SHOULD I USE? :

```
#!/bin/sh
```

This simple code tell the compiler on what shell we should run the code, in this case the bash shell, there are more shells and in the [linux](http://www.simonepanebianco.fr/linux-shell/) section of the site you can find other examples

##### COMMENTS:

```
# This is a comment!

echo "hello world"  #This is a comment too!
```

#### CREATE A FILE FOR THE SCRIPT

```
$ nano file_name.sh
```

#### CHMOD TO BE ABLE TO RUN IT

In order to run your script your script must be executable, to do so you can use the command [CHMOD](http://www.simonepanebianco.fr/linux-chmod/)

```
$ chmod 755 file_name.sh
```

#### use ./ to run it

In order to run your script (as a script) you cant do just symply

```
$ file_name.sh #This will not work
```

Because the shell will try to find a command called file\_name.sh that doesnt exist, but you can do it this way

```
$ ./file_name.sh #This will work
```

#### VARIABLES IN THE SHELL

Like most of the programming languages the shell can have variables, and there are some things to know about variables in shell **1.  NO $ in declaration BUT YES on the use** Example:

```
#!/bin/sh

Myvar="hello world"

echo $Myvar
```

**2 ALL Variable type are accepted**

```
#!/bin/sh

Myvar="String"

Myvar=12

Myvar=12.45

Myvar="12.45"

Myvar=string
```

All the variables are transformed in string after declaration Note though that special characters must be properly escaped to avoid interpretation by the shell. "This is discussed further in Chapter 6, [Escape Characters](https://www.shellscript.sh/escape.html)." **3 RESPECT THE CONTENT** Even if all variables are strings , you must respect the content, If in your variable there is string (with no number) you just cant add 1

```
#!/bin/sh

echo "Tell me a number?"

read my_num #THIS WILL ASK FOR A NUMBER

add="10"

result=`expr $my_num + $add`

echo "Hello, your number is $my_num and i have calculated: $result"
```

If we give him a number this will work normally and will print out the result, but if we dont

```
Tell me a number?

kerl #Text input

expr: non-integer argument #Error!! 

Hello, your number is kerl and i have calculated: #The program dont stop even if there is an error
```

So we learn 2 things: 1 READ is used to read an user input and that even if there is a Error linux shell dont stop. **4 READ** As we have see before we can use READ to read an input from the user:

```
#!/bin/sh

echo "Tell me a number?"

read my_num #THIS WILL ASK FOR A NUMBER

echo "Your number is $my_num"
```

**5 NO DECLARATION** All the variables actually NEVER GET DECLARATED, you can call a variable that doesn't exist and still get an empty string, this means that you dont have never ever declarations, but the big difference is that you need to avoid the $ symbol when you try to put value in the variable

```
a="hello"

a="world"

echo $a #this will print world
```

While this one will not work

```
a="hello"

$a="world"

echo $a #this will print hello and an Error
```

**6 THE "${VAR}\_SUFFIX" STRUCTURE** Another example of an easy mistake to make with shell scripts. One other thing worth mentioning at this point about variables, is to consider the following shell script:

* * *

```
#!/bin/sh

echo "What is your name?"

read USER_NAME

echo "Hello $USER_NAME"

echo "I will create you a file called $USER_NAME_file"

touch $USER_NAME_file
```

* * *

Think about what result you would expect. For example, if you enter "steve" as your USER\_NAME, should the script create `steve_file`? Actually, no. This will cause an error unless there is a variable called `USER_NAME_file`. The shell does not know where the variable ends and the rest starts. How can we define this? The answer is, that we enclose the variable itself in _curly brackets_:

* * *

[user.sh](https://www.shellscript.sh/eg/user.sh.txt)

```
#!/bin/sh

echo "What is your name?"

read USER_NAME

echo "Hello $USER_NAME"

echo "I will create you a file called ${USER_NAME}_file"

touch "${USER_NAME}_file"
```

* * *

The shell now knows that we are referring to the variable `USER_NAME` and that we want it suffixed with "`_file`". This can be the downfall of many a new shell script programmer, as the source of the problem can be difficult to track down. Also note the quotes around `"${USER_NAME}_file"` - if the user entered "Steve Parker" (note the space) then without the quotes, the arguments passed to `touch` would be `Steve` and `Parker_file` - that is, we'd effectively be saying `touch Steve Parker_file`, which is two files to be `touch`ed, not one. The quotes avoid this. Thanks to Chris for highlighting this.
