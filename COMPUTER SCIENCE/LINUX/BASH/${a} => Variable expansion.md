# Summary Table

|Syntax|Meaning|
|---|---|
|`${var:-x}`|use x if unset or empty|
|`${var:=x}`|assign x if unset or empty|
|`${var:?msg}`|exit if unset or empty|
|`${#var}`|length|
|`${var:pos:len}`|substring|
|`${var%pattern}`|remove suffix|
|`${var##pattern}`|remove prefix|
|`${var/old/new}`|replace|

This is how a normal variable is used in BASH : 

```bash
a="ciao mamma"
echo $a #ciao mamma
```

That means that `$a` is replaced with its storage value, but this also work : 

```bash
a="ciao mamma"
echo ${a} #ciao mamma
```

So why use this sintax ? 
This way we specify even more that we must use the value of a.. look at this : 

```bash
a="ciao mamma"
echo $a_mia #This print nothing since there is no variable called a_mia
```

```bash
a="ciao mamma"
echo ${a}_mia #bash understand the variable name and output ciao mamma_mia
```

## Advanced features
### Substring
```bash
a="abcdef"  
echo ${a:2:3} # start at index 2(c) end after 3 char: **cde**
```

### String length
```bash
name="Simone"
echo ${#name} # 6
```

### Remove short Suffix
```bash
file="backup.tar.gz"
echo ${file%.gz} # output: backup.tar
```

### Remove long suffix

```bash
file="backup.tar.gz"
```

### Remove Prefix:
```bash
path="/home/simone/file.txt"  
echo ${path##*/} # file.txt
```

### Replace inside variable
```bash
text="hello world"  
echo ${text/world/Bash} # hello bash
```

```bash
# Replace all
echo ${text//l/X} # heXXo worXd
```
### Default values
Default values are useful especially if you use the `set -u` in the script that will create an error if a variable is unset.

```bash
echo ${a:-"default"} # the word default is printed if a does not exist
```

In what cases $a is not printed ? Here's some tests: 

```bash
# Print default_value
a=""
echo ${a:-"default_value"}
a=
echo ${a:-"default_value"}
a=$NULLVAR
echo ${a:-"default_value"}

# Print the actual value
a=0
echo ${a:-"default_value"}
a=false #bool is considered text
echo ${a:-"default_value"}
a=null
echo ${a:-"default_value"}
```

### Differences between `-` and `:-`
There is a subtle but important difference: 
`${var-default}`: Uses default only if variable is **unset**
`${var:-default}`: Uses default if variable is **unset OR empty**

Example:

```bash
name=""  
echo ${name-default}   # prints empty  
echo ${name:-default}  # prints "default"
```

This difference matters in scripts.

### Default and assign

`${var:=default}`: if variable is unset OR empty:

- assign default to it
- return default

```bash
echo ${name:="Anonymous"}
```

Now `name` becomes `"Anonymous"` permanently.

Very useful for config values.

### Throw Error If Missing (Very Powerful)

`${var:?error message}` : If variable is unset OR empty → script exits with error.

