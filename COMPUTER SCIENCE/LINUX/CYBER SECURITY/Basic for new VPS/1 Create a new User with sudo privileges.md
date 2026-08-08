It is good practice to create a new user with `sudo` privileges, in case the only admin user there is got corrupted for some reason.

1) let's check if we have root privileges

```
$ sudo whoami
``` 

if the answer is `root` you are acting as root when doing sudo.

Let's create a new user :

```
sudo adduser yourusername
sudo usermod -aG sudo yourusername
```

Change user using the `su` command and try `sudo whoami` : you should be root