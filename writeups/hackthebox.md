```
root@kali:~/Desktop# nmap 10.10.10.5 -A 
 
Starting Nmap 7.40 ( https://nmap.org ) at 2017-05-17 19:53 BST 
Nmap scan report for 10.10.10.5 
Host is up (0.090s latency). 
Not shown: 998 filtered ports 
PORT   STATE SERVICE VERSION 
21/tcp open  ftp     Microsoft ftpd 
| ftp-anon: Anonymous FTP login allowed (FTP code 230) 
| 05-21-17  04:29AM                 3681 ahai.aspx 
| 03-18-17  02:06AM       <DIR>          aspnet_client 
| 05-21-17  05:09AM                 5432 aspxshell.aspx 
| 05-21-17  03:53AM                 1442 cmdasp.aspx 
| 05-21-17  03:55AM                38219 evil.asp 
| 05-21-17  03:56AM                 2779 evil.aspx 
| 05-21-17  03:50AM                58316 fresh.asp 
| 05-21-17  05:39AM                74169 h.exe 
| 05-21-17  05:29AM                74178 hack.exe 
| 03-17-17  05:37PM                  689 iisstart.htm 
| 05-21-17  04:44AM                 2816 msh.aspx 
| 05-21-17  03:54AM                59392 nc.exe 
| 05-21-17  05:30AM                74178 payload.exe 
| 05-21-17  04:54AM       <DIR>          test 
| 05-21-17  03:58AM                38585 test.asp 
| 05-21-17  04:02AM                38561 test.aspx 
| 05-21-17  03:54AM                    6 test.txt 
| 03-17-17  05:37PM               184946 welcome.png 
| 05-21-17  04:36AM                 2811 xell.aspx 
| 05-21-17  05:20AM               881554 yay.exe 
|_Only 20 shown. Use --script-args ftp-anon.maxlist=-1 to see all. 
80/tcp open  http    Microsoft IIS httpd 7.5 
| http-methods:  
|_  Potentially risky methods: TRACE 
|_http-server-header: Microsoft-IIS/7.5 
|_http-title: IIS7 
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port 
Device type: specialized|general purpose|phone 
Running (JUST GUESSING): Microsoft Windows 7|8|Phone|2008|8.1|Vista (91%) 
OS CPE: cpe:/o:microsoft:windows_7 cpe:/o:microsoft:windows_8 cpe:/o:microsoft:windows cpe:/o:microsoft:windows_server_2008:r2 cpe:/o:microsoft:windows_8.1 cpe:/o:microsoft:windows_vista::- cpe:/o:microsoft:windows_vista::sp1 
Aggressive OS guesses: Microsoft Windows Embedded Standard 7 (91%), Microsoft Windows 8.1 Update 1 (91%), Microsoft Windows Phone 7.5 or 8.0 (91%), Microsoft Windows Server 2008 R2 (90%), Microsoft Windows Server 2008 R2 or Windows 8.1 (90%), Microsoft Windows Server 2008 R2 SP1 or Windows 8 (90%), Microsoft Windows 7 (90%), Microsoft Windows 7 Professional or Windows 8 (90%), Microsoft Windows 7 SP1 or Windows Server 2008 R2 (90%), Microsoft Windows Vista SP0 or SP1, Windows Server 2008 SP1, or Windows 7 (90%) 
No exact OS matches for host (test conditions non-ideal). 
Network Distance: 2 hops 
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows 
 
TRACEROUTE (using port 21/tcp) 
HOP RTT      ADDRESS 
1   87.85 ms 10.10.12.1 
2   87.88 ms 10.10.10.5 
 
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ . 
Nmap done: 1 IP address (1 host up) scanned in 20.15 seconds 
```



