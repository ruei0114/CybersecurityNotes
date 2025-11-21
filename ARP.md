> [!info] LINK & TAG
> [TryHackMe | Networking Essentials](https://tryhackme.com/room/networkingessentials)
> 
> #network

- <font color = "c2e3f4">Address Resolution Protocol</font>
- <font color = "c2e3f4"> translation from layer 3 addressing to layer 2 addressing</font>
- <font color = "c2e3f4">Layer 2</font> [[OSI#^50a055 | Data-Link]]
- <font color = "c2e3f4">two devices on the same Ethernet cannot communicate without knowing each other’s MAC addresses.</font>

```sh
user@TryHackMe$ tshark -r arp.pcapng -Nn 
1 0.000000000 cc:5e:f8:02:21:a7 → ff:ff:ff:ff:ff:ff ARP 42 Who has 192.168.66.1? Tell 192.168.66.89 
2 0.003566632 44:df:65:d8:fe:6c → cc:5e:f8:02:21:a7 ARP 42 192.168.66.1 is at 44:df:65:d8:fe:6c
```

```sh
user@TryHackMe$ tcpdump -r arp.pcapng -n -v 
17:23:44.506615 ARP, Ethernet (len 6), IPv4 (len 4), Request who-has 192.168.66.1 tell 192.168.66.89, length 28 
17:23:44.510182 ARP, Ethernet (len 6), IPv4 (len 4), Reply 192.168.66.1 is-at 44:df:65:d8:fe:6c, length 28
```