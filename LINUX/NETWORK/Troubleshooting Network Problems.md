I had a problem today with my Linux server, this is a good way to learn about networking and configurations in linux.

## THE PROBLEM
 I rebooted my server via `ssh`, thinking that everything should work normally, but the server was unreachable! Couldn't connect to the server even using IP address.

Fortunately I can easily connect a monitor, mouse and keyboard to my server, so I can enter the server, and I can see that the server is connected, but can not reach the internet.

### Context
A little context:
My server use NetworkManager to handle networking.
I set the Server to have a static IP address.
I also use a self hosted DNS server (PiHole)

## Troubleshooting
Of course the first thing I have tried to do is to ping google.com with no result, then I have tried with another pc in the network but nothing.

Next step is to 

```
ip a
```
To check networks, and I noticed that there is not `ip` address for the esp3s0 network. Now I know that it means that the connection is not active, then I didn't know that.

Since my server use NetworkManager I have tried to : 

```
sudo systemctl restart NetworkManager
```
with no result, the problem is not there (also rebooting the PC didn't help)

Next step is : 

```
nmcli connection show
```
`nmcli` (NetworkManager Command Line Interface) is a command-line tool that allow users to interact with NetworkManager. With it I can configure, monitor, and troubleshooting network interfaces.

```
nmcli connection show
```
that give us the list of all "active" connection, the network is not there (of course).


while :

```
nmcli device status
```
Give us the list of all devices with state connection.

## Configuration file
So, the first thing to do is understand why NetworkManager is not managing the network.

```
sudo nano /etc/NetworkManager/NetworkManager.conf
```
It seems that is managed:

```
[main]
plugins=ifupdown,keyfile

[ifupdown]
managed=true
```

So that is not the problem then...

Let's try another configuration file : 

```
ls /etc/NetworkManager/system-connections/
enp3s0.nmconnection
```

Now we have the filename that contains the config, so that we can : 
```
sudo nano /etc/NetworkManager/system-connections/enp3s0.nmconnection
```

We are looking for something like `unmanaged=true` but no luck with it ! 
## Checking Logs for Errors

We can use the journalctl to check for errors : 

```
journalctl -u NetworkManager | grep enp3s0
```

## THE SOLUTION
Finally, the only intelligent thing to do was : 

```
nmcli connection up enp3s0
```

This just started the connection, all configuration were good, but for some reason (that I still do not know) the connection was down.

Rebooting the server actually didn't activate the connection, as we can see if we do : 

```
nmcli connection show enp3s0

...
connection.autoconnect: no
...
```

Autoconnection is deactivated, that is why rebooting got the problem.

In order to change that we can use the cli tool : 

```
sudo nmcli connection modify enp3s0 connection.autoconnect yes
```

and the problem is solved.