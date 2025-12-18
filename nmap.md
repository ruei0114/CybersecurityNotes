> [!info] LINK & TAG
> [TryHackMe | Nmap: The Basics](https://tryhackme.com/room/nmap)
> [TryHackMe | Nmap Live Host Discovery](https://tryhackme.com/room/nmap01)
> [TryHackMe | Nmap Basic Port Scans](https://tryhackme.com/room/nmap02)
> [TryHackMe | Nmap Advanced Port Scans](https://tryhackme.com/room/nmap03)
> 
> #network #shell 

| Option                                                              | Explanation                                                                                        |
| ------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| `-sL`                                                               | List scan – list targets without scanning                                                          |
| [[#**_Host Discovery_**\|Host Discovery]]                           |                                                                                                    |
| -n                                                                  | no DNS lookup                                                                                      |
| -R                                                                  | reverse-DNS lookup for all hosts                                                                   |
| `-sn`                                                               | Ping scan – host discovery only                                                                    |
| -PR                                                                 | ARP scan                                                                                           |
| -PE                                                                 | ICMP Echo Scan                                                                                     |
| -PP                                                                 | ICMP Timestamp Scan                                                                                |
| -PM                                                                 | ICMP Address Mask Scan                                                                             |
| -PS                                                                 | TCP SYN Ping Scan                                                                                  |
| -PA                                                                 | TCP ACK Ping Scan                                                                                  |
| -PU                                                                 | UDP Ping Scan                                                                                      |
| TCP/UDP to specify scan port                                        | `-PA21`, `-PA21-25` and <br>`-PA80,443,8080`                                                       |
| **_Port Scanning_**                                                 |                                                                                                    |
| `-sT`                                                               | TCP connect scan – complete three-way handshake                                                    |
| `-sS`                                                               | TCP SYN – only first step of the three-way handshake                                               |
| `-sU`                                                               | UDP Scan                                                                                           |
| `-F`                                                                | Fast mode – scans the 100 most common ports                                                        |
| `-p[range]`                                                         | Specifies a range of port numbers – `-p-` scans all the ports                                      |
| `-Pn`                                                               | Treat all hosts as online – scan hosts that appear to be down                                      |
| **_Advanced Port Scans_**                                           |                                                                                                    |
| -sN                                                                 | TCP Null Scan                                                                                      |
| -sF                                                                 | TCP FIN Scan                                                                                       |
| -sX                                                                 | TCP Xmas Scan                                                                                      |
| -sM                                                                 | TCP Maimon Scan                                                                                    |
|                                                                     |                                                                                                    |
| **_Service Detection_**                                             |                                                                                                    |
| `-O`                                                                | OS detection                                                                                       |
| `-sV`                                                               | Service version detection                                                                          |
| `-A`                                                                | OS detection, version detection, and other additions                                               |
| **_Timing_**                                                        |                                                                                                    |
| `-T<0-5>`                                                           | Timing template – paranoid (0), sneaky (1), polite (2), normal (3), aggressive (4), and insane (5) |
| `--min-parallelism <numprobes>` and `--max-parallelism <numprobes>` | Minimum and maximum number of parallel probes                                                      |
| `--min-rate <number>` and `--max-rate <number>`                     | Minimum and maximum rate (packets/second)                                                          |
| `--host-timeout`                                                    | Maximum amount of time to wait for a target host                                                   |
| **_Real-time output_**                                              |                                                                                                    |
| `-v`                                                                | Verbosity level – for example, `-vv` and `-v4`                                                     |
| `-d`                                                                | Debugging level – for example `-d` and `-d9`                                                       |
| **_Report_**                                                        |                                                                                                    |
| `-oN <filename>`                                                    | Normal output                                                                                      |
| `-oX <filename>`                                                    | XML output                                                                                         |
| `-oG <filename>`                                                    | `grep`-able output                                                                                 |
| `-oA <basename>`                                                    | Output in all major formats                                                                        |

---

![[Pasted image 20251217220812.png]]

---
## **_Host Discovery_**
- IP range using `-`: If you want to scan all the IP addresses from 192.168.0.1 to 192.168.0.10, you can write `192.168.0.1-10`
- IP subnet using `/`: If you want to scan a subnet, you can express it as `192.168.0.1/24`, and this would be equivalent to `192.168.0.0-255`
- Hostname: You can also specify your target by hostname, for example, `example.thm`

---

- run Nmap with `sudo` privileges : Nmap would automatically use SYN scan (`-sS`)
- default to connect scan (`-sT`) if run as a local user
- Unprivileged user scan (`-PS`, `-sT`) usually have no choice but to establish 3-way handshake if the port is opent. (will show on the log)
