# OSI Seven-Layer Model

OSI 7-layer model is a conceptual network format which encourages modular design in networking, meaning that each layer has as little to do with the operation of other layers as possible.

Mnemonic: Please Do Not Throw Sausage Pizza Away. 

The seven layers include:

- Layer 7: Application
- Layer 6: Presentation
- Layer 5: Session
- Layer 4: Transport
- Layer 3: Network 
- Layer 2: Data Link
- Layer 1: Physical

## Layer  7. Application layer:
Being the topmost layer of the OSI model, this is the only layer that directly interacts with data from the user. Software applications like web browsers and email clients rely on the application layer to initiate communications. 

The software applications are not part of the application layer by themselves, but rather the application layer is responsible for the protocols and data manipulation that the software relies on to present meaningful data to the user.

Once the user's data has been gathered, application layer will make a connection with the bottom layer - presentation layer for further manipulation. 

Application layer protocols:
 
- `HTTP` & `HTTPS`
- `FTP`
- `SMTP`
- `DNS`
- `DHCP`
- `SSH`

## Layer 6. Presentation layer:
The presentation layer is responsible for handling the syntax and semantics of the information exchanged between network devices. 

It handles data conversion, encryption and compression.

- Data conversaon 
- Data Encryption 
- Data compression 

## Layer 5. Session layer
In a network, multiple applications running on different systems may need to communicate with one another. This process is called a session, and the Session layer handles these interactions. It provides the mechanisms for establishing a session between applications, maintaining the session and ending the session once the communication is complete. It also provides mechanisms for synchronization and recovery in case of communication failure or interruptions. 

Lets say we are running an email client, a web browser and a torrent applications at the same time. Each of these applications will be using a different session, which is managed by this layer. This is the reason why we can at the same time have a browser, mail client and torrent client open for example. This layer will assign each of these applications a session so that the data can be managed accordingly. 

## Layer 4: Transport Layer - Segmentation and Reassembly
Unlike the name would suggest, transport layer does not transport data between different devices. Transport layer's job is to break down data from the upper layers into digestible chunks and to append header with source and destination [[PORTS|port]] so that the machines on a network know which service said data chunks are meant for.

- **Source port** is required on the source machine in cases where we have multiple applications using the same protocol. If, for example, we had multiple browsers installed, when we try to access a website on one through HTTP, we do not want the result to be opened in another browser. Source port will be a random number between 1024-65535.
- **Destination port** will depend on which service we require from the destination machine, and will be provided by the application layer. It will most commonly be one of the well known ports (1-1024).

This process is called **segmentation**, and said chunks are called **segments** or **datagrams**.

The receiving system then does the **reassembly** of these segments - recognizes the incoming packets as one data transmission, reassembles the packets correctly based on the information included in the packets by the sending system and verifies that all the packets for that piece of data arrived in good shape. 

The primary functions of the Transport Layer are:

- To enable efficient network transmission, the Transport Layer splits the total amount of data it gets from the applications running at the top layers into smaller units known as segments. The Transport Layer puts these bits back together into the original data stream at the other end.

- In situations when organised data transfer is required, the Transport Layer creates a connection between the source and the destination. In order to create the proper parameters and guarantee that both systems are prepared to communicate data, a handshake protocol is established. When the data transfer is complete, the Transport Layer closes the connection.

- The transport layer also assures dependable data transmission. Receiving acknowledgments, or ACK bits, is how this is accomplished. While waiting for the recipient to acknowledge the parts it sent, the sender keeps an eye on them. Any damaged segments are retransmitted by the sender if they receive an acknowledgment.

- Flow regulation - In order to prevent data overload, transport layer regulates the data transfer rate. 
  
- Both error detection and repair are handled by the transport layer. Checksums are one of these techniques for error detection. By computing and validating checksums, it can ascertain whether data was tampered with during transmission. The Transport Layer will request retransmission if it finds anything.

Depending on whether the connection favors performance or stability, we differentiate two transport layer protocols:
- [[TCP vs UDP]]

## Layer 3: Network Layer
Because these segments will need to travel all over the Internet along different pathways, we need a mechanism that knows once the segment does arrive, which pathway it's supposed to be sent down to so that it all gets to the final destination. This is handled by the Network layer.

Network layer will take the segment from the Transport layer, re-encapsulate it by adding a header to it which will have a source and destination IP address. We call this chunk a **packet**.

By far the most common protocol that is used on the Network layer is IPv4 (but it's not the only one).

Network Layer handles functions such as:
- Assigning Logical Address to devices which are connecting to a network to send/receive data packets. 
- Routing - process of identifying the best path to transmit the packets
- Host-to-host delivery or Forwarding - process in which the network layer transmits or forwards the data via routers after determining the best path.
- Logical subnetting - dividing a bigger network into smaller chunks so that IP addresses are used more efficiently. 
- Network Address Translation (NAT) - process of converting any private IP address into a public IP address.

### Network Layer protocols:
- [[Internet Protocol]]
- [[ICMP]]

##  Layer 2: Data-Link Layer
When we say "link", we mean two devices connecting together. Everything up to this layer was a machine readable data. Data-Link layer has a job to take said data and connect it to the last layer - Physical layer. It does this by taking the packet provided by the Network layer and encapsulates it in a **frame**. 

Data-Link does splits its job into  two sublayers:
- **Logical Link Control** (LLC) layer talks to the operating system through drivers and handles multiple network protocols and provides flow control.
- **Media Access Control** (MAC) layer creates and addresses the frame. It adds NIC’s MAC address and attaches MAC addresses to the frame, adds and checks the FCS and ensures that frames are sent along the network cabling. 

A frame will consist of a **header** and a **trailer** attached to it. Trailer exists because much like all the other layers, Data-Link is also governed by protocols (Ethernet being the most common). 

Data transmission involves a stream of signals through a physical device, and in nature there are many signals which are just static noise. In order to differentiate data transferred between devices from a static noise, we attach a trailer to the packet. With it, destination device can recognize said stream of data and know how to handle it.

## Layer 1: Physical layer

Network needs a physical channel through which the data can be transferred. Most common type is the UTP cable (unshielded twisted pair) which can transmit and receive data, as well as a central box (Hub/Switch) that handles a flow from one machine to another. Each system on a network has a cable connected to the central box.

Network Interface Card (NIC) serves as the interface between the PC and the network. Each NIC has a unique identifier with a 48-bit value called media access control address (MAC address) so data is transferred to the right system. No two NICs ever share the same MAC address. Those numbers are provided by Institute of Electrical and Electronics Engineers (IEEE) to the companies making NICs, which often print the numbers onto the device itself using hexadecimal notation. 
- Example: 00–40–05–60–7D–49

First six digits in MAC address represent the number of the NIC manufacturer and they are referred to as Organizationally Unique Identifier (OUI) while the last six digits represent the unique serial number for the NIC and are referred to as device ID. Nomenclature for MAC addresses is also known as MAC-48 and EUI-48 (Extended Unique Identifier, chosen for trademarking purposes). 

NICs transfer the information through the UTP cable in the form of binary data that is broken down into discrete chunks called **frames**. The NICs create and send, as well as receive and read these frames. Different frames are used on different networks and all NICs on the same network must use the same frame type to communicate with other NICs.

(Optional)
	Frame begins with the MAC address of the NIC to which the data is to be sent, followed by the MAC address of the sending NIC. Next comes the **Types** field which indicates the type of data transferred, followed by the **Data** field which contains the encapsulated information. Lastly, there is a special bit of checking information called **frame check sequence**. FCS uses a type of binary math called **cyclic redundancy check** (CRC) in order to verify that the data has arrived intact. Most wired connections hold up to **1500 bytes** of data. 

When a system sends a frame out on the network, the frame goes into the central box. What happens from that point depends on the type of central box.
- Hub could make a copy of the frame and send it to all the connected ports except for the port from which the message originated. Only the NIC to which the frame was addressed would process that frame and the others would drop it when they saw it wasn’t addressed to their MAC addresses.  
- Switch sent a frame only to the interface associated with the destination of the MAC address.

(Optional)
	In situations where NIC doesn’t know the MAC address of another NIC, they send a broadcast onto the network to ask for it. If a NIC sends a frame using a broadcast address (MAC address FF-FF-FF-FF-FF-FF), that frame is processed by every single NIC on that network. That broadcast frame’s data will contain a request for a system’s MAC address. Without knowing the MAC address to begin with, the requesting computer will use an IP address to pick the target computer out of the crowd. The system that is sought will read the request in the broadcast frame and respond its MAC address.

When the frame finally arrives to the destination machine, the entire process will be reversed.
- Data Link will remove the header and a trailer from the frame and pass the packet along to the Network layer.
- Network layer will remove the header and pass the packet to the Transport layer.
- Transport layer will read the destination port and transfer the frame to the appropriate service in the upper layers.

PDU - protocol data unit


# TCP/IP model
The reason why TCP/IP model has this name is because the persons who developed it focused primarily on these two protocols. Back in the day of Arpanet, they needed to be sure that the data was reliable.

##  The Application layer
The Application layer combines the top three layers of the OSI model and its responsible for providing network services and applications to end-users.

Although we can say that the OSI Presentation layer is part of the TCP/IP Application layer, no application requires any particular form of presentation as seen by the OSI model. Applications in TCP/IP already use standardized formats that everyone understands. 

Furthermore, Session Layer's responsibilities are also handled by applications themselves - every application has to be able to initiate, control and disconnect from a remote system. There is no clearly defined way to do this and each TCP/IP application uses its own methods.

All applications can talk to the network, so long they are part of the network. They also need to be part of the network to function: Web browsers, e-mail clients, multiplayer games etc.

Just like in the OSI model, the Application Layer will produce a stream of data which will be handed off to the layer below it.

## The Transport Layer
The Transport Layer is equivalent to the OSI Transport layer. It’s responsible for providing reliable end-to-end communication between applications running on different hosts. 

Much like it's OSI equivalent, Transport Layer will produce **segments** from the data that the Application Layer provided. It doesn't care how data is delivered because at some point of the data transfer, the destination device will have to know how to handle the data which Application Layer produced.

Transport Layer includes two protocols - Transmission Control Protocol (TCP) and User Datagram Protocol (UDP). 

- TCP is a connection-oriented protocol that provides reliable, ordered and error checked delivery of data between applications. As a consequence of adding all of those mechanisms, the data transmission will be reliable but slow.

- UDP is a connectionless communication that provides an unreliable, unordered and unchecked delivery of data. The trade-off is that the data transmission is much faster because it doesn't have to append extra bits to the segment. It’s used with software like VoIP or video streaming.

TCP segments include fields:

- Destination port
- Source port
- Sequence number
- Checksum
- Flags
- Acknowledgement
- Data

UDP on the other hand also gets the data from the Application layer and adds port and length numbers, as well as a checksum to create a container called a UDP datagram. UDP doesn’t care if the receiving computer gets all its data, which is why UDP datagram lacks most of the fields found in TCP segments.

In summary, data arrives from the Application layer which is then broken up into fragments, given destination, source and sequence numbers and then shipped to the Internetworking Layer to be wrapped into an IP packet. 

## The Internetworking Layer
The Internetworking layer is equivalent to OSI Network Layer and is responsible for handling logical addressing and routing (forwarding). 

The main protocol used on this layer Internet Protocol which provides a unique IP address to each device on the network. Any device or protocol that deals with pure IP packets sits on the Internetworking Layer. 

The Internet layer doesn’t care what type of data the IP packet carries, nor does it care whether the data gets there in good order or not. That is handled by the Transport layer.

Internetworking layer will take the segment provided by the Transport Layer and will create a **packet**.

## Network Access Layer
Network Access Layer groups up the last two layers of the OSI model - Data-Link and Physical. The keyword in this layer is **access** - this layer deals with any physical (hardware) part of the network.