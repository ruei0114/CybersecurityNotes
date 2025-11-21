> [!info] LINK & TAG
> [picoResources](https://picoctf.org/resources.html)
> [picoGym](https://play.picoctf.org/practice)
> [picoPrimer](https://primer.picoctf.com/)
> 
> #ctf #picoCTF #cybersecurity

> [!example] Table of contents
>  <font color = "7e69c4">Practice</font>
>  - [[#Obedient Cat]]
>  - [[#Mod 26]]
>  - [[#Python Wrangling]]
>  - [[#Wave a flag]]
>  - [[#information]]
>  - [[#transformation]]

## <font color = "8080c0">Practice</font>

### Obedient Cat
- [URL](https://play.picoctf.org/practice/challenge/147)
- #general
- <font color = "3aa690">steps</font>
	1. <font color = "fddcef">Use [[Linux command#^011625|wget]] to get the "flag" file</font>
	2. <font color = "fddcef">[[Linux command#^da7cfc|Cat]]"flag" file to get the content</font>

---
### Mod 26
- [URL](https://play.picoctf.org/practice/challenge/144)
- #cryptography
- <font color = "3aa690">steps</font>
	- <font color = "fddcef">Use</font> [[ROT13]]<font color = "fddcef"> to decode the text</font>

---
### Python Wrangling
- [URL](https://play.picoctf.org/practice/challenge/166)
- #general 
- <font color = "3aa690">steps</font>
	1. <font color = "fddcef">[[Linux command#^011625|wget]] the following three files (python script, password, flag)</font>
	2. <font color = "fddcef">Read the python script we can know some function(usage) of the program</font>
	3. <font color = "fddcef">[[Linux command#^da7cfc|cat]] pw.txt to get password</font>
	4.  ```
	 $ python ende.py -d flag.txt.en
	 $ please enter the password: 
	 5. <font color = "fddcef">Successfully get to decoded flag</font>
	 
---
### Wave a flag
- [URL](https://play.picoctf.org/practice/challenge/170)
- #general 
- <font color = "3aa690">steps</font>
	1. <font color = "fddcef">[[Linux command#^011625|wget]] the file(warm)</font>
	2. <font color = "fddcef">From the hint3, we can know that we have to execute the file with the following syntax</font>
		 
		```
		$ ./warm
		```
	3. <font color = "fddcef">To make it executable, we have to [[Linux command#^1288d7|Change mode]]. So the complete syntax would be like</font>
		```
		$ chmod +x warm
		$ ./warm
		```
	4. <font color = "fddcef">Follow the instruction, then you can get the flag</font>
---
### information
- [URL](https://play.picoctf.org/practice/challenge/186)
- #forensics
- [Reference solution](https://hackmd.io/@catkitchen721/Hy0UfEJQY)
- [This video helps a lot](https://www.youtube.com/watch?v=rBw8CDSKinQ)
	-  <font color = "fddcef">In the video we can learn some knowledge (Some of them is not useful here😅)</font>
	- [[Linux command#^e1a2c7|grep]]
	- [[Steganography]]
	- [[Base64]] <font color = "d5c5e9">THE KEY OF THIS PROBLEM!!!</font>
- <font color = "3aa690">steps</font>
	1. <font color = "fddcef">There are two ways to find the information we need</font>
		1. <font color = "fddcef">Use some helpful website([for example](https://www.metadata2go.com/)) to get the</font><font color = "d5c5e9"> Metadata</font> <font color = "fddcef">of the file</font>
		2. <font color = "fddcef">Use [[Linux command#^87706e|exiftool]] command in Linux terminal</font>
	2. <font color = "d5c5e9">"Lisence" </font><font color = "fddcef">and</font> <font color = "d5c5e9">"Current IPTC Digest"</font><font color = "fddcef">are garbled(亂碼)</font>
	3. <font color = "fddcef">Observe </font><font color = "d5c5e9">"Lisence" </font><font color = "fddcef">, it may be [[Base64]]</font>
	4. <font color = "fddcef">Decode it with base64</font>

---
### transformation
- [URL](https://play.picoctf.org/practice/challenge/104)
- #reverse_engineering
- [Reference 1](https://infosecwriteups.com/picoctf-2021-transformation-6242546fba02)
- [Reference 2](https://www.youtube.com/watch?v=xNX6dOS3a_g)
- <font color = "3aa690">steps</font>
	1. <font color = "fddcef">We can see ''.join([chr((ord(flag[i]) << 8) + ord(flag[i + 1])) for i in range(0, len(flag), 2)]) in the statement of this question, and it is a python code</font>
	2. <font color = "fddcef">Check out for the function of "join()", "chr()", "ord()", we know that it's kind of encode</font>
	3. <font color = "fddcef">[[Linux command#^011625|wget]] and [[Linux command#^da7cfc|cat]] the enc file, there is a string"灩捯䍔䙻ㄶ形楴獟楮獴㌴摟潦弸彤㔲挶戹㍽", I thought that it may be encoded</font>
	4. <font color = "fddcef">Tried </font>
		```
		flag=""灩捯䍔䙻ㄶ形楴獟楮獴㌴摟潦弸彤㔲挶戹㍽""
	    print()''.join([chr((ord(flag[i]) << 8) + ord(flag[i + 1])) for i in range(0, 
        len(flag), 2)]))
		```
		
		<font color = "fddcef">it is not a solution </font>
	5.  <font color = "fddcef">Follow the hint and tried several kinds of decoders online, the closest answer is "ioT{6bt_nt4_f8d269}", and don't know what to do next</font>
	6. <font color = "fddcef">Read reference1, found out that step4 is a close step, and knowing that what we have to do is to reverse the process of step4</font>
	7. <font color = "fddcef">Maybe I didn't think clearly, after trying several times, still can't figure out the way to reverse the code</font>
	8. <font color = "fddcef">Watch Reference2, successfully find the way out</font>
		``` python
		flag = "灩捯䍔䙻ㄶ形楴獟楮獴㌴摟潦弸彤㔲挶戹㍽"  
		for i in range(len(flag)):  
		    print(chr(ord(flag[i])>>8))  
		    print(chr((ord(flag[i]))-(ord(flag[i])>>8<<8)))
		
        ```

---
