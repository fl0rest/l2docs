## SSL - Secure Sockets Layer

SSL, or Secure Sockets Layer, is an encryption-based Internet security protocol. SSL encrypts data that is transmitted across the web. SSL is the predecessor to the modern TLS (Transport Layer Security) encryption used today (but people still refer to it as SSL).

Anyone who tries to intercept this data will only see a garbled mix of characters that is nearly impossible to decrypt.

SSL initiates an authentication process called a handshake between two communicating devices to ensure that both devices are really who they claim to be. SSL also digitally signs data in order to provide data integrity, verifying that the data is not tampered with before reaching its intended recipient.

* Secure Socket Layer
* SSL encrypts data that is transmitted across the web
* It initiates an auth process called a handshake between the two communicating devices to make sure that both are what they claim to be
* It’s now TLS, but still called SSL

<https://www.cloudflare.com/learning/ssl/what-is-ssl/>

SSL (Secure Sockets Layer) also referred to as the TLS (Transport Layer Security) is a form of asymmetric encryption of data on the internet. 

Originally, data on the Web was transmitted in plaintext that anyone could read if they intercepted the message. For example, if a consumer visited a shopping website, placed an order, and entered their credit card number on the website, that credit card number would travel across the Internet unconcealed. SSL was created to correct this problem and protect user privacy. 

By encrypting any data that goes between a user and a web server, SSL ensures that anyone who tries to intercept this data will only see a garbled mix of characters that is nearly impossible to decrypt. 

### What is an SSL certificate?

An SSL certificate is a text file hosted in a website's origin server which contains the website's public key and devices attempting to communicate with the origin server will reference this file to obtain the public key and verify the server's identity. 

In order to get an SSL certificate, a Certificate Signing Request (CSR) has to be created first. Information that CSR includes is:

* Fully-Qualified Domain Name (FQDN)
* Legal name of the organization
* Division of the Organization
* City
* Region
* Country
* Email address

Once a CSR is created, an SSL certificate can then be purchased from the CA. A CA is an outside organization, a trusted third party, that generates and gives out SSL certificates. Once the certificate is issued, it needs to be installed and activated on the website's origin server and when it's activated on the origin server, the website will be able to load over HTTPS and all traffic to and from the website will be encrypted and secure.

SSL certificates include the following information in a single data file:

* The domain name that the certificate was issued for
* Which person, organization, or device it was issued to
* Which certificate authority issued it
* The certificate authority's digital signature
* Associated subdomains
* Issue date of the certificate
* Expiration date of the certificate
* The public key (the private key is kept secret)

The public and private keys used for SSL are essentially long strings of characters used for encrypting and signing data. Data encrypted with the public key can only be decrypted with the private key during the process known as an SSL handshake:

* Step 1: The SSL/TLS client will send the server a “ClientHello” message that includes information about the browser, the OS, SSL/TLS version and the encryption algorithm that the client supports.

* Step 2: The SSL/TLS server sends back a “ServerHello” message with an agreement on which encryption algorithm they should use during the session, it's digital certificate containing the domain name and certificate authority, and the public key they would use for encryption.

* Step 3: The client then confirms that the web server’s digital certificate is valid and issued by a CA. 

	Technically, anyone can create their own SSL certificate by generating a public-private key pairing and including all the information mentioned above. Such certificates are called self-signed certificates because the digital signature used, instead of being from a CA, would be the website's own private key.
	
	But with self-signed certificates, there's no outside authority to verify that the origin server is who it claims to be. Browsers don't consider self-signed certificates trustworthy and may still mark sites with one as "not secure," despite the https:// URL. They may also terminate the connection altogether, blocking the website from loading.
  
* Step 4: If the certificate is valid, client extracts the public key and generates a new key for that particular session, then encrypts the session key with the public key and sends it back to the server.
* Step 5: The SSL/TLS server decrypts the session key using its private key.
* Step 6: Both the client and the server now use the session key to communicate.
* Step 7: Finally, both the server and the client now exchange the finished messages using the new session key. This message states that the handshake is complete. 

### Types of SSL certificates:

* Number of sites they cover:
	* Single domain
	* Wildcard - includes the main domain and all of the subdomains
	* Multi-domain - applies to multiple unrelated domains
* Validation level:
	* Domain validation - business has to prove they control the domain
	* Organization validation - CA directly contacts a person or business requesting the certificate
	* Extended validation - full background check of an organization 

### SSL verification methods:

* HTTP validation method:
	Inside of docroot create a directory .well-known/pki-validation/gsdv.txt, and inside of it add the metatag that SSL provides
* Email verification
* DNS verification