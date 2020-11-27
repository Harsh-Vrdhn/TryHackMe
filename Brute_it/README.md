<h1> Brute It </h1>

Room: Brute It

Link: https://tryhackme.com/room/bruteit

Machine IP: 10.10.204.208

### Reconnaissance

Now starting with an nmap scan we see that there are 2 open ports, port 80 and 22.
   
  <img src="screenshots/nmap.png">

The nmap gives us answer for our first 4 questions.

And we find a hidden directory named admin from the directory scan.

### Getting a shell
Now looking into the admin we see a login panel, checking the source code gives us the username admin.
So ,now we can bruteforce the login panel using hydra.
   
   <img src="screenshots/hydra.png">

Now after logging into the admin panel of the web page we see our web flag "THM{brut3_f0rce_is_e4sy}" and also our id rsa key for user john.

Using the wget command we download the id_rsa file and now we have to crack it using john but first we have to convert it into a hash and for that we use the command: python /usr/share/john/ssh2john.py id_rsa > id_rsa.hash
 
 And now using john for cracking this hash.
    
   <img src="screenshots/john.png">

 The passphrase is rockinroll

Logging into ssh via the above found credentials but first of all we have to change our id_rsa files permissions for that we type 'chmod 600 id_rsa'.
    
<img src="screemshots/sshlogin.png"

Now, cat user.txt and we have our user flag: THM{a_password_is_not_a_barrier}

### Privilege Escalation

First running the command sudo -l we see that we can run cat command with sudo without entering the password so we run the following command to grab the root flag:
 sudo cat /root/root.txt
 and we get the flag "THM{pr1v1l3g3_3sc4l4t10n}"

Since, we can run cat as sudo, we have the privileges to read the contents of shadow file using this command.
Now we run, "sudo cat /etc/shadow" and copy the root users hash into a txt file in our machine and use john to crack it and for that we use the following command:
  sudo john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt > crack.txt

<img src="screenshots/root.png">

And we have completed the room brute it. 
