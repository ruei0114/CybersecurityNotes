#network
- <font color = "c2e3f4">windows :</font> `ipconfig`
- <font color = "c2e3f4">UNIX-based :</font>`ifconfig`, `ip a s`

---

```sh
user@TryHackMe$ ip a s 
[...] 
	4: wlo1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000 
	link/ether cc:5e:f8:02:21:a7 brd ff:ff:ff:ff:ff:ff 
	altname wlp3s0 
	inet 192.168.66.89/24 brd 192.168.66.255 scope global dynamic noprefixroute wlo1 
		valid_lft 36795sec preferred_lft 36795sec 
	inet6 fe80::73e1:ca5e:3f93:b1b3/64 scope link noprefixroute 
		valid_lft forever preferred_lft forever
```
- host address : 192.168.66.89/24
- broadcast address : 192.168.66.255
- `/24` means that the leftmost 24 bits within the IP address do not change across the network

---

RFC 1918 defines the following three ranges of private IP addresses:

- `10.0.0.0` - `10.255.255.255` (`10/8`)
- `172.16.0.0` - `172.31.255.255` (`172.16/12`)
- `192.168.0.0` - `192.168.255.255` (`192.168/16`)