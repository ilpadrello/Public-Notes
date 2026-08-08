---
title: "LINUX CHOWN"
date: "2018-09-18"
---

## About chown

The **chown** command changes [ownership](https://www.computerhope.com/jargon/o/owner.htm) of files and directories in a Linux [filesystem](https://www.computerhope.com/jargon/f/filesyst.htm).

## What is file "ownership"?

[Linux](https://www.computerhope.com/jargon/l/linux.htm) is designed to support a large number of [users](https://www.computerhope.com/jargon/u/user.htm). Because of this, it needs to keep careful track of who is allowed to access a file, and how they can access it. These access rules are called [permissions](https://www.computerhope.com/jargon/p/permissi.htm).

There are three major types of file permissions:

- **User** permissions. These permissions apply to a single user who has special access to the file. This user is called the **owner**.
- **Group** permissions. These apply to a single group of users who have access to the file. This group is the **owning group**.
- **Other** permissions. These apply to every other user on the system. These users are known as **others**, or **the world**.

When a file is created, its owner is the user who created it, and the owning group is the user's current group.

**chown** can change these values to something else.

## Syntax

```
chown [-c|--changes] [-v|--verbose] [-f|--silent|--quiet] [--dereference]

      [-h|--no-dereference] [--preserve-root]

      [--from=currentowner:currentgroup] [--no-preserve-root]

      [-R|--recursive] [--preserve-root] [-H] [-L] [-P]

      {new-owner|--reference=ref-file} file ...
```

```
chown --help
```

```
chown --version
```

Specifying the new owner

New ownership of _file_ is specified by the [argument](https://www.computerhope.com/jargon/a/argument.htm) _new-owner_, which takes this general form:

```
[user[:group]]
```

Specifically, there are five ways to format _new-owner_:

| _new-owner_ form | Description |
| --- | --- |
| _user_ | The name of the user to own the file. In this form, the colon ("**:**") and the _group_ is omitted. The owning group is not altered. |
| _user_**:**_group_ | The _user_ and _group_ to own the file, separated by a colon, with no spaces in between. |
| **:**_group_ | The group to own the file. In this form, _user_ is omitted, and the _group_ must be preceded by a colon. |
| _user_**:** | If _group_ is omitted, but a colon follows _user_, the owner is changed to _user_, and the owning group is changed to the login group of _user_. |
| **:** | Specifying a colon with no _user_ or _group_ is accepted, but ownership will not be changed. This form does not cause an error, but changes nothing. |

Notes on usage

- _user_ and _group_ can be specified by name or by number.
- Only [root](https://www.computerhope.com/jargon/r/root.htm) can change the owner of a file. The owner cannot transfer ownership, unless the owner is root, or uses [**sudo**](https://www.computerhope.com/unix/sudo.htm) to run the command.
- The owning group of a file can be changed by the file's owner, if the owner belongs to that group. The owning group of a file can be changed, by root, to any group. Members of the owning group other than the owner cannot change the file's owning group.
- The owning group can also be changed by using the [**chgrp**](https://www.computerhope.com/unix/uchgrp.htm) command. **chgrp** and **chown** use the same [system call](https://www.computerhope.com/jargon/s/system-call.htm), and are functionally identical.
- Certain miscellaneous file operations can be performed only by the owner or root. For instance, only owner or root can manually change a file's "atime" or "mtime" (access time or modification time) using the [**touch**](https://www.computerhope.com/unix/utouch.htm) command.
- Because of these restrictions, you will almost always want to run **chown** as root, or with **sudo**.

## Options

| Option | Description |
| --- | --- |
| **\-c**, **\--changes** | Similar to **\--verbose** mode, but only displays information about files that are actually changed. For example:
```
changed ownership of 'dir/dir1/file1' from hope:neil to hope:hope


```

 |
| **\-v**, **\--verbose** | Display [verbose](https://www.computerhope.com/jargon/v/verbose.htm) information for every file processed. For example:

```
changed ownership of 'dir/dir1/file1' from hope:neil to hope:hope

ownership of 'dir/dir1' retained as hope:hope
```

 |
| **\-f**, **\--silent**, **\--quiet** | Quiet mode. Do not display output. |
| **\--dereference** | [Dereference](https://www.computerhope.com/jargon/d/dereference-operator.htm) all [symbolic links](https://www.computerhope.com/jargon/s/symblink.htm). If _file_ is a symlink, change the owner of the referenced file, not the symlink itself. This is the default behavior. |
| **\-h**, **\--no-dereference** | Never dereference symbolic links. If _file_ is a symlink, change the owner of the symlink rather than the referenced file. |
| **\--from=**_currentowner_**:**_currentgroup_ | Change the owner or group of each file only if its current owner or group match _currentowner_ and/or _currentgroup_. Either may be omitted, in which case a match is not required for the other attribute. |
| **\--no-preserve-root** | Do not treat **/** (the root directory) in any special way. This is the default behavior. If the **\--preserve-root** option is previously specified in the command, this option will cancel it. |
| **\--reference=**_ref-file_ | Use the owner and group of file _ref-file_, rather than specifying ownership with _new-owner_. |
| **\-R**, **\--recursive** | Operate on files and directories [recursively](https://www.computerhope.com/jargon/r/recursive.htm). Enter each matching directory, and operate on all its contents. |

Recursive options

The following options modify how a [hierarchy](https://www.computerhope.com/jargon/h/hierfile.htm) is traversed when the **\-R** or **\--recursive** option is specified.

| Option | Description |
| --- | --- |
| **\--preserve-root** | Never operate recursively on the root directory **/**. If **\--recursive** is not specified, this option has no effect. |
| **\-H** | If a file specified on the command line is a symbolic link to a directory, traverse it and operate on those files and directories as well. |
| **\-L** | Traverse all symbolic links to a directories. |
| **\-P** | Do not traverse any symbolic links; operate on the symlinks themselves. This is the default behavior. |

If more than one of **\-H**, **\-L**, or **\-P** is specified, only the final option takes effect.

Other options

These options display information about the program, and cannot be used with other options or arguments.

| Option | Description |
| --- | --- |
| **\--help** | Display a brief help message and exit. |
| **\--version** | Display [version](https://www.computerhope.com/jargon/v/version.htm) information and exit. |

Exit status

**chown** exits with a status of **0** for success. Any other number indicates failed operation.

## Why change a file's ownership?

You should use **chown** when you want a file's user or group permissions to apply to a different user or group.

Hypothetical scenarios

Here are some examples of when you might use **chown**:

- You create a file, **myfile.txt**, using **sudo** or while logged in as root, so the file is owned by **root**. However, you intend the file to be used by your regular user account, **myuser**. Use **chown** to change the owner:

```
sudo chown myuser myfile.txt
```

- You own **myfile.txt**, but you want to give it to another user on the system named **notme**. You also want to change the owning group to that user's group, **notmygroup**. Use **chown** to change the owner and group:

```
sudo chown notme:notmygroup myfile.txt
```

- You just transferred an entire directory of files, **otherfiles**, from another computer. All the files and directories are owned by your username on the other system, and you want your current user and group to own them all. Change the ownership of the directory and all its contents [recursively](https://www.computerhope.com/unix/sudo.htm), with the **\-R** option:

```
sudo chown -R myuser:mygroup otherfiles
```

The above command will change the ownership of every file, [subdirectory](https://www.computerhope.com/jargon/s/subdirec.htm), and subdirectory contents in **otherfiles**.

Groups in Linux

In Linux, a user can be a member of multiple groups, but it has only one "current group". The user's current group is the user's **group identity**, or **GID**.

When the user creates a new file, the file's ownership is set to the user's UID (user identity) and GID (group identity). So when user **carla** starts writing a new document, the file is owned by carla, and also by her current group. She can change the file's group ownership with **chown**, but only root can use **chown** to change the owner to someone else.

Also, each user has a configurable **login group**, which can be any of the user groups. So when **carla**logs in, her login group is her current group. The login group can be changed with the [**usermod**](https://www.computerhope.com/unix/usermod.htm)command, using the **\-g** option.

```
sudo usermod -g newlogingroup carla
```

A user can change current group with the [**newgrp**](https://www.computerhope.com/unix/unewgrp.htm) command. The change takes place in a [subshell](https://www.computerhope.com/unix/ubash.htm#command-execution-environment), and persists until the subshell is closed. Even if **carla** changes her current group with **newgrp**, it will be reset to her login group the next time she logs in.

You can check your current group using the [**id**](https://www.computerhope.com/unix/uid.htm) command with the **\-g** option:

```
id -g
```

```
1001
```

This is your numeric GID (the number of your current group). To see the name, specify the **\-n** option:

```
id -ng
```

```
hope
```

To view all of your group memberships, use a capital **G**:

```
id -nG
```

```
hope sudo neil libvirtd vboxusers usergroup
```

By default, every Linux user has a private group, with that user as the only member. So, when the user account **jeff** is created with the [**adduser**](https://www.computerhope.com/unix/adduser.htm) command, a group named **jeff** is also created. Group **jeff** is jeff's default login group, and has only one member (jeff).

Groups in other operating systems

Other operating systems use **chown**, but their groups may function differently.

In [macOS X](https://www.computerhope.com/jargon/m/macosx.htm) and [BSD](https://www.computerhope.com/jargon/b/bsd.htm), for example, users don't have private groups. Instead, all regular users belong to a general group called **users**.

In these operating systems, the options and functionality of **chown** may be similar, but different. If you're using **chown** on a non-Linux operating system, make sure to run **man chown** to learn what the differences are.

## Examples

Viewing ownership

Before you use **chown**, you may want to check the current ownership of a file. You can view a file's ownership, permissions, and other important information with the [**ls**](https://www.computerhope.com/unix/uls.htm) command, using the **\-l** option:

```
ls -l myscript.sh
```

```
-rwxrw-r-- 1 hope hopeusers 12 Nov  5 13:14 myscript.sh
```

In the output, you see several fields of information listed, including the permissions and ownership of the file. It might not make sense at first, so let's describe it in detail.

Here's what the information means:

| Data | Field position | Description |
| --- | --- | --- |
| **\-** | Field **1**, [character](https://www.computerhope.com/jargon/c/charact.htm) **1** | **File type**: **d** for a [directory](https://www.computerhope.com/jargon/d/director.htm), **l** (lowercase L) for a symbolic link, or **\-** (a dash) for a regular file. |
| **rwx** | Field **1**, characters **2**\-**4** | **User permissions**. The owner can read ("**r**"), write to ("**w**"), and [execute](https://www.computerhope.com/jargon/e/execute.htm)("**x**") this file. |
| **rw-** | Field **1**, characters **5**\-**7** | **Group permissions**. The owning group can read and write to this file, but cannot execute it as a command. |
| **r--** | Field **1**, characters **8**\-**10** | **Other permissions**, also known as **world** permissions. Any other user on the system is allowed to read the file only. |
| **1** | Field **2** | **Number of symbolic links** to this file. If there are no symbolic links to the file, this number is **1**, because the original file name is included in this count. If there were one symbolic link to the file, this number would be **2**, or **3** for two symbolic links, etc. |
| **hope** | Field **3** | **Name of owner**. This is the name of the user who owns the file. When this user tries to access the file, access is restricted according to the **user permissions**. |
| **hopeusers** | Field **4** | **Name of owning group**. This is the user group who owns the file. When a user who is a member of this group tries to access the file, access is restricted according to the **group permissions**. |
| **12** | Field **5** | **Size**. This file contains **12** [bytes](https://www.computerhope.com/jargon/b/byte.htm) of data. |
| **Nov** | Field **6** | **Mtime (month)**. Abbreviated name of the month when the file's contents were last modified. This file was last modified in the month of November. |
| **5** | Field **7** | **Mtime (day of month)**. This file was last modified on the fifth day of November. |
| **13:14** | Field **8** | **Mtime (time, or year)**. This file was last modified at **13:14** (1:34 P.M.) on November 5 of this year. If it was modified over a year ago, this field would list the year instead, for instance **2015**. |
| **myscript.sh** | Field **9** | [**File name**](https://www.computerhope.com/jargon/f/filename.htm). The name of the file. |

So the important fields here are 1, 3 and 4. They tell us that user **hope** can read, write, or execute the file's contents, and members of the group **hopeusers** can read or write to it.

Changing ownership

```
sudo chown hope file.txt
```

Change the owner of **file.txt** to user **hope**.

```
sudo chown hope file1 file2 file3
```

Change the owner of **file1**, **file2**, and **file3** to user **hope**.

```
sudo chown hope file*
```

Here, the asterisk ("**\***") is a [wildcard](https://www.computerhope.com/jargon/w/wildcard.htm) which the [shell](https://www.computerhope.com/jargon/s/shell.htm) expands to a list of every file whose name begins with "**file**". If the [current directory](https://www.computerhope.com/jargon/c/currentd.htm) contains four files named **file1**, **file2**, **file3**, and **file4**, all these files' names are passed to the **chown** command, and their owners changed to user **hope**.

```
sudo chown hope myfiles
```

Change the owner of file or directory **myfiles** to user **hope**.

```
sudo chown -R hope myfiles
```

Change the owner of **myfiles** to user **hope**. If **myfiles** is a directory, **chown** will recursively (**\-R**) search that directory, and change the owner of all files, subdirectories, and subdirectory contents.

```
sudo chown hope:admins file1 file2
```

Change the owners of **file1** and **file2** to user **hope**, and the owning groups to **admins**.

```
sudo chown hope: file1
```

Change the owner of **file1** to user **hope**, and the owning group to hope's login group.

```
chown :othergroup file2
```

Change the owning group of **file2** to group **othergroup**. Notice that this is the only command in these examples which may be run without **sudo**.

If user **hope** runs the previous command but does not belong to group **othergroup**, the command will fail, unless it is run with **sudo**.

```
sudo chown 1000:1001 file1
```

Change the ownership of **file1** to the user with numeric UID **1000**, and the group with numeric GID **1001**.

```
sudo chown +1000:+1001 file1
```

Same as the previous command. If user **hope** has UID 1000, and another user is named "1000" but has UID 1002, this command form (with the "**+**" signs) unambiguously changes the owner to **hope**.

```
sudo chown -R hope:hope Documents
```

Recursively change the ownership of directory **Documents**, and all files and subdirectories therein, to user **hope**, group **hope**.

```
sudo chown -Rc --reference /home/hope/inbox ~/Documents/work
```

Recursively change the ownership of the directory **~/Documents/work**, and all files and subdirectories therein, to match the ownership of the file or directory **/home/hope/inbox**.

In the above command, **~** (a [tilde](https://www.computerhope.com/jargon/t/tilde.htm)) is an [alias](https://www.computerhope.com/unix/ualias.htm) in [bash](https://www.computerhope.com/unix/ubash.htm) which represents your [home directory](https://www.computerhope.com/jargon/h/homedir.htm). Your home directory can also be represented by the [environment variable](https://www.computerhope.com/jargon/e/envivari.htm) **$HOME**, as in **$HOME/Documents/work**.

Also, if any files change ownership (**\-c** option), information will be printed to [standard output](https://www.computerhope.com/unix/uchown.htm):

```
changed ownership of 'dir/file2' from neil:neil to hope:hope

changed ownership of 'dir/dir1/file1' from susie:susie to hope:hope

changed ownership of 'dir/dir1' from judy:judy to hope:hope

changed ownership of 'dir/dir2/file2' from jeff:jeff to hope:hope

changed ownership of 'dir/dir2' from carla:carla to hope:hope

changed ownership of 'dir/file1' from steve:steve to hope:hope

changed ownership of 'dir' from grace:grace to hope:hope
```
