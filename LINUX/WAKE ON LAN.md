### WOL USES MAC ADDRESSES
The concept of IP addresses are handled by software, the IP address of a card is generated via software when the PC is up. While MAC addresses are STORED in a chip inside the Network card, and will still be the same even when the PC is in stand by.

Since the MAC address is always the same, if I send the magic package it will be intecepted even if the PC is in Stand-By (and it doesn't have an IP address)

## RASPBERRY PIE
From the raspberry pie: 

### 1. install the wakeonlan software:
```
sudo apt update
sudo apt install wakeonlan
```

### 2 Send the magic packet
```
wakeonlan <MAC_ADDRESS>
```

This should work if the mac address is correct


## CHECK SERVER STATUS
To check the server status (on / off), we can use the ping or nmap : 

```
ping -c 1 <SERVER_IP>
```

```
nmap -sn <SERVER_IP>
```

