The HTML specification is maintained by the W3C.

*[HTML]: Hyper Text Markup Language
*[W3C]: World Wide Web Consortium


Port is a logical connection used by programs and services to exchange information. It determines which program or service on a computer or server is going to be used.  All ports are identified by the unique number that ranges from 0-65535. 

Whenever a device attempts to make a connection to another device on the network, a port will be paired up with an IP address. An IP address will be used to determine the physical location of the server, while the port will determine which service that server wants to use. 

We can use Netstat utility (network statistics) in order to display current network connections and port activities on our computer. 

```shell
sandar@nova:~$ netstat -n
Active Internet connections (w/o servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp        0      0 192.168.1.110:22        192.168.1.103:42470     ESTABLISHED
tcp6       0      0 192.168.1.110:80        192.168.1.107:62563     FIN_WAIT2  
tcp6       0      0 192.168.1.110:80        192.168.1.107:62550     TIME_WAIT  
```

Port numbers are assigned by IANA - Internet assigned Numbers Authority. They are broken down into three categories:

|   Port numbers    |    Port names              |
|   ------------    |    ----------------------- |
|    0-1023         |    System/Well Known Ports |
|    1024-49151     |    User/Registered ports   |
|    49152-65535    |    Dynamic/Private ports   |

First two categories are used by the server to which our device connects to, while the last one is the one that device assigns itself during the session.

| Application | TCP/UDP | Port    | Notes                    |
| ----------- | ------- | ------- | ------------------------ |
| FTP         | TCP     | 20/21   | File transfer            |
| SSH         | TCP     | 22      | Secure remote control    |
| Telnet      | TCP     | 23      | Remote control           |
| SMTP(S)     | TCP     | 25/587  | Sending emails           |
| DNS         | UDP     | 53      | Naming                   |
| DHCP        | UDP     | 67/68   | Assigns IPs              |
| HTTP(S)     | TCP     | 80/443  | Web(secure)              |
| POP3(S)     | TCP     | 110/995 | Email retrieval(secure)  |
| IMAP4       | TCP     | 143/993 | Email retrieval (secure) |
| MySQL       | TCP     | 3306    | Database server          |
