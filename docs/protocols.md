## What is the Internet Protocol (IP)?

The Internet Protocol (IP) is a protocol, or set of rules, for routing and addressing packets of data so that they can travel across networks and arrive at the correct destination. Data traversing the Internet is divided into smaller pieces, called packets. IP information is attached to each packet, and this information helps routers to send packets to the right place. Every device or domain that connects to the Internet is assigned an IP address, and as packets are directed to the IP address attached to them, data arrives where it is needed.

Once the packets arrive at their destination, they are handled differently depending on which transport protocol is used in combination with IP. The most common transport protocols are TCP and UDP.

## What is a network protocol?

In networking, a protocol is a standardized way of doing certain actions and formatting data so that two or more devices are able to communicate with and understand each other.

To understand why protocols are necessary, consider the process of mailing a letter. On the envelope, addresses are written in the following order: name, street address, city, state, and zip code. If an envelope is dropped into a mailbox with the zip code written first, followed by the street address, followed by the state, and so on, the post office won't deliver it. There is an agreed-upon protocol for writing addresses in order for the postal system to work. In the same way, all IP data packets must present certain information in a certain order, and all IP addresses follow a standardized format.

## What is an IP address? How does IP addressing work?

An IP address is a unique identifier assigned to a device or domain that connects to the Internet. Each IP address is a series of characters, such as '192.168.1.1'. Via DNS resolvers, which translate human-readable domain names into IP addresses, users are able to access websites without memorizing this complex series of characters. Each IP packet will contain both the IP address of the device or domain sending the packet and the IP address of the intended recipient, much like how both the destination address and the return address are included on a piece of mail.

## IPv4 vs. IPv6
The fourth version of IP (IPv4 for short) was introduced in 1983. However, just as there are only so many possible permutations for automobile license plate numbers and they have to be reformatted periodically, the supply of available IPv4 addresses has become depleted. IPv6 addresses have many more characters and thus more permutations; however, IPv6 is not yet completely adopted, and most domains and devices still have IPv4 addresses.

## What is an IP packet?
IP packets are created by adding an IP header to each packet of data before it is sent on its way. An IP header is just a series of bits (ones and zeros), and it records several pieces of information about the packet, including the sending and receiving IP address. IP headers also report:

* Header length
* Packet length
* Time to live (TTL), or the number of network hops a packet can make before it is discarded
* Which transport protocol is being used (TCP, UDP, etc.)

In total there are 14 fields for information in IPv4 headers, although one of them is optional.

## How does IP routing work?

The Internet is made up of interconnected large networks that are each responsible for certain blocks of IP addresses; these large networks are known as autonomous systems (AS). A variety of routing protocols, including BGP, help route packets across ASes based on their destination IP addresses. Routers have routing tables that indicate which ASes the packets should travel through in order to reach the desired destination as quickly as possible. 

Packets travel from AS to AS until they reach one that claims responsibility for the targeted IP address. That AS then internally routes the packets to the destination.

Packets can take different routes to the same place if necessary, just as a group of people driving to an agreed-upon destination can take different roads to get there.

## What is TCP/IP?
 
The Transmission Control Protocol (TCP) is a transport protocol, meaning it dictates the way data is sent and received. A TCP header is included in the data portion of each packet that uses TCP/IP. Before transmitting data, TCP opens a connection with the recipient. TCP ensures that all packets arrive in order once transmission begins. Via TCP, the recipient will acknowledge receiving each packet that arrives. Missing packets will be sent again if receipt is not acknowledged.

TCP is designed for reliability, not speed. Because TCP has to make sure all packets arrive in order, loading data via TCP/IP can take longer if some packets are missing.

TCP and IP were originally designed to be used together, and these are often referred to as the TCP/IP suite. However, other transport protocols can be used with IP.

## What is UDP/IP?

The User Datagram Protocol, or UDP, is another widely used transport protocol. It is faster than TCP, but it is also less reliable. UDP does not make sure all packets are delivered and in order, and it does not establish a connection before beginning or receiving transmissions.