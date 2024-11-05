# Basic Tools for Computer Network Analysis

When working with computer networks, having the right tools can make all the difference in troubleshooting and understanding network issues. This guide introduces three essential tools: **ping**, **traceroute** and **netstat**. Each of these tools provides valuable information to help you diagnose what’s going on in your network.

## ping

`ping` is a simple tool that checks if a device (like a website or another computer) is reachable over the network. It sends packets of data to the target and waits for a response. Think of it like shouting "Hello!" and listening for an answer.


How is it used, you may ask? It is as easy as it looks. Open up your terminal and type in:

```shell-session
ping <hostname or IP address>
```

Example listed below:

```shell-session
ping google.com
Pinging google.com [172.217.169.110] with 32 bytes of data:
Reply from 172.217.169.110: bytes=32 time=83ms TTL=116
Reply from 172.217.169.110: bytes=32 time=22ms TTL=116
Reply from 172.217.169.110: bytes=32 time=21ms TTL=116
Reply from 172.217.169.110: bytes=32 time=23ms TTL=116

Ping statistics for 172.217.169.110:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 21ms, Maximum = 83ms, Average = 37ms
```

Now, let’s get to analyzing it. What is this output we just received?

* Response Time: You'll see how long it takes for a message to go to the target and come back.
^^The round-trip times for messages ranged from 21ms to 83ms, with an average of 37ms.^^

* Packet Loss: This tells you if any messages disappeared along the way.
^^All 4 packets were successfully received with 0% loss, meaning none of the messages failed to reach the server.^^

* TTL (Time to Live): This shows how many hops the packet made before it reached its destination.
^^The TTL value of 116 indicates the number of hops the packet made to reach Google’s server; a higher number suggests a shorter route.^^

All in all, the results are wonderful.

## traceroute

`traceroute` is a tool used to find the path your data takes to reach a destination. They send packets to the target with incrementally larger TTL values, so you can see each router (or hop) your data passes through.

How is it used, you may ask? Similar procedure as ping.
Open up your terminal and type in:

```shell-session
traceroute <hostname or IP address>
```

Example listed below:

```shell-session
traceroute to google.com (142.251.179.102), 30 hops max, 60 byte packets
 1  nx-mel-vlan20.dx1b.nexcess.net (208.69.120.1)  22.087 ms  22.045 ms  22.004 ms
 2  xldx1b.core-1.mel.dtw.nexcess.net (192.240.191.186)  0.416 ms  0.389 ms  0.388 ms
 3  xlcore1.edge-1.mel.dtw.nexcess.net (192.240.191.164)  0.442 ms  0.403 ms  0.288 ms
 4  xe-10-1-0.bar1.Detroit1.Level3.net (4.15.104.5)  16.561 ms  16.542 ms  16.506 ms
 5  ae2.3601.edge3.Chicago10.net.lumen.tech (4.69.206.246)  42.470 ms  42.398 ms  42.384 ms
 6  15169-3356-chi.sp.lumen.tech (4.68.127.114)  69.339 ms  32.278 ms  32.144 ms
 7  * * *
 8  142.251.231.68 (142.251.231.68)  31.393 ms 142.251.60.206 (142.251.60.206)  31.543 ms 142.251.60.212 (142.251.60.212)  32.156 ms
 9  192.178.249.206 (192.178.249.206)  31.542 ms 192.178.249.234 (192.178.249.234)  32.406 ms 192.178.105.216 (192.178.105.216)  34.990 ms
10  142.251.234.29 (142.251.234.29)  32.219 ms 192.178.99.91 (192.178.99.91)  31.648 ms 142.251.233.223 (142.251.233.223)  34.231 ms
11  192.178.81.224 (192.178.81.224)  20.526 ms 192.178.81.234 (192.178.81.234)  20.633 ms 192.178.81.224 (192.178.81.224)  20.629 ms
12  192.178.244.223 (192.178.244.223)  22.642 ms 142.251.76.103 (142.251.76.103)  22.018 ms 192.178.244.231 (192.178.244.231)  20.826 ms
13  192.178.81.193 (192.178.81.193)  20.488 ms 172.253.66.221 (172.253.66.221)  21.061 ms 108.170.238.3 (108.170.238.3)  21.405 ms
14  192.178.75.151 (192.178.75.151)  21.719 ms 209.85.253.83 (209.85.253.83)  21.861 ms 192.178.75.151 (192.178.75.151)  23.250 ms
15  * * *
16  * * *
17  * * *
18  * * *
19  * * *
20  * * *
21  * * *
22  * * *
23  * * *
24  * pd-in-f102.1e100.net (142.251.179.102)  20.930 ms *
```

Now, let’s get to analyzing it. What is this output we just received?

* **Initial Information**
    + Destination: `traceroute to google.com` (`142.251.179.102`): This indicates the hostname and its corresponding IP address that you are trying to reach.
    + Max Hops: `30 hops max`: This specifies the maximum number of hops (routers) that the traceroute utility will attempt to reach before timing out.
    + Packet Size: `60 byte packets`: Indicates the size of the packets being sent.

* **Hops**: A list of each router your data passed through, with their IP addresses.
For example, hop 1 is `208.69.120.1`, hop 2 is `192.240.191.186`, and so on.

* **Response Times**: See how long it takes to get to each hop.
For instance, hop 1 has response times of `22.087 ms`, `22.045 ms`, and `22.004 ms`.

* **Star (*) Entries**
In the output, there are lines with asterisks (* * *). This means that the packets did not receive a response from those hops.

* **Final Destination**
The last hop in the output (hop 24) indicates that the packets finally reached the destination host `pd-in-f102.1e100.net (142.251.179.102)`. The response time of `20.930 ms` reflects the time taken to reach Google's server.

## netstat

`netstat` is a handy tool for seeing what’s happening with your computer's network connections. It shows a list of all active connections, listening ports, and routing information.

How is it used, you may ask?
Open up your terminal and type in:
`netstat [options]`

Example listed below:
```shell-session
netstat -tupln | grep 25
tcp        0      0 192.168.246.250:6888    0.0.0.0:*               LISTEN      -
tcp        0      0 192.168.250.146:6888    0.0.0.0:*               LISTEN      -
tcp        0      0 127.0.0.1:44725         0.0.0.0:*               LISTEN      -
tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      -
udp        0      0 192.168.246.250:6888    0.0.0.0:*                           -
udp        0      0 192.168.250.146:6888    0.0.0.0:*                           -
udp        0      0 192.168.250.98:123      0.0.0.0:*
```

Now, let’s get to analyzing it. What is this output we just received?

* Active Connections: A list of all the connections currently open and their status (like CONNECTED or LISTENING).

* Listening Ports: Shows which ports your computer is watching for incoming connections. TCP/UDP

* Routing Table: Displays how data is directed on your network. `0.0.0.0:*` means the service is open to accept connections from any IP address on any port, making it widely accessible.


!!! abstract "Additional Fun Facts"

    `ping` operates using `ICMP` (Internet Control Message Protocol) and does not utilize any ports.
    
    `traceroute` generally uses `ICMP` or `UDP`; when using `UDP`, it typically starts from port 33434 and utilizes a range of ephemeral ports.
    
    `netstat` provides information on active network connections and reports on both `TCP` and `UDP` ports across the entire range of 0-65535, displaying the local and remote addresses associated with those connections.
