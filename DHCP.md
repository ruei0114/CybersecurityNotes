> [!info] LINK & TAG
> [TryHackMe | Networking Essentials](https://tryhackme.com/room/networkingessentials)
> 
> #network

![[Pasted image 20251117154501.png]]

```sh
user@TryHackMe$ tshark -r DHCP-G5000.pcap -n 
1 0.000000 0.0.0.0 → 255.255.255.255 DHCP 342 DHCP Discover - Transaction ID 0xfb92d53f 
2 0.013904 192.168.66.1 → 192.168.66.133 DHCP 376 DHCP Offer - Transaction ID 0xfb92d53f 
3 4.115318 0.0.0.0 → 255.255.255.255 DHCP 342 DHCP Request - Transaction ID 0xfb92d53f 
4 4.228117 192.168.66.1 → 192.168.66.133 DHCP 376 DHCP ACK - Transaction ID 0xfb92d53f
```

## When DHCP process done
we expected DHCP server provides
- The leased IP address to access network resources
- The gateway to route our packets outside the local network
- A DNS server to resolve domain names (more on this later)