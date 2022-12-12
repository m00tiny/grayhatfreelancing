+++
title = 'Olympus - TryHackMe Walkthrough'
date = '2022-11-25'
description = 'A step-by-step walkthrough on how to complete the Olympus capture the flag room on TryHackMe.com'
categories = [
    'Walkthroughs',
    'TryHackMe',
    ]
tags = [
    'ctf',
    'sql injection',
    'attack surface mapping',
    'password cracking',
    ]
+++

## Task 1 | Connection
[https://tryhackme.com/room/olympusroom](Olympus) is a medium difficulty room on [https://tryhackme.com/](TryHackMe training platform) by Gavroche.  
It's primary focus is sql injection, in my opinion, with a slant on enumeration and systems comprehension.  
We're going to get started by adding *olympus.thm* to our */etc/hosts* file to assist with enumeration.. this is recommended in the description and I've adopted it as a solid sign that we'll need to do subdomain enumeration. But, we don't "know that", yet.  

## Task 2 | Flag Submission
Let's dive into the enumeration. We'll kick off with quick nmap and some file and directory brute forcing.

```
Nmap scan report for olympus.thm (10.10.94.239)
Host is up, received reset ttl 61 (0.097s latency).
Scanned at 2022-11-25 12:15:42 EST for 18s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 61 OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 0a7814042cdf25fb4ea21434800b8539 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDPNeXYxrC1xv8fhFNve9CXjWSQcCXnWZThU1putOar7KBcQmoCQUYOqvmS+CDauJMPqVE3rqS0+CpTJnZn2ZWXDaCzFLZ84hjBXq8BqoWOFB0Vv0PjRKfBKC54tpA67NgLfp1TmmlS6jp4i75lxkZ6pSTOPxGUrvYvJ0iN2cAHJkgA9SZDrvT11HEp5oLmS2lXtFSoK/Q9pKNIl7y+07gZLRUeIKIn1bFRc4qrXn+rpDQR2fP9OEYiHhdJmTJJL+KjDAqZmIj0SYtuzD4Ok2Nkg5DHlCzOizYNQAkkj6Ift7dkD6LPebRp9MkAoThDzLya7YaFIP66mCbxJRPcNfQ3bJkUy0qTsu9MiiNtyvd9m8vacyA803eKIERIRj5JK1BTUKNAzsZeAuao9Kq/etHskvTy0TKspeBLwdmmRFkqerDIrznWcRyG/UnsEGUARe2h6CwuCJH8QCPMSc93zMrsZNs1z3FIoMzWTf23MWDOeNA8dkYewrDywEuOvb3Vrvk=
|   256 8d5601ca55dee17c6404cee6f1a5c7ac (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBHFtzLQXLhGiDzPN7Al84lSfH3jFwGniFL5WQSaIjC+VGMU8mbvbGVuOij+xUAbYarbBuoUagljDmBR5WIRSDeo=
|   256 1fc1be3f9ce78e243334a644af684c3c (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIKhvoRyjZN/taS1uwwTaQ4uZrGhVUje0YWW4jg4rfdXw
80/tcp open  http    syn-ack ttl 61 Apache httpd 2.4.41 ((Ubuntu))
|_http-favicon: Unknown favicon MD5: ED726E098473BFA7523153F761D2232F
|_http-title: Olympus
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.41 (Ubuntu)
```

We can see on the homepage, which has some nice CSS that the "it-department" flat out tells us that the old version of the website is still available somewhere, so we kick of the enumeration scanning.
We find some non-traditional versions of some folders we expect and some 403 folders, I used dirsearch just because I typically use ffuf and I felt like using something different.  It actually enumerated all of the /phpmyadmin/ despite that they all returned 403.. some other folders we bumped into were /javascript/, /static/ and none of these were useful until I tried some other smaller wordlists **big.txt from dirb exposed the directory /~webmaster/** which landed me on a CMS. First step complete. Time to start finding some vulnerabilities and exposing some flags.

### Flag 1
Landing on the CMS, I start with a manual pentesting. Just some basic toying with user-supplied input immediately exposes and SQL injection vulnerability on the search function. As well, the typical `' OR '1'='1'; - --'` seems to bypass login to some extent that would likely be worth investigating. But, lets start with the SQLi.. if we can get access the database, we'll probably find some flags, as well if we can get a shell, we can find some flags.  So, we'll take go ahead and launch sqlmap (note too that the comments here expose the CMS as being **Simple Content Management System**, **October 2016** .. So, you could definitely search for vulnerabilities affecting that platform.  But, we're not going to do that, as discussed, let's go ahead and launch `sqlmap`  
 `sqlmap -u http://ip/~webmaster/ -a --crawl 3 --threads 10 --level 5 --risk 3 --forms`  

This should be sufficient, we're going to go and just grab a PHP session cookie. As soon as it asks if you want to fuzz search.php say yes and it should quickly find several SQL injection vulnerabilities almost right away. Mine found a timing attack first, whisn't my preference but I let it run wild anyway. Sure enough, it dumped the whole database pretty fast. The first thing to note here is the flag table.  

**flag{Sm4rt!_k33P_d1gGIng}**

### Flag 2
Next I went trying to crack the dumped passwords and I was able to break the user `prometheus` using the john and rockyou.txt wordlist `prometheus:summertime`.  
Signing in as this user and hunting for more vulnerabilities, I did continue to find more but nothing else led me really any further. Feel free to explore that on your own, your results may differ.  
I did notice one thing, however.. other than prometheus, the users's the e-mail address all showed the domain chat.olympus.thm - so, I added that to my `/etc/hosts` file and tried browsing to it, whala we've founda new vhost.  I try the user `prometheus` credentials here and they still work.  
Even better, it's a chat and there's chat history. We can read that the other user uploaded a file and it's name changed on him, so he had difficulty finding it. Well, it's point us directly at the next vulnerability AND I dump everything when I run sqlmap, so I happened to already realize the chat data is in there, I rememebered seeing a filename resembling a hash listed with a user. I went an double checked, sure enough. So, I went and grabbed Pentest Monkey's reverse PHP shell, and uploaded it.. I ran sqlmap again and dumped the Chat table again, sure enough there was my file with a new filename.. So, I opened up netcat to listen for the incoming connection and browsed to the file and boom, reverse shell.
Unfortunately we still have a short ways to go to get the next flag.
Searching for a flag as the `www-data` user was unsucessful for me. So, I set my sights on privilege escalation to either `zeus` or `root`, obviously a strong preference for root. One of the first things that stands out is searching for a setmode executables returns `/usr/bin/cputils` and it's exactly what it looks like. It runs as user `zeus` and it lets you copy a file seeing `zeus` has a `.ssh` folder in it's home directory we correctly assume there's an SSH key sitting in there with a default name and copy `/home/zeus/.ssh/id_rsa` out and either use `scp` to download it or just co[y and paste it and set the right permissions on the new file `chmod 600 id_rsa`. From there, I try to ssh in as zeus to find the key is password protected so we run `ssh2john` and crack the key `snowflake`.. **then** we sshe in as `zeus` `ssh -i id_rsa zeus@olympus.thm`. Finally another flag.


**flag{Y0u_G0t_TH3_l1ghtN1nG_P0w3R}**

### Flag 3
There's no more users to escalate too, so we assume there's a hidden flag. I search environment variables and don't find anything and start assume I'm going to have to break out of a container or VM as root. So, I go for root. But I do find all these random files in /var/www/html that I didn't know were sitting there (recalling the reverse shell dropped us in / and not /var/www/html) it's not available from the http service. So I check out these files and I noticed these two lines:
```
$suid_bd = "/lib/defended/libc.so.99";
$shell = "uname -a; w; $suid_bd";
```
To me this looks exactly like a malicious library that's going to pop a root shell, so I just run it manually `uname -a; w; /lib/defended/libc.so.99` and sure it enough I get the coveted `#` ..  `cd /root` and there's your flag:

**flag{D4mN!_Y0u_G0T_m3_:)_}**

### Flag 4
First I jump back to / and kick off a very slow and long command `grep -r flag` and I go browsing looking for methods to break out of LXD. I don't find anything too new, mostly old ways to exploit lxc to gain root and not really anything new about breaking out of a container. So, I go ahead and run `find / -name flag` and within seconds it finds the final flag hiding in `/etc` (preceding .):

**flag{Y0u_G0t_m3_g00d!}**
