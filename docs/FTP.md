FTP is a protocol that relies on two communications channels between the client and server: a command channel for controlling the conversation and a data channel for transmitting file content.

FTP transfer process:

1. A user typically needs to log on to the FTP server, although some servers make some or all of their content available without a login, a model known as _anonymous FTP

2. The client initiates a conversation with the server when the user requests to download a file.

3. Using FTP, a client can upload, download, delete, rename, move and copy files on a server.

Users can work with FTP via a simple command-line interface -- terminal -- or with a dedicated graphical user interface.  Most common client is  **FileZilla** that supports FTP, FTPS and SFTP, but web browsers can also serve as FTP clients. There are several different ways an FTP server and client software can conduct a file transfer using FTP:

- **Anonymous FTP.** This is the most basic form of FTP. It provides support for data transfers without encrypting data or using a username and password. It's most commonly used for download of material that is allowed for unrestricted distribution. 
- **Password-protected FTP.** This is also a basic FTP service, but it requires the use of a username and password, though the service might not be encrypted or secure. It also works on port 21.
- **FTP Secure (FTPS)** is an extension of FTP that adds security through the use of Transport Layer Security (TLS) or Secure Sockets Layer (SSL) encryption. It provides an additional layer of protection by encrypting the control and data connections, preventing unauthorized access and data interception. FTPS can use either implicit SSL/TLS (FTPIS) or explicit SSL/TLS (FTPS). Implicit FTPS requires SSL/TLS encryption from the start, while explicit FTPS allows the client and server to negotiate encryption after the initial connection. It typically defaults to using port 990.
- **Secure FTP (SFTP)** - This is technically not an FTP protocol, but it functions similarly. Rather, SFTP is a subset of the Secure Shell (SSH) protocol that runs over port 22. SSH is commonly used by systems administrators to remotely and securely access systems and applications, and SFTP provides a mechanism within SSH for secure file transfer.

By default, FTP does not encrypt traffic, and individuals can capture packets to read usernames, passwords and other data. By encrypting FTP with FTPS data is protected, limiting the ability of an attacker to eavesdrop on a connection and steal data.