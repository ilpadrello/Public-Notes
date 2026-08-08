### ⚠ You need to create a file for each user you want to use.

```
ssh-keygen -t rsa -b 4096 -f ~/.ssh/vps_key
```

remove the `-f ~/.ssh/vps_key` or change it if you want to use the default filename or specify a different one.

### Copy the new key to the VPS

```
ssh-copy-id -i ~/.ssh/vps_key.pub -p your_ssh_port yourusername@your_vps_ip
``` 

replace the obvious stuff

Now try to logging into the machine with :

```
ssh -i ~/.ssh/vps_key -p your_ssh_port yourusername@your_vps_ip
``` 

It would be intelligent to add an alias for this

**Important Considerations (Specific to New Keys):**

- **Key File Permissions:** Ensure that your private key file (`~/.ssh/vps_key`) has the correct permissions (usually 600). You can set these with: `chmod 600 ~/.ssh/vps_key`
- **Keep Keys Organized:** Using descriptive filenames for your SSH keys helps you manage multiple connections.
- **Use the `-i` flag:** Whenever you SSH into this VPS, you'll need to use the `-i ~/.ssh/vps_key` flag to specify the correct private key file.
- **Config File (Optional):** You can also add the VPS connection and private key to your `~/.ssh/config` file to avoid having to type the long `ssh -i ...` command every time. Example of adding the config.

```
Host vps_alias
    HostName your_vps_ip
    User yourusername
    Port your_ssh_port
    IdentityFile ~/.ssh/vps_key
```


# ⚠ BEFORE THIS STEP MAKE SURE YOU CAN ACCESS THE SERVER VIA KEYS
This is important, if you don't want to rest outside your server make sure that the access via Keys work as it is supposed to.

Log in to tour VPS using the new SSH key (so that you are sure that it works)
then you can edit the `/etc/ssh/sshd_config`

```
PasswordAuthentication no
```

Try to connect using password now.
## Configuration Override
In my case, I don't know why, my config was override by a file `/etc/ssh/sshd_config.d/50-cloud-init.conf` where the file was setting `PasswordAuthentication yes`

To find all the configurations that can override this I did : 

```
sudo grep -r "PasswordAuthentication" /etc/ssh/sshd_config.d/
```

## Configuration, ssh_config.d vs sshd_config.d

The main difference is that `ssh_confg.d` is a config directory that configure the SSH client, so the outbound connections (like usernames, ports, key files)
While the `sshd_config.d` directory, configure the SSH server (inbound connections), so auth methods, ports and security options.

# Connecting from a new device
If you want to connect from a new device, you have to make a choise :
### Same private key
Use the same private key of the other device, copy and paste and regenerate the new public key with the command `ssh-keygen -f ~/.ssh/id_rsa -y > ~/.ssh/id_rsa.pub`

### Create a New Key pair
You can create a new private a public key from the new device or the old device, then send the public key to the server with `ssh-copy-id`, but you need to re-enable the password connection, or using `scp` from the old device : 

```
scp -P your_ssh_port ~/.ssh/new_vps_key.pub yourusername@your_vps_ip:~/.ssh/new_vps_key.pub
```

This will work also with the no password setting.