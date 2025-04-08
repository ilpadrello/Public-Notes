So that BOT would not easy life knowing the port.

### Debian
```
sudo nano /etc/ssh/sshd_config
```

You should find something like : 

```
#Port 22
#AddressFamily any
#ListenAddress 0.0.0.0
```

The default SSH port is 22 let's change that : 

```
Port 49656 # a port between 49152 and 65535
#AddressFamily any
#ListenAddress 0.0.0.0
```

We need to restart the ssh deamon :

```
sudo systemctl restart sshd
```

### Ubuntu 23.04
When working with Ubuntu 23.04 Restarting `sshd` is a little tricky, look online



