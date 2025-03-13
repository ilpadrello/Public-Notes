---
title: "LINUX CHMOD"
date: "2018-09-17"
---

**chmod** is used to change the [permissions](https://www.computerhope.com/jargon/p/permissi.htm) of [files](https://www.computerhope.com/jargon/f/file.htm) or [directories:](https://www.computerhope.com/jargon/d/director.htm)

In general, **chmod** commands take the form:

```
chmod options permissions file name
```

If no _options_ are specified, **chmod** modifies the permissions of the file specified by _file name_ to the permissions specified by _permissions_.

```
ls -l to check the permissions on files
```

_permissions_ defines the permissions for the owner of the file (the "user"), members of the group who owns the file (the "group"), and anyone else ("others"). There are two ways to represent these permissions: with symbols ([alphanumeric](https://www.computerhope.com/jargon/a/alphanum.htm) [characters](https://www.computerhope.com/jargon/c/charact.htm)), or with [octal](https://www.computerhope.com/jargon/o/octal.htm) numbers (the digits **0** through **7**).

Let's say you are the owner of a file named **myfile**, and you want to set its permissions so that:

1. the **u**ser can **r**ead, **w**rite, ande **x**ecute it;
2. members of your **g**roup can **r**ead ande **x**ecute it; and
3. **o**thers may only **r**ead it.

This command will do the trick:

```
chmod u=rwx,g=rx,o=r myfile
```

This example uses symbolic permissions notation. The letters **u**, **g**, and **o** stand for "**user**", "**group**", and "**other**". The equals sign ("**\=**") means "set the permissions exactly like this," and the letters "**r**", "**w**", and "**x**" stand for "read", "write", and "execute", respectively. The commas separate the different classes of permissions, and there are no spaces in between them.

Here is the equivalent command using octal permissions notation:

```
chmod 754 myfile
```

Here the digits **7**, **5**, and **4** each individually represent the permissions for the user, group, and others, in that order. Each digit is a combination of the numbers **4**, **2**, **1**, and **0**:

- **4** stands for "read",
- **2** stands for "write",
- **1** stands for "execute", and
- **0** stands for "no permission."

So **7** is the combination of permissions **4**+**2**+**1** (read, write, and execute), **5** is **4**+**0**+**1** (read, no write, and execute), and **4** is **4**+**0**+**0** (read, no write, and no execute).

<table class="mtable3 tab"><tbody><tr class="tcw"><td><b>-c</b>,&nbsp;<b>--changes</b></td><td>Like&nbsp;<b>--verbose</b>, but gives verbose output only when a change is actually made.</td></tr><tr class="tcw"><td><b>-f</b>,&nbsp;<b>--silent</b>,&nbsp;<b>--quiet</b></td><td>Quiet mode; suppress most error messages.</td></tr><tr class="tcw"><td><b>-v</b>,&nbsp;<b>--verbose</b></td><td>Verbose mode; output a diagnostic message for every file processed.</td></tr><tr class="tcw"><td><b>--no-preserve-root</b></td><td>Do not treat '<b>/</b>' (the&nbsp;<a href="https://www.computerhope.com/jargon/r/root.htm">root</a>&nbsp;directory) in any special way, which is the default setting.</td></tr><tr class="tcw"><td><b>--preserve-root</b></td><td>Do not operate&nbsp;<a href="https://www.computerhope.com/jargon/r/recursive.htm">recursively</a>&nbsp;on '<b>/</b>'.</td></tr><tr class="tcw"><td><b>--reference=</b><i>RFILE</i></td><td>Set permissions to match those of file&nbsp;<i>RFILE</i>, ignoring any specified&nbsp;<i>MODE</i>.</td></tr><tr class="tcw"><td><b>-R</b>,&nbsp;<b>--recursive</b></td><td>Change files and&nbsp;<a href="https://www.computerhope.com/jargon/d/director.htm">directories</a>&nbsp;recursively.</td></tr><tr class="tcw"><td><b>--help</b></td><td>Display a help message and exit.</td></tr><tr class="tcw"><td><b>--version</b></td><td>Output&nbsp;<a href="https://www.computerhope.com/jargon/v/version.htm">version</a>&nbsp;information and exit.</td></tr></tbody></table>

## Technical Description

**chmod** changes the file mode of each specified _FILE_ according to _MODE_, which can be either a symbolic representation of changes to make, or an [octal](https://www.computerhope.com/jargon/o/octal.htm) number representing the bit pattern for the new mode bits.

The format of a symbolic mode is:

\[**ugoa**...\]\[\[**+-=**\]\[_perms_...\]...\]

where _perms_ is either zero or more letters from the set **r**, **w**, **x**, **X**, **s** and **t**, or a single letter from the set **u**, **g**, and **o**. Multiple symbolic modes can be given, separated by commas.

A combination of the letters **u**, **g**, **o**, and **a** controls which users' access to the file will be changed: the user who owns it (**u**), other users in the file's group (**g**), other users not in the file's group (**o**), or all users (**a**). If none of these are given, the effect is as if **a** were given, but bits that are set in the [umask](https://www.computerhope.com/unix/uumask.htm)are not affected.

The operator **+** causes the selected file mode bits to be added to the existing file mode bits of each file; **\-** causes them to be removed; and **\=** causes them to be added and causes unmentioned bits to be removed except that a directory's unmentioned set user and group ID bits are not affected.

The letters **r**, **w**, **x**, **X**, **s** and **t** select file mode bits for the affected users: read (**r**), write (**w**), execute (**x**), execute only if the file is a directory or already has execute permission for some user (**X**), set user or group ID on execution (**s**), restricted deletion flag or sticky bit (**t**). For directories, the execute options **X** and **X** define permission to view the directory's contents.

Instead of one or more of these letters, you can specify exactly one of the letters **u**, **g**, or **o**: the permissions granted to the user who owns the file (**u**), the permissions granted to other users who are members of the file's group (**g**), and the permissions granted to users that are in neither of the two preceding categories (**o**).

A numeric mode is from one to four octal digits (**0**\-**7**), derived by adding up the bits with values **4**, **2**, and **1**. Omitted digits are assumed to be leading zeros. The first digit selects the [set user ID](https://www.computerhope.com/jargon/s/suid.htm) (**4**) and set group ID (**2**) and restricted deletion or sticky (**1**) attributes. The second digit selects permissions for the user who owns the read (**4**), write (**2**), and execute (**1**); the third selects permissions for other users in the file's group, with the same values; and the fourth for other users not in the file's group, with the same values.

**chmod** never changes the permissions of [symbolic links](https://www.computerhope.com/jargon/s/symblink.htm); the **chmod** system call cannot change their permissions. However, this is not a problem since the permissions of symbolic links are never used. However, for each symbolic link listed on the [command line](https://www.computerhope.com/jargon/c/commandi.htm), **chmod** changes the permissions of the pointed-to file. In contrast, **chmod** ignores symbolic links encountered during [recursive](https://www.computerhope.com/jargon/r/recursive.htm) directory traversals.

## Setuid And Setgid Bits

**chmod** clears the set-group-ID bit of a regular file if the file's group ID does not match the user's effective group ID or one of the user's supplementary group IDs, unless the user has appropriate privileges. Additional restrictions may cause the [set-user-ID](https://www.computerhope.com/jargon/s/suid.htm) and set-group-ID bits of _MODE_ or _RFILE_to be ignored. This behavior depends on the policy and functionality of the underlying **chmod** system call. When in doubt, check the underlying system behavior.

**chmod** preserves a directory's set-user-ID and set-group-ID bits unless you explicitly specify otherwise. You can set or clear the bits with symbolic modes like **u+s** and **g-s**, and you can set (but not clear) the bits with a numeric mode.

## Restricted Deletion Flag (or "Sticky Bit")

The restricted deletion flag or sticky bit is a single bit, whose interpretation depends on the file type. For directories, it prevents unprivileged users from removing or renaming a file in the directory unless they own the file or the directory; this is called the restricted deletion flag for the directory, and is commonly found on world-writable directories like **/tmp**. For regular files on some older systems, the bit saves the program's text image on the swap device so it will load more quickly when run; this is called the sticky bit.

## Viewing Permissions of files

A quick and easy way to list a file's permissions are with the long listing (**\-l**) option of the [**ls**](https://www.computerhope.com/unix/uls.htm) command. For example, to view the permissions of **file.txt**, you could use the command:

```
ls -l file.txt
```

...which will display output that looks like the following:

```
-rwxrw-r-- 1   hope   hopestaff  123   Feb 03 15:36   file.txt
```

Here's what each part of this information means:

<table class="mtable3 tab"><tbody><tr class="tcw"><td><b>-</b></td><td>The first character represents the file type: "<b>-</b>" for a regular file, "<b>d</b>" for a directory, "<b>l</b>" for a symbolic link.</td></tr><tr class="tcw"><td><b>rwx</b></td><td>The next three characters represent the permissions for the file's owner: in this case, the owner may&nbsp;<b>r</b>ead from,&nbsp;<b>w</b>rite to, ore&nbsp;<b>x</b>ecute the file.</td></tr><tr class="tcw"><td><b>rw-</b></td><td>The next three characters represent the permissions for members of the file group. In this case, any member of the file's owning group may&nbsp;<b>r</b>ead from or&nbsp;<b>w</b>rite to the file. The final dash is a placeholder; group members do not have permission to execute this file.</td></tr><tr class="tcw"><td><b>r--</b></td><td>The permissions for "others" (everyone else). Others may only&nbsp;<b>r</b>ead this file.</td></tr><tr class="tcw"><td><b>1</b></td><td>The number of&nbsp;<a href="https://www.computerhope.com/jargon/h/hardlink.htm">hard links</a>&nbsp;to this file.</td></tr><tr class="tcw"><td><b>hope</b></td><td>The file's owner.</td></tr><tr class="tcw"><td><b>hopestaff</b></td><td>The group to whom the file belongs.</td></tr><tr class="tcw"><td><b>123</b></td><td>The size of the file in&nbsp;<a href="https://www.computerhope.com/jargon/b/block.htm">blocks</a>.</td></tr><tr class="tcw"><td><b>Feb 03 15:36</b></td><td>The file's mtime (date and time when the file was last modified).</td></tr><tr class="tcw"><td><b>file.txt</b></td><td>The name of the file.</td></tr></tbody></table>

## Examples

```
chmod 644 file.htm
```

Set the permissions of **file.htm** to "owner can read and write; group can read only; others can read only".

```
chmod -R 755 myfiles
```

Recursively (**\-R**) Change the permissions of the directory **myfiles**, and all folders and files it contains, to mode **755**: User can read, write, and execute; group members and other users can read and execute, but cannot write.

```
chmod u=rw example.jpg
```

Change the permissions for the owner of **example.jpg** so that the owner may read and write the file. Do not change the permissions for the group, or for others.

```
chmod u+s comphope.txt
```

Set the "Set-User-ID" bit of **comphope.txt**, so that anyone who attempts to access that file does so as if they are the owner of the file.

```
chmod u-s comphope.txt
```

The opposite of the above command; un-sets the SUID bit.

```
chmod 755 file.cgi
```

Set the permissions of **file.cgi** to "read, write, and execute by owner" and "read and execute by the group and everyone else".

```
chmod 666 file.txt
```

Set the permission of **file.txt** to "read and write by everyone.".

```
chmod a=rw file.txt
```

Accomplishes the same thing as the above command, using symbolic notation.
