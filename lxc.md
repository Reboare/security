# lxc

## Privileged Containers

All bets are off with privileged containers, to test out creating one we want to do the following 

```
sudo su
lxc launch ubuntu:16.04
lxc exec <name> bash
useradd tecmint
passwd tecmint
mkdir /home/tecmint
chown tecmint:tecmint /home/tecmint
usermod -a -G lxd tecmint
su tecmint
```



References:

https://linuxcontainers.org/lxc/getting-started/

  




