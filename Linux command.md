> [!info] LINK & TAG
> [What the shell(piocResources)](https://primer.picoctf.com/#_what_the_shell)
> [picoPrimer](https://primer.picoctf.com/)
> [explainshell](https://explainshell.com/)
> [[Linux Shell]]
> [[Linux Script]]
> #ctf #cybersecurity #Linux #shell

^95af2f

> [!example] Table of contents
> - [[#Tab]]
> -  [[#Comment]]
> - [[#sudo apt]]
> -  [[#cd]]
> - [[#pwd]]
> - [[#mkdir]]
> - [[#touch]]
> - [[#ls]]
> - [[#cat]]
> - [[#echo]]
> - [[#mv]]
> - [[#whoami]]
> - [[#find]]
> - [[#file]]
> - [[#rm]]
> - [[#cp]]
> - [[#ifconfig]]
> - [[#ip a s]]
> - [[#telnet]]
> - [[#ping]]
> - [[#traceroute]]
> - [[#wget]]
> - [[#whois]]
> - [[#nslookup]]
> - [[#dig]]
> - [[#chmod]]
> - [[#grep]]
> - [[#tcpdump|tcpdump]]
> - 

^28066f

## explain<font color = "5795f3">shell</font>.com

^f37071

- A library or manual of Linux command
- The [LINK](https://explainshell.com/)

---

### Tab
- <font color = "c2e3f4">A nice way to auto-complete a command</font>

---
### Comment
- <font color = "c2e3f4">Text after</font> <font color="f2f4ef">#</font>
```
#This is a comment
```

---

### sudo apt
- <font color = "efb730">sudo apt update</font>    <font color = "c2e3f4">更新套件資訊與最新列表</font>
- <font color = "efb730">sudo apt upgrade</font>    <font color = "c2e3f4">更新已安裝的套件到最新版本</font>
- <font color = "efb730">sudo apt install+[package name]</font>    <font color = "c2e3f4">安裝指定套件</font>
- <font color = "efb730">sudo apt purge+[package name]</font>    <font color = "c2e3f4">刪除指定套件</font>

---

### cd
- <font color = "c2e3f4">Go to home directory</font>
```
users:~/download/test/inside_test$ cd
users:~$
```
- <font color = "c2e3f4">Go to some next level directory</font>
	-  <font color = "efb730">cd+[directory name]</font>
```
users:~$ cd download
users:~/download$ 
```
- <font color = "c2e3f4">Go to upper directory</font>
	- <font color = "efb730">cd ..</font>

---
### pwd
- <font color = "c2e3f4">Show (print under the command) where your shell parked</font>
```
users:~/download/test/inside_test$ pwd
home/users/download/test/inside_test
users:~/download/test/inside_test$
```

---
### mkdir
- <font color = "c2e3f4">Make a new directory</font>
- <font color = "efb730">mkdir+[new directory name]</font>
---
### touch
- <font color = "c2e3f4">create an empty file</font>
- <font color = "efb730">touch+[new file name]</font>

---
### ls
- <font color = "c2e3f4">list under this directory</font>
- <font color = "c2e3f4">List out with column</font>
	- <font color = "efb730">ls -1</font>
- <font color = "c2e3f4">use listing format to show all file (including the hinden ones)</font>
	- <font color = "efb730">ls -a</font>
---
### cat

^da7cfc

- <font color = "c2e3f4">Show text content of the file</font>
- <font color = "efb730">cat+[file name]</font>

---
### echo
- <font color = "c2e3f4">Put some text into a file. If the file is not exist, make a new file. </font>
- <font color = "efb730">echo+["some text"]+[>]+[file name]</font>
```
users:~$ echo "hello world" > test.txt
```

---
### mv
- <font color = "c2e3f4">move file(s) to directory</font>
- <font color = "efb730">mv+[file1] [file2] ... ... +[directory]</font>
- <font color = "c2e3f4">can also be used to rename a file</font>
- <font color = "efb730">mv+[name] [new_name]</font>
```
users:~$ mv file1 file2 file3 new_directory
```

---

### whoami
- <font color = "c2e3f4">print effective userid</font>
- <font color = "efb730">whoami</font>

---

### find
- <font color = "c2e3f4">search for files in a directory hierarchy</font>
- <font color = "c2e3f4">-iname ignore capitalization</font>
- <font color = "efb730">find+[path]+[-iname]+[file name]</font>
- <font color = "efb730">find+[path]+[-iname]+["*.extension"]</font>
```
users:~$ find . -iname "*.txt"
```

---

### grep
- <font color = "c2e3f4">search for any word or pattern inside a file</font>
- <font color = "efb730">grep+[text]+[file]</font>
```bash
users:~$ grep THM dictionary.txt
```

---

### file
- <font color = "c2e3f4">determine file type</font>
- <font color = "c2e3f4">副檔名只是告訴電腦該用什麼應用程式開啟</font>
- <font color = "efb730">file+[file name]</font>
```
users:~$ file "hello.txt"
```

---

### rm
- <font color = "c2e3f4">remove files or directories</font>
- <font color = "efb730">rm+[file name]</font>
- <font color = "c2e3f4">recursively remove all files in directory</font>
- <font color = "efb730">rm+[-r]+[directory]</font>

---

### cp
- <font color = "c2e3f4">copy files</font>
- <font color = "efb730">cp+[file name]</font>
- <font color = "c2e3f4">copy the directory and recursively copy all files in directory</font>
- <font color = "efb730">cp+[-r]+[directory]</font>

---

### ifconfig
- <font color = "c2e3f4">(old)look up your IP address</font>

---

### ip a s

^ea9a33

- `ip address show` <font color = "c2e3f4">for short</font>
- <font color = "c2e3f4">(new)look up your IP address</font>
- <font color = "c2e3f4">see</font> [[IP]] <font color = "c2e3f4">for more</font>

---

### telnet

^d4b1c2

- <font color = "c2e3f4">port 23</font>
- <font color = "c2e3f4">application layer protocol</font>
- <font color = "c2e3f4">connect to a server</font>
- <font color = "c2e3f4">communicate with a remote system and issue text commands</font>
- <font color = "efb730">telnet+[ip address]+[port number]</font>
	- Echo server: This server echoes everything you send it. By default, it listens on port 7.
	- Daytime server: This server listens on port 13 by default and replies with the current day and time.
	- Web (HTTP) server: This server listens on TCP port 80 by default and serves web pages.
- <font color = "c2e3f4">press the</font> `CTRL + ]` <font color = "c2e3f4">to close connection</font>

```sh
user@TryHackMe$ telnet 10.201.60.118 7 
telnet 10.201.60.118 7 
Trying 10.201.60.118... 
Connected to 10.201.60.118. 
Escape character is '^]'. 
Hi 
Hi 
How are you? 
How are you? 
Bye 
Bye 
^] 
telnet> quit 
Connection closed.
```

---

### [[ping]]

^290d80

- <font color = "c2e3f4"> test connectivity to a target system and measures the round-trip time (RTT)</font>
```sh
user@TryHackMe$ ping 192.168.11.1 -c 4 
PING 192.168.11.1 (192.168.11.1) 56(84) bytes of data. 
64 bytes from 192.168.11.1: icmp_seq=1 ttl=63 time=11.2 ms 
64 bytes from 192.168.11.1: icmp_seq=2 ttl=63 time=3.81 ms 
64 bytes from 192.168.11.1: icmp_seq=3 ttl=63 time=3.99 ms 
64 bytes from 192.168.11.1: icmp_seq=4 ttl=63 time=23.4 ms 
--- 192.168.11.1 ping statistics --- 
4 packets transmitted, 4 received, 0% packet loss, time 3003ms 
rtt min/avg/max/mdev = 3.805/10.596/23.366/7.956 ms
```
- [[ICMP]]

---

### traceroute

^8c816f

- <font color = "c2e3f4">discover the route from your host to the target.</font>
- [[ICMP]]
---
### wget

^011625

- <font color = "c2e3f4">The [non-interactive](https://askubuntu.com/questions/266387/what-exactly-is-non-interactive-program) network downloader</font>
- <font color = "efb730">wget + [URL of item]</font>

---

### whois

^f2a134

- <font color = "c2e3f4"> look up the WHOIS records of any registered domain name</font>
- <font color = "efb730">whois+[domain]</font>
```
user@TryHackMe$ whois twitter.com
```

---

### nslookup

^798710

- <font color = "c2e3f4">Name Server Look Up</font>
- <font color = "efb730">nslookup+[-type=[type]]+[domain]+[DNS server]</font>
```sh
user@TryHackMe$ nslookup -type=A tryhackme.com 1.1.1.1
```

---

### dig

^4f7301

<font color = "c2e3f4">Domain Information Groper</font>
- <font color = "efb730">dig+@[SERVER]+[DOMAIN_NAME]+[TYPE]</font>
```sh
user@TryHackMe$ dig tryhackme.com MX
```

---
### chmod

^1288d7

- [Reference](https://www.runoob.com/linux/linux-comm-chmod.html)

---
### grep

^e1a2c7

- <font color = "c2e3f4">Print lines matching a pattern (to find particular text)</font>
- [Reference](https://blog.gtwang.org/linux/linux-grep-command-tutorial-examples/)
- <font color = "efb730">grep + [target] + [file]</font>

---

### [[tcpdump]]
<font color = "c2e3f4">capturing network traffic and taking a closer look at the various protocols</font>

---
### [[nmap]]
- <font color = "c2e3f4"> Nmap is an open-source network scanner</font>

---

### netcat

^d5ff4a

- <font color = "c2e3f4">get target server information</font>
- <font color = "efb730">nc+[target IP]+[Port]</font>
- shift+enter
- `GET / HTTP/1.1`
- `host: netcat`
```sh
pentester@TryHackMe$ nc 10.49.147.59 80
```
---
- <font color = "c2e3f4">listen on a TCP port and connect to a listening port on another system(like a server)</font>
- <font color = "efb730">nc+[-vnlp]+[Port]</font>

| option | meaning                                                    |
| ------ | ---------------------------------------------------------- |
| -l     | Listen mode                                                |
| -p     | Specify the Port number                                    |
| -n     | Numeric only; no resolution of hostnames via DNS           |
| -v     | Verbose output (optional, yet useful to discover any bugs) |
| -vv    | Very Verbose (optional)                                    |
| -k     | Keep listening after client disconnects                    |


---

### curl

---
### wc
- 

---
### steghide

^0d4cdb

---
### exiftool

^87706e

---

### [[ffuf]]


---

