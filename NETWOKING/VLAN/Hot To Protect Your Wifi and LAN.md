VLAN are not only for stuff outside your network : 

Even if all the devices are connected in the e same LAN you can have logical separated network ! 
So that you have two sub-networks in the same network.

### Why you would create VLAN ?

**Home**
Let's say you have cameras, or important stuff that you do not want them connected to the internet. You may also want to monitor, or reduce bandwidth to you children devices.
Why should you dishwasher talk to your PC ???

**Work**
It make Network segmentation way easier.
You could add roles to groups in your `vlan`, debug stuff easier, make sure that data is only accessible only to authorized users etc.

### How VLAN works ?
Vlan operate way down at the data link layer (layer 2), and they work by stamping or tagging network frames with a `vlan` identifier (or ID) as they pass through a network switch or router.
This id is nomally an integer (1-2-3...)
When a device sends out a data frame, the network switch adds the VLAN ID of that frame, and when it receive a frame it reads the VLAN id and fowards it to the appropriate VLAN.
This tagging process ensure that data is correctly routed and do not exit the VLAN network

### How the device know in witch VLAN it is since is all connected ?
This is done in you router.
You can specify that a specific port is part of a specific VLAN.
For the Wifi is also in the configuration.

## Different types of VLAN:

## Port-based VLANs
Those are common, where the membership of a VLAN is based on the Switch physical port you are connected in. All the devices connected to those ports are automatically assigned to a specific VLAN.

### Protocol-based VLAN
This is where VLAN membership is based on the protocol used by the client: 
This allows for more Dynamic and flexible network segmentation.
The most common scenario  is perhaps, for home user, to place all the wifi-traffic from a particular SSID (wifi) onto its own VLAN so that traffic can be managed centrally.

Done properly everyone on the guest Wi-Fi shoiuld also be in their own VLAN

### Tag-based VLANs
This type of VLAN uses tags like the 802.1q standard (I have no idea what this is) to determine VLAN membership.
This approach is more flexible an scalable especially in larger networks with dynamic requirements.

Voice VLANS are desined specifically for VoIP, ensuring quality of service by prioritizing voice traffic over all other types.

## Configuring VLANs
