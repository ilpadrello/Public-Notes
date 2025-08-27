So that BOT would not easy life knowing the port.
(More info about why down)

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


### WHY ?
Changing the port **is worth it** because:
- It massively reduces log spam & brute-force attempts especially for bots.
- It costs you nothing.
But you should **not rely on it alone**. It’s a _noise filter_, not a _lock_