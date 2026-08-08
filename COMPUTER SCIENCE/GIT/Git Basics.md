---
title: "GIT AND GIT HUB"
date: "2018-10-14"
tags: 
  - "git"
---

```
Links:
https://www.youtube.com/watch?v=HVsySz-h9r4

```

## COMMANDS( Fast )

### git init

Create a Git repository in the current folder.

### git status

View the status of each file in a repository

### git add <file>

Stage a file for the next commit

### git commit -m "Message"

commit staged files with a descriptive message

### git commit --amend

It will [open an editor](https://thoughtbot.com/blog/visual-ize-the-future) with the last commit message, so you can modify it. After saving, a new commit will be created with the same changes and the new message, replacing the commit with the previous message.

This can be useful to include files you forgot to track, or include modifications to the files you just commited.

## `git mergetool`

This is a useful command to run when rebasing or merging (if you have conflicts while doing so)

[https://git-scm.com/docs/git-mergetool](https://git-scm.com/docs/git-mergetool)

### git log

view a repository commit history

### git config --global user.name "<name>"

Define the author name to be used in all repositories

### git config --global user.email <email>

Define the author email to be used in all repositories.

### git tag -a v1.0(<tagname>) -m "Stable version of the website" ("<description>")

This creates a tag called v1.0

### git revert 514fbe7 <commit tag>

This revernt to a specific commit. When using git revert, remember to specify the commit that you want to undo—not the stable commit that you want to return to. It helps to think of this command as saying “undo this commit” rather than “restore this version.”

### git reset --hard

This is like a reset button to the last commit, all changes you have done since will be erased, This changes all tracked files to match the most recent commit. Note that the --hard flag is what actually update the files. Running git reset without any flags will simply unstage the file, leaving his content as it is. In either case, git reset only oprates on the working directory and the staging area, so our git log history remains unchanged. **Be careful** with git reset and git clean both operate on the working directory, not on the committed snapshots. Unlike git revert they permanently undo changes.

### git clean -f

This will remove all untracked files (remove it physically, no more on the disk). **Be careful** with git reset and git clean both operate on the working directory, not on the committed snapshots. Unlike git revert they permanently undo changes.

### git branch

This shows us all the branches and the one with the \* is the one we are currently working on

### git branch <branchname>

This create a new branch (no need to use " " )

### git rm <filename>

The git rm command tells Git to stop tracking a specific file (and delete it if necessary).

### git merge <branchname>

This git command merge the current commit/branch "checkouted" with the branch name you give.

### git branch -d <branchname>

This git command delete a brunch, usefull if the branch and the master have the same history.

### git remote

Tell us the connections we have to other repositories

### git remote -v

This show us the full path to our original repository, verifying that origin is a remote connection.  
The same path designated as a "fetch" and "push" location.

### git branch -r

This will show all the remote branches.

### git rebase --interactive HEAD~\[<7>\]

The 7 is variable, this actually will rebase (merge) all the last 7 commits into one, the downside is that you have to count all the commits you need to merge, there is another way

### git rebase --interactive \[commit-hash\]

Where the \[commit-hash\] is the hash of the commit JUST BEFORE THE FIRST ONE YOU WANT TO REWRITE FROM, so the one that is in the hash will not be touched. This is actually logical because what it does is actually rebasing in its own base but merging all together with an --intercative rebase.

## GIT AND GIT HUB

Git is a VCS  Version Control System, its power is to save a different version of the software you are making but not only! GIT allows an easy team software development cause you can add branches to your code (if you are developing some new functions for examples) and then unify all the branches in a smart way.

### GIT VS GIT-HUB

Git is the software that allows the versioning while Git-Hub is the server storing all the code

### INSTALLING GIT

in order to work with git you need to install the git bash from this site

```
https://git-scm.com/downloads
```

### GIT BASH

Git comes with its bash, (like Linux terminal), in there you can put the necessary commands to use GIT

**CONFIGURE GIT  
**in order to use Git correctly, it's a good habit to put your username and your email, to do so you need to use the GIT BASH

```
$ git config --global user.name "Simone Panebianco"
$ git config --global user.email "simone.panebianco@gmail.com"
```

You can see the config file just simply with:

```
$ git config --list
```

### NEED HELP?

If you need help with  a "verb" (command) you can do this:

```
$ git help [verb] // $ git help config

or

$ git [verb] --help // $ git config --help
```

### VERSIONING SEMANTICS

One of the most important thing in versioning is the semantic behind:

```
Version 0.1.0
Version 12.0.3
Version 0.2.3-unstable2.simone
```

What 0.1.0 means? And how is different from 12.0.3 or 0.2.3-unstable2.simone ?  
The semantic for versioning is simple:

**Major.Minor.Patch**

**Major:  
**Major updates normally are the one that are incompatible with the older version of the software, very important changes

**Minor:  
**Minors are like just adding new functionalities to the software but the core of the software didn't change

**Patch:**  
Patch are for bugs corrections

Also you can add **\-branches** if you want to add a branch to the code, normally this will be unified after

### GIT COMMANDS

Git comes with the bash, and when you have a bash, of course you have also the commands for the bash

**GIT INIT**Git init  is the basic command to initiate a git versioning on your project, this will create a ".git" folder in your project (it's a hidden folder)

```
$ git init
```

To stop the versioning of this project just remove the folder

**ADD  
**the command "add" will adds (of course) a file to the git repository, to add all the files in the folder just use "add ."  
This command just add the files names to the list of the things we want to save in future

```
$ git add index.html
$ git add .
```

**IGNORE FILES:**

Sometimes there are files that we don't need to Track, like folders with our preferences or system informations  
with GIT we can specify which file we don't want to track.  
To do so we must first create a file called gitignore

```
$ touch .gitingnore //touch is the command to create a file and the dot make the file invisible
```

Now we just need to open this file with a text editor and we can put inside the files name that we want to ignore:

```
.DS_Store
.Project
*.js // This can be used to ignore all the files with the .js extention (or others extentions .pyc .txt etc etc)
```

We could also say to ignore the .gitignore file but actually, its a better way to have this committed (tracked)

**STATUS  
**Status is a command the allows to see the status file:

```
$git status

///Example of response
On branch master
No commits yet

Changes to be committed:
(use "git rm --cached file..." to unstage)
new file: index.html //this tell us that the index.html file is traked
new file: test.html

```

**RM**

**CHECKOUT**

**COMMIT  
**

So here is where the actual work happens: So we have put all the files we want to track on the staging area (the place where all the file that we want to track are listed), we have made a status call and we have seen that result is

```
new file : index.html
nex file : test.html
```

This is because in the staging area the files are known as new files, those files don't exist in the backup but with the add command they are ready to but committed to the backup.  
So now we can run

```
$ git commit -m "initial commit" // -m stands for message or comment
```

The commit command does the job of tracking the file and save all the files information inside the .git folder  
with the "-m" parameter we add a message to this commit, this is very very important cause when we commit the files, when we change the code, is very important to  track what change we made exactly! For now this is our first commit so we don't need to put anything  important inside, but usually, you will put stuff like: "added this function" or "this bug corrected" etc etc

Now if we modify one of our files we can see in the status command that:

```
modified : namefile.html
```

this is  the core of the Git hub functionality  
The program knows what have changed and if we get another commit we can add those changes to our .git repository

### LOG

log command is an usefull command that log all the commit made in the git folder

```
commit de81002dee84317e09e403bc610900d67f1a9177 (HEAD -> master)
Author: Simone Panebianco <simone.panebianco@gmail.com>
Date: Sun Oct 14 16:56:57 2018 +0200

my second commit to test changes

commit c925b760242b14e949f83cb2dcea84510f34a407
Author: Simone Panebianco <simone.panebianco@gmail.com>
Date: Sun Oct 14 16:52:25 2018 +0200

initial commit test1
```

Every commit has an hash number that is like an id there are not 2 equals, and the name of the author and the date of the commit

### CLONE

This is one of the most usefull piece of code for the most used way of GIT!  
The most use of git is to clone a repository with the software and start making changes!

This is it's symple way for the command:

```
$ git clone [url] [where to clone]
$ git clone http://www.githubetcteee.com/etc/etc . // dot means in the current directory
```

Now all the file of the software are cloned to your local directory

**INFORMATION?**

If you want to see information about the repository you cloned you can use the commands :

```
$ git remote -v
$ git branch -a
```

```
$ git remote -v
origin https://github.com/rmccue/test-repository.git (fetch) 
origin https://github.com/rmccue/test-repository.git (push)
//This is where we have taken the repository
```

```
$ git branch -a
* master
 remotes/origin/HEAD -> origin/master
 remotes/origin/master
//This will give us all the branches of the repository Locally and Remotly
```

### DIFF

With the command "diff" we can know all the changes that we made in the files:

```
$ git diff
diff --git a/test.html b/test.html
index 68da700..a8574c4 100644
--- a/test.html
+++ b/test.html
@@ -1,3 +1,3 @@
 <html>
- this is a test //this has been removed
+ this is a test for good // this has been added 
 </html>
\ No newline at end of file
```

### PULL AND PUSH

Now that you have your code cloned to your local machine, you have made some changes and maybe you want to upload the changes that you have made in the main reposistory so everyone can profit of your modifications!  
To do so you have 2 commands PULL and PUSH!!!  
Those 2 commands don't do the same thing, PULL actually Incorporates changes from a remote repository into the current branch, this means that if while you were working on a clone of the project, someone pushed some changes on the code you will download those changes before PUSHING yours, this is important for safety reasons

```
$ git pull
Already up to date.
```

Now we can UPLOAD our code with no problem of consequences:

## BRANCHING

branching is very very important when using Git! until Now we have talked about the "master" branch that is the default branch of a project.  
Branching are the final component of Git version control. This give us four core elements to work with throughout the rest of this tutorial

- The Working Direcotry
- The Staged Snapshot
- Committed Snapshots
- Development Branches

In Git, a branch is an independent line of development. For example, if you wanted to experiment with a new idea without using Git, you might copy all of your project files into a new folder and start making changes, and if you like the result, you could copy the affected files back into the original project. Otherwise, you would simply delete the entire experiment and forget about it.

This is the exact functionality offered by Git branches - With some key improvements! :D  
First, branches present an error-proof method for incorporating changes from an experiment. Second, they let you store all of your experiments in a single directory, which makes it much easier to keep track of them and to share them with others. Branches also lend themselves to several standardized workflow for both individual and collaborative development.

### View existing branches

Let's start by listing our branches:

```
git branch
```

With this command we have all the branches of our project and the \* tell us what is currently checked out, if this is "master" this means that in our directory we have the most recent snapshot in the master branch, if is not master this means that we have the most recent snapshot in that specific branch and if it is "HEAD detached at <commit>" this means that we are working to a specific commit that is not the last one...

Branches are very useful when you want to add features, and resolve bugs in your code, you create a new branch and then start working for solve the problem or add feature, and then you can MERGE in the master but we will look at this later

### Creating a branch

To create a new branch use the code:

```
git branch <branchname>
```

Warning this command create a branch but not checkout it!  
Notice how git branch is a versatile command that can be used to either list branches or create them.

### Merging a branch

in order to merge a branch with the master you need to checkout the master and then run :

```
git merge <branch_name>
```

Always merge branches from where you want the branch to connect with, in this case the master.

### Deleting a brunch

When we merge with a master we may want to delete the brunch that we just created because it is not needed anymore. This is good if the brunch and master have the same history (like when you do a fast forward merge),

```
git branch -d <branchname>
```

Deleting a brunch is a relatively "safe" operation, in the sense that Git will warn you if you're deleting an unmerged branch. This is just another example of Git's commitment to never losing your work

## REBASING

## WORKING ON REMOTE

Until now, we have worked only in local, but the point of using such a powerful tool is to cooperate while creating projects, so we need to understand how remote versioning work.  
  
First create a project (git init, etc) or use one that you use for testing and then go back by one folder (go to the folder that contains your project) and clone it:

```
git clone my-git-repo ilpadrello-repo
```

This will clone the repo, and will set the origin of the cloned repo.  
Now if you want to test working with different account, you need to simulate a different person, to do so you can write in your git bash:

```
git config user.name "ilpadrello"
git config user.mail "ilpadrello@gmail.com"
```

If you notice we didn't use the --global flag so this new configuration will be valid only for this repo/folder, and that is why if you open the config file inside the .git folder you will see the new information in the bottom of the file. So with this trick, we will work as "ilpadrello".

Now we can start creating stuff to the new project, and to do so we really should use a different branch so :

```
git checkout -b bio-page
```

Now we can create a new bio page for our user "ilpadrello" and then commit the changes.  
When we have finished our work (and we have tested it) we can merge the branch to the master (in our cloned repo not in the origin yet)

```
git checkout master
git merge bio-page
```

So now the master in our cloned repo is different from the original one.

Now we can list the connection that "ilpadrello" has with this command:

```
git remote
```

As you can see we have a remote called origin.  
When we clone a repository, Git automatically adds an origin remote pointing to the original repository, under the assumption that you'll probably want to interact with it down the road.  
We can request a little more info about the connection with the verbose -v flag:

```
git remote -v
```

This will show us the full path to our original repository, verifying that origin is a remote connection.  
The same path is designated as "push" and "fetch" but we will see this later.

### Return being yourself

Now that we have done being "ilpadrello" you can just go back to the original repo (cd .. etc), the modifications that we have done with the other account are not here.

### Add ilpadrello as a Remote (You)

Before we can get ahold of ilpadrello modifications, we need access to the repository. Let's have a look at our current list of remotes:

```
git remote 
```

We don't have any origin (origin was never created because we didn't clone form anywhere). So, let's add "ilpadrello" as a remote repo.

```
git remote add mary ../ilpadrello-repo
git remote -v
```

We can now use mary to refer to ilpadrello's repository, which is located at ../marys-repo. The git remote add command is used to bookmark another Git repository for easy access.

Now we can discuss remote branches.

### Fetch ilpadrello's branches(you)

As noted earlier, we can use remote branches to access snapshots from another repo. Let's take a look at our current remote branches with the -r flag:

```
git branch -r
```

Again we don't have any.  
To populate our remote branch listing, we need to **fetch** the branches from ilpadrello's repo

```
git fetch ilpadrello
git branch -r
```

This will go to the "fetch" location showing in git (before) and download all the branches it finds there into our repo. The rsulting branches are shown below:

```
ilpadrello/bio-page
ilpadrello/master
```

Remote branches are always listed in the form <remote-name>/<branch-name> so that they will never be mistaken for local branches.  
Our remote branches are not direct links into ilpadrello's repo, they are read-only copies of her branches, stored in our own repo. This means that we would have to do another fetch to access any update.

### Checkout a Remote branch

Let's checkout a remote branch to review ilpadrello's changes:

```
git checkout ilpadrello/master
```

This puts us in a detached HEAD state, and this should not be a surprise since our remote branches are copies.

### Find the changes

```
git log master..ilpadrello/master --stat
```

This shows us what ilpadrello has added to her master branch, but it's also a good idea to see if we've added any new changes taht aren't in ilpadrello repo :

```
git log ilpadrello/master..master --stat
```

In our case this won't output anything since we haven't altered our database. Our history hasn't diverged, we are just behind a commit.

### Merge the changes

Let's approve the changes and integrate them into our own master branch.

```
git checkout master
git merge ilpadrello/maste
```

### Pushing a dummy page

To complete the tour git fetch command, we'll take a brief look at pushing. Fetching and pushing are almost opposite, in that fetching import branches while pushing export branches to another repo.
