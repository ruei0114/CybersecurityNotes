> [!info] LINK & TAG
> [What the shell(piocResources)](https://primer.picoctf.com/#_what_the_shell)
> [TryHackMe | Linux Shells](https://tryhackme.com/room/linuxshells)
> [explainshell](https://explainshell.com/)
> 
> #ctf #TryHackMe #shell #cybersecurity #Linux

> [!example] Table of contents
> 

---

- <font color = "c2e3f4">see which shell you are using</font>
```shell
user@tryhackme:~$ echo $SHELL
/bin/bash
```

---

- <font color = "c2e3f4">list down the available shells in your Linux OS</font>
```shell
user@tryhackme:~$ cat /etc/shells
# /etc/shells: valid login shells
/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/bin/dash
/usr/bin/dash
/usr/bin/tmux
/usr/bin/screen
/bin/zsh
/usr/bin/zsh
```

---

<font color = "c2e3f4"> type the shell name that is present on your OS to switch between shells</font>`user@tryhackme:~$ zsh`
<font color = "efb730">use</font> `chsh -s /usr/bin/zsh` <font color = "efb730">for permanent change</font>
