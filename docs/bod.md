# DNS and most common DNS records, their explanation

DNS stands for Domain Name System. It is a hierarchical and distributed naming system that is fundamental to the functioning of the Internet. DNS is used to translate human-readable domain names into IP addresses, which are numerical identifiers assigned to each device connected to a computer network that uses the Internet Protocol for communication.

A (Address) Record:
Associates a domain name with an IPv4 address.

AAAA (IPv6 Address) Record:
Similar to the A record but used for IPv6 addresses.

CNAME (Canonical Name) Record:
Creates an alias of one domain to another. It is often used for subdomains.

ALIAS Record:
Usage: Alias records are typically specific to certain DNS providers (e.g., AWS Route 53). They are designed to function similarly to CNAME records but can be used for the apex domain as well as subdomains.

MX (Mail Exchange) Record:
Specifies mail servers responsible for receiving emails on behalf of a domain.

TXT (Text) Record:
Holds arbitrary text data and is often used for verification or information purposes. (mostly for proving ownership over domain)

NS (Name Server) Record (where are our DNS zones):
Specifies authoritative DNS servers for the domain.

PTR (Pointer) Record:
Used for reverse DNS lookups, associating an IP address with a domain name.

SOA (Start of Authority) Record (kinda like a log):
Contains administrative information about the domain and the zone.

## Try understanding how DNS actually works, what happens when you type example.com

When you type a domain name like "example.com" into your web browser and press Enter, several steps are involved in the Domain Name System (DNS) resolution process. Here's a simplified overview of how DNS works:

Local Cache Check:
Your device (computer, smartphone, etc.) checks its local DNS cache to see if it already knows the IP address associated with the domain "example.com." This cache might contain previously resolved domain names and their corresponding IP addresses.

Operating System Resolution:
If the domain is not found in the local cache or if the cached record has expired, your operating system sends a DNS resolution request to a DNS resolver (usually provided by your Internet Service Provider - ISP).

DNS Resolver Check:
The DNS resolver, often operated by your ISP or a third-party DNS service (like Google's 8.8.8.8 or Cloudflare's 1.1.1.1), checks its cache to see if it has a record for "example.com."

Root DNS Servers:
If the resolver doesn't have the information, it contacts the root DNS servers. These servers provide information about the top-level domain (TLD) servers for the requested domain.

TLD DNS Servers:
The resolver then contacts the TLD servers (e.g., ".com" servers) to obtain information about the authoritative name servers for the domain "example.com."

Authoritative Name Servers:
The resolver contacts the authoritative name servers for "example.com" to get the specific IP address associated with the domain.

Response to DNS Resolver:
The authoritative name servers respond with the IP address for "example.com."

Cache Update:
The resolver caches this information for future use, and the resolved IP address is also cached on your local device.

Connection to the Server:
Your device now uses the obtained IP address to establish a connection to the web server hosting "example.com."

Web Server Response:
The web server responds to your request, and your browser renders the website.

This entire process is designed to efficiently translate human-readable domain names into the IP addresses required for network communication. DNS plays a crucial role in the functionality and accessibility of the internet, enabling users to access websites using user-friendly domain names rather than numerical IP addresses.

## Emails

DNS records for sending and for receiving Emails

MX records specify the mail servers responsible for receiving emails on behalf of your domain. 

Each MX record consists of a priority and a mail server hostname. The priority indicates the order in which mail servers should be used (lower values indicate higher priority).

TXT records 

### DKIM used for sender verification

DKIM provides a way to digitally sign emails, allowing the recipient to verify that the email was indeed sent by your domain.

DKIM records include a public key that corresponds to a private key used to sign outgoing emails.

DKIM is generated where MX record is pointing

### SPF identifies domains that are allowed to send the emails on behalf of your domain or some other identifier. 

SPF helps prevent email spoofing and phishing 

SPF value > v=spf1 ip4:192.0.2.0 ip4:192.0.2.1 include:examplesender.email -all

Finally, -all tells the server that addresses not listed in the SPF record are not authorized to send emails and should be rejected.

Alternative options here include ~all, which states that unlisted emails will be marked as insecure or spam but still accepted, and, less commonly, +all, which signifies that any server can send emails on behalf of your domain.

Record that we usually set for our customers v=spf1 a mx include:relay.mailchannels.net ~all

DMARC Instructs mail servers what to do if a message fails SPF/DKIM checks.


## Qmail vs dovecot, their purpose and their log locations

Qmail and Dovecot are both software components that play different roles in the email infrastructure. Here's an overview of each and information on where their logs are typically located:

Qmail:

Purpose:

Qmail is a mail transfer agent (MTA) designed to efficiently and securely handle the routing and delivery of email messages.
It is known for its security features and simplicity in design.

Logs:

Qmail logs are usually found in the /var/log/qmail directory.

Dovecot:

Purpose:

Dovecot is an email server component that provides the functionality for email retrieval (IMAP(mostly used) and POP3 retrieving emails).
It allows users to access their emails stored on the server.

Logs:

Dovecot logs are typically located in the /var/log/dovecot/ directory.

Nexcess Email Log Location
Outbound messages from on-server scripts - /var/log/send/current
Inbound SMTP - /var/log/smtp/current
Outbound SMTP - /var/log/smtp2/current
IMAP connections (on Legacy SIP) - /var/log/imap4-ssl/current
POP connections (on Legacy SIP) - /var/log/pop3-ssl/current
POP and IMAP (on Cloudhost and recent dedicated servers): /var/log/dovecot
Inbound SpamAssassin scoring - /var/log/maillog

IMAP and POP3 are email protocols used to access and manage emails on remote servers. IMAP enables more advanced email management and synchronization across numerous devices, while POP3 is better suited for configurations where emails need to be accessed only from a single device.

Ports:
Secure:
For IMAP, use port 993.
For POP3, use port 995.
For SMTP, Use port 587.

Non Secure:
IMAP 143
POP3 110
SMTP 25

## Web server status codes
100 - Information
These codes indicate that the request was received, continuing process, or the server is awaiting further instructions.
100 Continue: The server has received the initial part of the request and will continue to process it.
101 Switching Protocols: The requester has asked the server to switch protocols.
200 - Success
These codes indicate that the request was successfully received, understood, and accepted.
200 OK: The request was successful.
201 Created: The request was successful, and a new resource was created as a result.
204 No Content: The server successfully processed the request but there is no content to send.
300 - Redirect
These codes indicate that further action needs to be taken in order to complete the request.
301 Moved Permanently: The requested resource has been permanently moved to a new location.
302 Found (or Moved Temporarily): The requested resource has been temporarily moved to a different location.
304 Not Modified: The resource has not been modified since the last request.
400 - Client Error
These codes indicate that the client seems to have made an error.
400 Bad Request: The server cannot understand the request due to a client error.
401 Unauthorized: The request requires user authentication.
403 Forbidden: The server understood the request but refuses to authorize it.
404 Not Found: The requested resource could not be found on the server.
500 - Server Error
These codes indicate that the server failed to fulfill a valid request.
500 Internal Server Error: A generic error message indicating that the server has encountered an unexpected condition.
502 Bad Gateway: The server, while acting as a gateway or proxy, received an invalid response from an upstream server.
503 Service Unavailable: The server is not ready to handle the request. Common causes include the server being overloaded or undergoing maintenance.

FTP, FTPS, SFTP investigation and log locations
FTP (File Transfer Protocol), FTPS (FTP Secure), and SFTP (SSH File Transfer Protocol) are three different protocols used for transferring files between computers over a network. Each has its own characteristics and security mechanisms, including specific log locations for recording various activities and events.
1.1 FTP Purpose
FTP is a standard network protocol used for transferring files between a client and a server. It is a relatively old protocol and does not provide encryption, making it less secure for transferring sensitive data.
1.2 FTPS Purpose
FTPS is an extension of FTP that adds security through encryption. It uses SSL/TLS protocols to encrypt data in transit, making it more secure than plain FTP.
1.3 SFTP Purpose
SFTP is a secure file transfer protocol that operates over an encrypted SSH (Secure Shell) connection. It provides strong authentication and encryption, making it one of the most secure methods for transferring files.
Bonus (question they like to ask): Difference between FTPS and SFTP
Both FTPS and SFTP are protocols used for transferring files securely. However:
FTPS uses certain secure protocols to encrypt data (SSL/TLS), while SFTP works through an encrypted SSH connection.
SFTP works well with firewalls, but its binary data transmissions are not suitable for logging.
FTPS file transmissions are significantly faster than SFTP.
They have different authentication methods (encrypted key pairs) and the list of commands is different.
2. Log Location
2.1 FTP Logs
Logins - /var/log/proftpd/auth.log
Transfer - /var/log/proftpd/xfer.log
Encryption debugging - /var/log/proftpd/tls.log
2.2 SFTP Logs
Logins - /var/log/secure
3. Ports (bonus points)
FTP: 21 (between two hosts) or 20 (transfer data via the Data channel)
SFTP (SSH): 22

Networking
Private vs public IPs
Private:
A class: 10.0.0.0 -> 10.255.255.255
B class: 172.16.0.0 -> 172.31.255.255
C class: 192.168.0.0 -> 192.168.255.255
These private IP address ranges are reserved for use within local networks, such as home or office networks. They are not routable on the public Internet and are meant for internal use only.

Public:
Public IP addresses are assigned by Internet Service Providers (ISPs) and are globally routable on the public Internet. They are unique across the entire Internet. Public IP address ranges are not specifically designated by classes as in the past, but IP address blocks are allocated to organizations by regional Internet registries.
Ping
Purpose:
The ‘ping’ command is a network utility used to test the reachability of a host (usually identified by its IP address or domain name) on a network and measure the round-trip time for data to travel from the source to the destination and back. It is a fundamental tool for diagnosing network connectivity issues and assessing network performance.
Basic syntax:
$ ping [options] host_or_ip_address
Options (flags):
-c count: Specify the number of ICMP echo request packets to send (default is infinite).
-i interval: Set the interval between sending packets (default is 1 second).
-t timeout: Define the maximum time to wait for a response (default is network-dependent).
-q: Quiet mode; only display summary information.
-v: Verbose mode; display detailed information about each packet.
Reading the output:
The first thing we see is the server’s host name. This will confirm you have an active connection to the server. 
Next are the number of bytes being sent to the server, which will typically show 32. 
The next four lines indicate the response time from the server. You’ll see how many bytes of data were sent back, as well as how many milliseconds the response took to return. 
TTL means “time to live.” This information shows you the total routers the packet will travel through before stopping. 
If you see “request timed out,” it tells you that the packets couldn’t find the host, which indicates a connection problem.
The Ping statistics section shows the overall numbers for the process. The packets line shows the number of packets sent and received, and tells you if any were lost. If they were, there’s like a connection issue.
Lastly, approximate round trip times show the connection speed. The higher the time, the worse the connection.
2.1 Ping Example Command
$ ping -c 5 cloudhost-3907436.us-midwest-1.nxcli.net
PING cloudhost-3907436.us-midwest-1.nxcli.net (8.29.157.141) 56(84) bytes of data.
64 bytes from cloudhost-3907436.us-midwest-1.nxcli.net (8.29.157.141): icmp_seq=1 ttl=59 time=0.403 ms
64 bytes from cloudhost-3907436.us-midwest-1.nxcli.net (8.29.157.141): icmp_seq=2 ttl=59 time=0.335 ms
64 bytes from cloudhost-3907436.us-midwest-1.nxcli.net (8.29.157.141): icmp_seq=3 ttl=59 time=0.550 ms
64 bytes from cloudhost-3907436.us-midwest-1.nxcli.net (8.29.157.141): icmp_seq=4 ttl=59 time=0.427 ms
64 bytes from cloudhost-3907436.us-midwest-1.nxcli.net (8.29.157.141): icmp_seq=5 ttl=59 time=0.299 ms
--- cloudhost-3907436.us-midwest-1.nxcli.net ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 5010ms
rtt min/avg/max/mdev = 0.299/0.402/0.550/0.090 ms
Traceroute
The command use:
The `traceroute` command is a network diagnostic tool used to trace and display the route that data packets take from a source computer or device to a destination host or IP address on a network. It provides a detailed list of all the intermediary routers or network devices (hops) that the data packets pass through on their journey to the destination. This information is helpful for diagnosing network routing issues, identifying latency bottlenecks, and understanding the network path between two points.
An Internet Protocol (IP) tracer is helpful for figuring out the routing hops data has to go through, as well as response delays as it travels across nodes, which are what send the data toward its destination. Traceroute also enables you to locate where the data was unable to be sent along, known as points of failure. You can also perform a visual traceroute to get a visual representation of each hop.
Reading the information from `traceroute` command:
Hops and Round Trip Times (RTT)
The traceroute report lists data pertaining to every router the packets pass through as they head to their destination. The hops get numbered on the left side of the report window. Each line in the report has the domain name—if that was included—as well as the IP address belonging to the router.
There are also three measurements of time, displayed in milliseconds. These tell you the length of time to send the ICMP packets from your computer to that router and back.
Typical Hop Sequence
A “hop” refers to the move data makes as it goes from one router to the next. The first hop within the report provides information about the first router, which would be on your local-area network (LAN). The hops that come after provide data about routers controlled by your internet service provider (ISP).
When the ICMP packets get beyond the ISP’s domain, they go to the general internet, and you will likely see that the hop times increase, typically due to geographical distance.
If there is an asterisk
Sometimes, a traceroute has a hard time accessing a device or is unreachable. In these situations, it may show a message saying, “Request timed out,” along with an asterisk. This indicates that the router it reached was configured to deprioritize or automatically reject ICMP packets, which is done because ICMP is not categorized as essential traffic by many routers.
If you get several timeouts in a row, it can be because:
The packets arrived at a router with a firewall that prevents traceroute online requests.
The packets arrived at the subsequent router, but they were not able to return to the computer that sent them.
The router has a connection problem.
`traceroute` vs `ping`:
While ping is excellent for basic reachability testing and measuring round-trip times to a specific host or IP address, traceroute provides a more comprehensive view of the network's behavior, making it particularly valuable for diagnosing routing issues, network performance problems, and complex network topologies.
Traceroute is specifically designed to help identify routing issues within a network. If you suspect that there are problems with the routing of packets to a destination, such as traffic being routed through an unexpected path or delays at specific router hops, traceroute is the tool of choice. It provides a detailed view of the network path, including all the intermediate routers.
Comparison:
Purpose: ping is primarily used to check reachability and measure RTT, while traceroute is used to trace the network path and identify routing issues.
Output: ping provides simple reachability and latency information, while traceroute provides detailed information about each hop along the route.
Use Cases: Use ping for basic reachability and latency testing. Use traceroute when you need to diagnose complex network problems, understand the network path, or identify routing issues.
3.1 Traceroute Command Example
$ traceroute cloudhost-3907436.us-midwest-1.nxcli.net
traceroute to cloudhost-3907436.us-midwest-1.nxcli.net (8.29.157.141), 30 hops max, 60 byte packets
 1  nx-mel-vlan20.dx1b.nexcess.net (208.69.120.1)  42.723 ms  42.580 ms  42.433 ms
 2  xldx1b.core-1.mel.dtw.nexcess.net (192.240.191.186)  0.421 ms  0.284 ms  0.196 ms
 3  xlcore1.dx.1d.mel-01.dtw.nexcess.net (192.240.191.191)  15.007 ms  26.214 ms  14.747 ms
 4  * * *
 5  * * *
 6  cloudhost-3907436.us-midwest-1.nxcli.net (8.29.157.141)  0.372 ms  0.373 ms  0.321 ms

Firewall - APF vs iptables
APF (Advanced Policy Firewall) and iptables are related but distinct components in the context of firewall management on Linux systems.
iptables:
iptables is a Command-Line Utility:
iptables is a command-line utility for configuring the packet filter rules in the Linux kernel's netfilter framework.
It is a part of the Linux kernel and is the standard tool for managing packet filtering rules.
APF (Advanced Policy Firewall) Nexcess uses this:
APF is wrapper/simplifier of Iptables, not a stand-alone firewall
Firewall Management Script:
APF (Advanced Policy Firewall) is a script that sits on top of iptables and provides a higher-level interface for managing firewall rules.
It simplifies the process of configuring iptables rules by offering a more user-friendly interface.
Rule Management:
APF allows you to manage rules using configuration files, and it includes pre-configured rule sets for common services and applications.
Event-Based System:
APF includes an event-based system that can trigger actions based on various events, such as excessive connections or port scans. This can enhance security by automatically responding to specific network activities.
Comparison:
Abstraction Level:
iptables provides a lower-level, direct interface to the Linux kernel's netfilter framework.
APF is a higher-level tool that simplifies the creation and management of iptables rules.
Use Cases:
iptables is suitable for advanced users who require fine-grained control over packet filtering.
APF is often used by administrators who prioritize ease of use and may not need the full range of capabilities offered by iptables.

Linux ownership & permissions
Ownership
ownership refers to the user and group associated with a file or directory and what permissions they have. Every file and directory on a Linux system has an owner and a group. These ownership settings determine who can access and modify the file or directory

Changing user ownership:
Chown newowner:groupname filename

Changing group ownership:
Chown :newgroup filename
Permissions
Digit combination permission- adding permission via values to desired category
User (Owner): The user who owns the file. This user has specific permissions on the file.
Group: A collection of users. The group ownership allows multiple users to share access to a file. Users who are members of the group associated with a file can have specific permissions.
Others: All other users on the system who are not the owner or members of the group. This category includes the rest of the world.
Read4 Write2 eXecute1

What is 523 permission?
r _ x . _ w _ . _ w x

let's break down a 3-digit octal permission like 523:
The first digit (5) represents the user's (owner's) permissions.
5 in octal is equivalent to 101 in binary, which means:
The user has read (4) and execute (1) permissions, but not write (2) permissions.
The second digit (2) represents the group's permissions.
2 in octal is equivalent to 010 in binary, which means:
The group has write (2) permissions but not read (4) or execute (1) permissions.
The third digit (3) represents others' permissions.
3 in octal is equivalent to 011 in binary, which means:
Others have read (4) and write (2) permissions but not execute (1) permissions.


So, in the context of file or directory permissions, "523" would represent the following:
The user (owner) has read and execute permissions (5).
The group has write permissions (2).
Others have read and write permissions (3).


PERMISSIONS:

Linux file permissions are represented using a 3-digit octal (base-8) number, where each digit corresponds to a specific permission category: user (owner), group, and others. These numbers are used to specify who can perform certain actions on a file or directory. Each digit is a combination of three bits (0 to 7) representing read (4), write (2), and execute (1) permissions.

Here's how the permissions are represented:
Read (r) permission is represented by 4.
Write (w) permission is represented by 2.
Execute (x) permission is represented by 1.

You can calculate the permission value for each category (user, group, others) by adding up the values for the respective permissions. For example:
If a user has read (r) and write (w) permissions but no execute (x) permission, their permission value is 4 (read) + 2 (write) + 0 (no execute) = 6.
If a group has only read (r) permission, its permission value is 4 (read) + 0 (no write) + 0 (no execute) = 4.
If others have only execute (x) permission, their permission value is 0 (no read) + 0 (no write) + 1 (execute) = 1.

Keep in mind that there are also special permissions like the setuid (s), setgid (s), and sticky bit (t), which can modify the standard permission behavior, but they are not represented in the the 3-digit octal notation. These special permissions are indicated by additional characters when viewing permissions with the ls command (e.g., -rwsr-xr-x).

How do we change permissions? Command examples / different ways:


Adding Permissions:


To give the owner (user) of a file read and write permission:

chmod u+rw filename

To give the group read and execute permission:

chmod g+rx filename

To give others execute permission:

chmod o+x filename

Removing Permissions:

To remove write permission for the owner:

chmod u-w filename

To remove all permissions for the group and others:

chmod go-rwx filename

Setting Permissions:


To set read and execute permission for the owner and group, and read-only for others:

chmod u=rx,g=rx,o=r filename




Combining Permissions:


To give read and write permission to the owner and group, and read-only for others:

chmod ug+rw,o+r filename


Numeric Notation:

In numeric notation, each permission type is represented by a digit (0-7), and you calculate the permission value by adding the values for read (4), write (2), and execute (1).

To give read and write permission to the owner and read-only permission to the group and others (equivalent to chmod u+rw,g+r,o+r):

chmod 644 filename

To give full permission to the owner, read and execute permission to the group, and no permission to others (equivalent to chmod u+rwx,g+rx,o-rwx):

chmod 750 filename

To make a file executable by everyone (equivalent to chmod a+x):

chmod 111 filename

To make a directory accessible only by the owner (equivalent to chmod u=rwx,g=,o=):

chmod 700 directory


OWNERSHIP:

In Linux, file and directory ownership is a critical aspect of managing file system security. Ownership determines who has control over a file or directory and what permissions they have. There are two primary components of ownership: the user owner and the group owner.

User Owner:
The user owner is the individual user who has control over the file or directory. This user can perform various actions on the file, depending on the permissions set.

Group Owner:
The group owner is a group of users who also have certain permissions on the file or directory. These permissions are separate from those of the user owner and allow multiple users to collaborate on files while maintaining certain security restrictions.

How do we change ownership? Command examples / different ways:


Here are some common commands and concepts related to Linux ownership:

ls -l: Use the ls command with the -l option to list files and directories in long format, which includes ownership information. The output will display the user owner, group owner, and permissions.
Example:

$ ls -l
-rw-r--r-- 1 user1 group1 1024 Oct 10 12:34 file.txt



chown: The chown command is used to change the owner of a file or directory. You need superuser (root) privileges to change ownership to a different user.
Example:

sudo chown newowner:groupname file.txt

chgrp: The chgrp command is used to change the group ownership of a file or directory. Like chown, you typically need superuser privileges to change the group.
Example:

sudo chgrp newgroup file.txt

groups: The groups command displays the groups a user belongs to.
Example:

groups username

useradd: The useradd command is used to create a new user on the system.
Example:

sudo useradd newuser

groupadd: The groupadd command creates a new group on the system.
Example:

sudo groupadd newgroup

It's essential to manage ownership and permissions carefully, especially for sensitive system files and directories, to maintain system security and proper access control. Incorrectly configured ownership and permissions can lead to security vulnerabilities or operational issues

Linux basic commands(checking current load, checking load from 2 hours etc...)
cd commands:
cd - go into the directory

cd .. - one directory up

cd ../otherdirectory - change a dir without exiting the previous one

cd /home/lora_arthur/Home/lora_arthur/otherdirectory - whole path to go to

cd ~/otherdirectory - the last command but shorter (~ stands for path to USER’S home directory)

cd + tab - shows the list of possible directories I can go into


pwd - current path

mkdir - make a directory

touch - create a file (multiple ones by just adding one more with a space)

mkdir test1 && touch test1/newfile - adds a directory + creates a file in the directory

rm - removes a file

rmdir - removes a directory ( will not work if the directory is not empty)


ls commands:

ls - list the files

- lah
- l : use listing format
- a: do not ignore entries starting with .
- h : human readable sizes

-lartch
- r : reverse order
- t: newest first
- c : with -lt : sort by, and show, ctime
with -l : show ctime and sort by name
Otherwise: sort by ctime , newest first


mv test.txt training1/ - moving a file to a dir

ln - make links between files

ln -s training1/test test-shortcut - making a shortcut


vim test.txt - edit the file with insert key

:wq! - exit and save vim

cat text.txt - check the content of file

cat newfile.txt | sort | uniq -c - will show if the file has a duplicate content and how many times has it been repeated

echo 'newtext' > newfile.txt - adds text to a file (running the same command with different text will replace the original one)

echo 'newtext2' >> newfile.txt - adds a new line to the text

ls -lah nonexistent 2> error.txt - will create a file where the error message of this command will be stored

grep "file" *.txt - will search for a keyword through every file that has txt format

grep -r 'such' ./* - will search for a keyword in any type of file 

Stat- gives information about the file and filesystem. Stat command gives information such as the size of the file, access permissions and the user ID and group ID, birth time access time of the file.

Dig- command is a flexible tool for interrogating DNS name servers.

History- history command keeps a list of all the other commands that have been run from that terminal session

Clear- We can clear our terminal by using this command

Diff- allows you to compare two files line by line. It can also compare the contents of directories.

Links- useable command to create the links for the files and the -s option to specify that this will be a symbolic link. If you omit the -s option, then a hard link will be created instead.

Sudo - Runs a command as a superuser

Df -h - used to display the disk space used in the file system. The df stands for "disk filesystem." It defines the number of blocks used, the number of blocks available, and the directory where the file system is mounted. (inode)

Checking system load:

uptime: The uptime command provides information about the current system load and how long the system has been running. The output will include load averages for the last 1, 5, and 15 minutes.


top: The top command displays real-time information about system processes, including the current system load. Press q to quit top when you're done viewing the information.
You can also use the htop command, which is an interactive process viewer that provides more features and a more user-friendly interface.


w: The w command displays information about currently logged-in users and their activities, including the system load.


sar: The sar command (System Activity Reporter) can be used to collect, report, and save system activity information. You can check historical load data by specifying a time frame, like 2 hours ago.
To check the average system load for the last 2 hours, you can use the following command:
sar -q -s HH:MM:SS -e HH:MM:SS


Replace HH:MM:SS with the start and end times you want to analyze.


atop: The atop command provides a more detailed view of system performance, including historical data. It's not always installed by default, so you may need to install it first.
You can navigate through different views using the keys listed at the bottom of the atop interface. To exit, press q.


dstat: The dstat command provides real-time system resource statistics, including CPU, memory, and I/O usage, which can help you understand system load.
To stop dstat, press Ctrl+C.




SSL Certificates
Their Purpose
In summary, the primary purpose of SSL/TLS is to provide a secure and encrypted communication channel over the Internet, ensuring the confidentiality, integrity, and authenticity of the transmitted data. This is crucial for securing online transactions, protecting sensitive information, and establishing trust between users and websites
Encryption:
SSL/TLS encrypts the data exchanged between the client and the server. This encryption ensures that even if the transmitted data is intercepted by unauthorized entities, it cannot be easily read or understood.
Data Integrity:
SSL/TLS provides mechanisms to verify the integrity of the transmitted data. This ensures that the data has not been tampered with during transit.
Authentication:
SSL/TLS facilitates mutual authentication between the client and the server. This ensures that both parties can verify each other's identities. In the case of websites, SSL/TLS allows users to verify that they are connected to the legitimate website and not a malicious impostor.
Compliance with Regulations:
Many data protection regulations and standards, such as GDPR, require the use of encryption to protect sensitive information. SSL/TLS helps organizations comply with these regulations.

Signed vs Self-Signed
The difference between signed SSL/TLS certificates and self-signed SSL/TLS certificates:
Signed SSL/TLS Certificates:
Certificate Authority (CA): These certificates are issued by trusted Certificate Authorities (CAs) that are widely recognized and accepted by web browsers and other client applications.
Trust: Web browsers and client applications inherently trust SSL/TLS certificates signed by well-known CAs. When a user connects to a website with a signed certificate, the browser doesn't display security warnings (assuming no issues with the certificate).
Cost: Obtaining a signed certificate from a trusted CA typically involves a cost, as CAs need to validate the domain ownership before issuing the certificate.
Self-Signed SSL/TLS Certificates:
Certificate Issued by the Entity: Self-signed certificates are generated and signed by the entity that owns the server (e.g., the website administrator) rather than a third-party CA.
Trust: Self-signed certificates are not automatically trusted by web browsers and client applications. When a user connects to a website using a self-signed certificate, the browser typically displays a warning about the potential security risk.
Cost: Self-signed certificates are free to create and use, making them a cost-effective option.
Use Cases:
Signed Certificates: These are suitable for public-facing websites and applications where user trust and security are critical. They are essential for e-commerce websites, online banking, and any site or service handling sensitive user data.
Self-Signed Certificates: Self-signed certificates are commonly used for internal services, testing environments, or in situations where the audience connecting to the server is controlled and can be informed about the use of self-signed certificates. They are not recommended for public-facing websites or applications, as they can lead to a poor user experience due to security warnings.
SSL/TLS certificates are essential for securing data transmission on the internet. While signed certificates are trusted and widely used for public-facing websites, self-signed certificates have their place in controlled environments or for internal use. However, it's crucial to be aware of the limitations and potential security warnings associated with self-signed certificates.
In summary, the key distinction is that signed certificates are issued by a trusted third-party CA, while self-signed certificates are generated and signed by the entity itself. The choice between them depends on the level of trust required for your specific use case and whether the certificate is intended for public or internal use.
