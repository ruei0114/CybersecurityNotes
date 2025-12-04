> [!info] LINK & TAG
> [TryHackMe | Authentication Bypass](https://tryhackme.com/room/authenticationbypass)
> [TryHackMe | Subdomain Enumeration](https://tryhackme.com/room/subdomainenumeration)
> 
> #tag


```sh
user@machine$ ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://MACHINE_IP


user@machine$ ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://MACHINE_IP -fs [size]

# Username Enumeration
user@tryhackme$ ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.49.187.100/customers/signup -mr "username already exists"

```