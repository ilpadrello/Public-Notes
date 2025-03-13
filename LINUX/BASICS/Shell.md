---
title: "LINUX SHELL"
date: "2018-09-17"
---

**~**    <- this char indicates the root folder (not sure if is the root folder but is the "basic" folder)

##### COMMENTS:

```
# This is a comment!

echo "hello world"  #This is a comment too!
```

**ECHO:**

```
echo [option(s)] [string(s)] :
```

the echo command can be used to print something, you can print it on the shell directly by writing something like echo "hello world" or you can put it in a file like echo "hello world" > my-script.sh   **SHEBANG:**

```
#!/bin/sh — Execute the file using sh, the Bourne shell, or a compatible shell

#!/bin/csh — Execute the file using csh, the C shell, or a compatible shell

#!/usr/bin/perl -T — Execute using Perl with the option for taint checks

#!/usr/bin/php — Execute the file using the PHP command line interpreter

#!/usr/bin/python -O — Execute using Python with optimizations to code

#!/usr/bin/ruby — Execute using Ruby
```

That is called a [shebang](http://en.wikipedia.org/wiki/Shebang_%28Unix%29), if on the _very first_ line of a file it tells the shell what program to interpret the script with, when executed. In the fist line, the script is to be interpreted and run by the [bash](http://en.wikipedia.org/wiki/Bash_%28Unix_shell%29) shell.

##### [CHMOD:](http://www.simonepanebianco.fr/linux-chmod/)

**chmod** is used to change the [permissions](https://www.computerhope.com/jargon/p/permissi.htm) of [files](https://www.computerhope.com/jargon/f/file.htm) or [directories:](https://www.computerhope.com/jargon/d/director.htm)

In general, **chmod** commands take the form:

```
chmod options permissions file name

chmod u=rwx,g=rx,o=r myfile


```

If no _options_ are specified, **chmod** modifies the permissions of the file specified by _file name_ to the permissions specified by _permissions_.

```
ls -l to check the permissions on files
```

##### [CHOWN:](http://www.simonepanebianco.fr/linux-chown/)

The **chown** command changes [ownership](https://www.computerhope.com/jargon/o/owner.htm) of files and directories in a Linux [filesystem](https://www.computerhope.com/jargon/f/filesyst.htm).

```
chown [-c|--changes] [-v|--verbose] [-f|--silent|--quiet] [--dereference]

      [-h|--no-dereference] [--preserve-root]

      [--from=currentowner:currentgroup] [--no-preserve-root]

      [-R|--recursive] [--preserve-root] [-H] [-L] [-P]

      {new-owner|--reference=ref-file} file ...
```

##### GREP:

The **grep command** is used to search text. It searches the given file for lines containing a match to the given strings or words. It is one of the most useful commands on Linux and Unix-like system. Let us see how to use grep on a Linux or Unix like system.

<table><tbody><tr><td class="code"><pre class="bash">grep 'word' filename

grep 'word' file1 file2 file3

grep 'string1 string2'  filename

cat otherfile | grep 'something'

command | grep 'something'

command option1 | grep 'data'

grep --color 'data' fileName</pre></td></tr></tbody></table>

Search /etc/passwd file for boo user, enter:

`$ grep boo /etc/passwd`

[PIPING PHILOSOPHY:](http://www.simonepanebianco.fr/linux-piping-pipe-philosofy/) A pipe is a form of redirection (transfer of standard output to some other destination) that is used in Linux and other Unix-like operating systems to send the output of one command/program/process to another command/program/process for further processing. The Unix/Linux systems allow stdout of a command to be connected to stdin of another command. You can make it do so by using the pipe character **‘|’**.

```
command_1 | command_2 | command_3 | .... | command_N
```
