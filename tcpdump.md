> [!info] LINK & TAG
> [TryHackMe | Tcpdump: The Basics](https://tryhackme.com/room/tcpdump)
> 
> #network #shell

|Command|Explanation|
|---|---|
|`tcpdump -i INTERFACE`|Captures packets on a specific network interface|
|`tcpdump -w FILE`|Writes captured packets to a file|
|`tcpdump -r FILE`|Reads captured packets from a file|
|`tcpdump -c COUNT`|Captures a specific number of packets|
|`tcpdump -n`|Don’t resolve IP addresses|
|`tcpdump -nn`|Don’t resolve IP addresses and don’t resolve protocol numbers|
|`tcpdump -v`|Verbose display; verbosity can be increased with `-vv` and `-vvv`|

### NOTE
`tcpdump -i INTERFAEC` : use [[Linux command#^ea9a33|ip a s]] to see available interfaces

---

## Filtering
### Filtering by port
- filtering packet by DNS(port 53) query
```sh
user@TryHackMe$ sudo tcpdump port 53 -n
```

### Filtering by Protocol
- filtering packet by protocol ICMP
```sh
user@TryHackMe$ sudo tcpdump icmp -n
```

---

### Logical Operators

Three logical operators that can be handy:

- `and`: Captures packets where both conditions are true. For example, `tcpdump host 1.1.1.1 and tcp` captures `tcp` traffic with `host 1.1.1.1`.
- `or`: Captures packets when either one of the conditions is true. For instance, `tcpdump udp or icmp` captures UDP or ICMP traffic.
- `not`: Captures packets when the condition is not true. For example, `tcpdump not tcp` captures all packets except TCP segments; we expect to find UDP, ICMP, and ARP packets among the results.

---

### Read packet from file
```sh
user@TryHackMe$ tcpdump -r traffic.pcap src host 192.168.124.1 -n | wc 
reading from file traffic.pcap, link-type EN10MB (Ethernet) 
910 17415 140616
```
- use `-r FILE` to read from a packet capture file
- count the lines by piping the output via the `wc` command
	- we have 910 packets with the source IP address set to `192.168.124.1`

---

### Advanced filtering
#### filter by length
- `greater LENGTH`: Filters packets that have a length greater than or equal to the specified length
- `less LENGTH`: Filters packets that have a length less than or equal to the specified length
