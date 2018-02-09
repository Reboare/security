# Escaping Restricted Shells

Some sysadmins don't want their users to have access to all commands, so they use a restricted shell. If the hacker get access to a user with a restriced shell, he needs to be able to break out of it.  Many linux distros include rshell, which is a restriced shell.

To access the restricted shell you can do this:

```
sh -r 
rsh

rbash
bash -r
bash --restricted

rksh
ksh -r
```

## Escaping

As a first step it's worth checking the PATH variable to see what you can and can't do.

```
echo $PATH
ls $PATH
```

There may be some programs within the path that let you spawn a regular shell.

If the box was gained via SSH access, it's worth seeing if the noprofile command works too:

```
ssh user@example.com 'bash --noprofile'
```

[http://securebean.blogspot.cl/2014/05/escaping-restricted-shell\_3.html?view=sidebar](http://securebean.blogspot.cl/2014/05/escaping-restricted-shell_3.html?view=sidebar)  
[http://pen-testing.sans.org/blog/pen-testing/2012/06/06/escaping-restricted-linux-shells](http://pen-testing.sans.org/blog/pen-testing/2012/06/06/escaping-restricted-linux-shells)

