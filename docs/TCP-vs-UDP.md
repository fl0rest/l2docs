# TCP
Transmission Control Protocol is a connection-oriented protocol where a stable connection is established between a client and a server before data can be sent. The data transmitted by TCP protocol is in the form of continuous datastreams.

TCP protocol operation is divided into three phases:
- Connection establishment
- Data transfer
- Connection termination

##### Connection establishment (Three-way handshake):
Before a client attempts to connect with a server, the server must first bind to and listen at a port to open it up for connections: this is called a passive open. Once the passive open is established, a client may establish a connection by initiating an active open using the three-way (or 3-step) handshake.

TCP segment will have a header attached to it which will list out a source and destination port, a segment sequence number and a flag which is used to provide additional information regarding data transfer. In case of a 3-way handshake, we deal with following flgs:

1. **SYN**: The active open is performed by the client sending a SYN (synchronize) to the server. The client sets the segment's sequence number to a random value A.
2. **SYN-ACK**: In response, the server replies with a SYN-ACK (synchronize-acknowledge). The acknowledgment number is set to one more than the received sequence number i.e. A+1, and the sequence number that the server chooses for the packet is another random number, B.
3. **ACK**: Finally, the client sends an ACK (acknowledge) back to the server. The sequence number is set to the received acknowledgment value i.e. A+1, and the acknowledgment number is set to one more than the received sequence number i.e. B+1.

Steps 1 and 2 establish and acknowledge the sequence number for one direction (client to server). Steps 2 and 3 establish and acknowledge the sequence number for the other direction (server to client). Following the completion of these steps, both the client and server have received acknowledgments and a full-duplex communication is established.

##### Data transfer
The Transmission Control Protocol differs in several key features compared to the User Datagram Protocol:

- Ordered data transfer: the destination host rearranges segments according to a sequence number
- Retransmission of lost packets: any cumulative stream not acknowledged is retransmitted
- Error-free data transfer: corrupted packets are treated as lost and are retransmitted
- Flow control: limits the rate a sender transfers data to guarantee reliable delivery. The receiver continually hints the sender on how much data can be received. When the receiving host's buffer fills, the next acknowledgment suspends the transfer and allows the data in the buffer to be processed.
- Congestion control: lost packets (presumed due to congestion) trigger a reduction in data delivery rate

##### Connection termination
The connection termination phase uses a four-way handshake, with each side of the connection terminating independently. When an endpoint wishes to stop its half of the connection, it transmits a FIN packet, which the other end acknowledges with an ACK. Therefore, a typical tear-down requires a pair of FIN and ACK segments from each TCP endpoint. After the side that sent the first FIN has responded with the final ACK, it waits for a timeout before finally closing the connection, during which time the local port is unavailable for new connections; this state lets the TCP client resend the final acknowledgment to the server in case the ACK is lost in transit. 

The time duration is implementation-dependent, but some common values are 30 seconds, 1 minute, and 2 minutes. After the timeout, the client enters the CLOSED state and the local port becomes available for new connections.

It is also possible to terminate the connection by a 3-way handshake, when host A sends a FIN and host B replies with a FIN & ACK (combining two steps into one) and host A replies with an ACK.

Source: https://en.wikipedia.org/wiki/Transmission_Control_Protocol

# UDP
UDP is the “fire and forget” type of a protocol. It uses a connectionless communication model with a minimum of protocol mechanisms. 

UDP datagram header doesn’t possess any of the extra fields TCP segment headers carry to make sure the data is received intact. The trade-off is that a transmission of data is far faster because more data can be packed into a datagram. As such, UDP is utilized in cases where speed is preferable to stability.

UDP works best when you have a lot of data that doesn’t need to be perfect or when the systems are so close to each other that the chances of a problem occurring are too small to bother worrying about. A few dropped frames on a Voice over IP call, for example, won’t make much difference in the communication between two people. 

Two of the most important networking protocols, Domain Name System (DNS) and Dynamic Host Configuration Protocol (DHCP) also use UDP. 

DHCP uses UDP because DHCP server can’t assume another computer is ready on either side of the session, so each step of a DHCP session just sends the information for that step without any confirmation. 

Sending a connectionless datagram also makes sense because the client won’t have an IP address to begin the three-way hand-shake. DHCP uses two port numbers: DHCP clients use port 68 for sending data to and receiving data from the DHCP server, and DHCP servers use port 67 for sending and receiving data to and from DHCP clients.