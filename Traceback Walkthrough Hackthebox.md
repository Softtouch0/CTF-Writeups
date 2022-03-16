# Traceback Walkthrough | Hackthebox

My walkthrough for Traceback machine on Hackthebox, it is a Linux machine with easy difficulty level.

![](https://i.imgur.com/zdq5wYp.png)


First of all, like every other machine, I used Nmap to scan for Open ports
**ssh port 22** was open and **http port 80** was open too. 
**Http port was runing on Apache server**.

![](https://i.imgur.com/1wfximM.png)


So i checked the web on port 80 and found this:

![](https://i.imgur.com/9w7q3H3.png)


The website has been hacked but a backdoor was open. So I have to figure out how to enter through the backdoor, So the first thing I did was to check the source code.

![](https://i.imgur.com/VBPkat5.png)


There is nothing new on the source code but toward the end of the line, I was a comment session, which caught my attention and I decided to check it up on google.

![](https://i.imgur.com/6D6zNVM.png)


It showed A Github Repository by TheBinitGhimire, It has the same name as what is on the comment on the web page, I clicked on the link and I found many PHP WEB SHELLS.

![](https://i.imgur.com/wGumxCB.png)


I knew it will take a long time checking the WEB SHELL one after the other, So I automated the process, copied the whole web shell in a text file and used DIRB to bruteforce the files.


![](https://i.imgur.com/9zaQWzz.png)



Dirb returned two shells + http://10.10.10.181/alfav3.0.1.php (CODE:200|SIZE:2) and + http://10.10.10.181/smevk.php (CODE:200|SIZE:1261), the first shell alfav3.0.1.php returned an empty on the server, so I tried 10.10.10.181/smevk.php on the server and it returned a login page.

![](https://i.imgur.com/4RY1iIJ.png)


It Asked for Username and Password. So I checked the source code of the shell on Github Repository, and the default password and username is Admin. I tried the credential and I was logged in.

![](https://i.imgur.com/L1lNxyY.png)


I saw the current user is webadmin, So I navigate to the Home Directory folder where I see the user webadmin


![](https://i.imgur.com/S0SwLb5.png)


![](https://i.imgur.com/qpp6vow.png)


I saw the .ssh directory is writeable and executable.

![](https://i.imgur.com/ilc4VUa.png)



I can be able to login as a webadmin by placing my SSH public key on the Authorized_keys file under the .ssh Directory. Therefore I have to generate key using the ssh-keygen command.

![](https://i.imgur.com/wHyIDeo.png)


So I rename id_rsa.pub as authorized_keys

![](https://i.imgur.com/sX4SxsP.png)


After that I uploaded the authorized_keys on the .ssh Directory on the server, then I tried to login with ssh with my own generated key.


![](https://i.imgur.com/sCcs5Ri.png)


I used the ls command to check what is on my current directory, then I found a file name note.txt on the current Directory.

![](https://i.imgur.com/zK0N1cw.png)


In the file, I saw Lua, Lua is a programming language, So i used sudo -l to see the command webadmin can execute without password. 

![](https://i.imgur.com/sCB7bob.png)


I found that the webadmin can run /home/sysadmin/luvit. So I check online to see what I can find.

![](https://i.imgur.com/BxFPyg5.png)


I found a command in GTOBins, os.execute ("/bin/bash -i"), I run the command and my user was changed to sysadmin.

![](https://i.imgur.com/5kDaAV3.png)


Then I navigate my Directory to Home and saw two Directories.. sysadmin and webadmin. I navigate to sysadmin, There I found my user.txt.

![](https://i.imgur.com/cZ2h21u.png)


There I found my user.txt.


The user.txt contains the user flag... Now I got the User flag.


![](https://i.imgur.com/Hv13EFl.png)


User Flag: 5f396291dc7376ceb74d0c98c75b0858

Now, I have to obtain the root access to get the root flag. To obtain the root flag I have to escalate my privilege and get root shell before I can get my root flag.
I tried many means to get in, but I couldn't not find a way, so I decided to check the processes maybe I can find my way through. So I used linpeas.sh script I found on github, to check the active processes and dormant processes.


![](https://i.imgur.com/Hs0mmXS.png)


Downloaded it to the tmp folder, give it executable permission. Execute the script.


![](https://i.imgur.com/bXFwbuI.png)


![](https://i.imgur.com/ilXbHp4.png)


The I saw, a process on /etc/update_motd.d running root permission and running repeatedly every 30 seconds. So I decided to exploit that process, I added some script that will spawn the root shell for me.


![](https://i.imgur.com/zXwvYSH.png)

N.B mot.d means means message of the month, you can read more about it and see how it works.


So I generated malicious script using msfvenom to exploit the vulnerability that I have seen in the server.


![](https://i.imgur.com/CWRQ9BM.png)


I edited the 00-header file, typed the above script on the last line of the file and save it. Then tried to  login via ssh with sysadmin. 


![](https://i.imgur.com/rs7vpXE.png)


Voila!!! I got my root shell and the root flag as well.


![](https://i.imgur.com/lbWPzKo.png)



Root Flag: e0310b154532322454062d9bbafc9ef5













