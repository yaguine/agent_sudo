# Agent Sudo WriteUp (Tryhackme) by [yag1n3](https://github.com/yaguine)

---

## First Steps

thanks to the ICMP TTL we know it's a Linux Machine

nmap 
* p21 vsftpd 3.0.3
* p22 OpenSSH 7.6p1 Ubuntu
* p80 Apache httpd 2.4.29 ((Ubuntu))

### Web

as soon as we enter the website, someone called *Agent R* tells us that if you wanna enter the website you have tu use your *codename* as *user-agent*  
we understand that *user-agent* is the HTTP Header and *codename* a letter since he is called *Agent R*  
we force an HTTP Request with every letter, and we notice that there is a note to *Agent C* that we name *agentC_attention.txt*

*Agent R* tells *chris* that his password is too weak and that he has to talk to *Agent J*  
we use bruteforce with **Hydra** and **rockyou.txt** via FTP and we get the credentials *chris:crystal* 
we download the files *cute-alien.jpg*, *cutie.png* and *To_agentJ.txt*

### User.txt

we use **binwalk** to extract a .zip file from *cutie.png*, and we use **zip2john** & **john** to unzip it with the password *alien*  
we get a **base64** text that when translated says *Area51*  
we use `steghide extract -sf cute-alien.jpg` with the passphrase *Area51* to discover *message.txt*
there we discover the **ssh credentials** *james:hackerrules!* and we are able to get the user flag

---

## Root.txt

whith the command `sudo -l` we discover that we can use */bin/bash* as ALL except for root, that is *(ALL, !root)*  
so when using `sudo /bin/bash` we get an error message

time to google !! we discover CVE-2019-14287 that allows us that bypass this restriction  
this vulnerability exists when the sudo version is <1.8.28 and the restriction is *(ALL, !root)*  
we check the sudo version with `sudo --version` and is under 1.8.28 indeed  
so we can get a root shell with the following command :  
```bash
sudo -u#-1 /bin/bash
```

Ctf Finished !!!!!
