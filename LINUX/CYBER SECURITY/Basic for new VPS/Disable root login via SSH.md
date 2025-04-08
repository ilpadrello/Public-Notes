If you have a sudoer user, it is a good practice to stop root login via SSH.

# IMPORTANT : have a backup ssh session
**You MUST be able to log in with your `sudo` user before restarting the sshd service.** If you can not login with your sudo user, you will lock yourself out of your server.

It's good practice to keep one SSH session open while you make these changes, just in case something goes wrong

Log in as the sudoer user, open the SSH config file with `sudo` privileges : 

```
sudo nano /etc/ssh/sshd_config
```

Now we can edit the `PermitRootLogin` to no.
Save and restart sshd. (usually `sudo systelctl restart sshd`)