## IP address

An IP address is a unique numerical value assigned to a device or domain that connects to the Internet. Each IP address is a series of characters, such as '192.168.1.1'. Via DNS resolvers, which translate human-readable domain names into IP addresses, users are able to access websites without memorizing this complex series of characters.

Two main functions:

* Host or network interface identification 
* Location addressing

!!! info
    IPv4 / IPv6 - IPv4 is a 32-bit IP protocol address construction, IPv6 is 128-bit one.

## Network prefixes

IP addresses can be taken from the IPv4 or the IPv6 pool and are divided into two parts, a network section and a host section. The network section identifies the particular network and the host section identifies the particular node (for example, a certain computer) on the Local Area Network (LAN).

## IP address types

There are four different types of IP addresses: **`public`**, **`private`**, **`static`**, and **`dynamic`**. While the public and private are indicative of the location of the network—private being used inside 
a network while the public is used outside of a network—static and dynamic indicate permanency.

* **`Public`**: public IP address is an address where one primary address is associated with your whole network. In this type of IP address, each of the connected devices has the same IP address

* **`Private`**: private IP address is a unique IP number assigned to every device that connects to your home internet network, which includes devices like computers, tablets, smartphones, which is used in your household

* **`Static`**: static IP address is an IP address that cannot be changed. In contrast, a dynamic IP address will be assigned by a Dynamic Host Configuration Protocol (DHCP) server, which is subject to change. Static IP address never changes, but it can be altered as part of routine network administration.
* **`Dynamic`**: dynamic IP addresses always keep changing. It is temporary and is allocated to a device every time it connects to the web.

## IP Classes - Classful network

A classful network is an obsolete network addressing architecture used in the Internet from 1981 until the introduction of Classless Inter-Domain Routing in 1993. 

The method divides the IP address space for Internet Protocol version 4 into five address classes based on the leading four address bits.
IP addresses from the first three classes (A, B and C) can be used for host addresses.

* **Class A**: 10.0. 0.0 — 10.255. 255.255.
* **Class B**: 172.16. 0.0 — 172.31. 255.255.
* **Class C**: 192.168. 0.0 — 192.168. 255.255.

## CIDR

Classless Inter-domain Routing (CIDR) was introduced in order to improve both address space utilization and routing scalability in the Internet. It was needed because of the rapid growth of the Internet and growth of the IP routing tables held in the Internet routers.

CIDR moves away from the traditional IP classes (Class A, Class B, Class C, and so on). In CIDR , an IP network is represented by a prefix, which is an IP address and some indication of the length of the mask. Length means the number of left-most contiguous mask bits that are set to one.

So, network 172.16.0.0 255.255.0.0 can be represented as 172.16.0.0/16. CIDR also depicts a more hierarchical Internet architecture, where each domain takes its IP addresses from a higher level.

This allows for the summarization of the domains to be done at the higher level. For example, if an ISP owns a network 172.16.0.0/16, then the ISP can offer 172.16.1.0/24, 172.16.2.0/24, and so on to customers.

Yet, when advertising to other providers, the ISP only needs to advertise 172.16.0.0/16.
CIDR RANGE VISUALIZER - <https://cidr.xyz>
