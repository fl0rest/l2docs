!!! info "Keywords" 

	MUA (Mail User Agent) - Webmails, Clients (Outlook, Thunderbird)
	MDA (Mail Delivery Agent)
    MTA (Mail Transfer Agent)
    SMTP (Simple Mail Transfer Protocol)
    IMAP (Internet Message Access Protocol)
    POP3 (Post Office Protocol 3)
    MX records (Mail Exchange Records)
    SPF (Sender Policy Framework)
    DKIM (Domain Keys Identified Mail)
    DMARC (Domain-based Message Authentication and Conformance)

## Process

### Mail User Agent

MUA (Mail User Agent) or an email client is a software or a web application that allows users to compose, send, and manage their emails. Examples include Microsoft Outlook, Mozilla Thunderbird, and Apple Mail (software application) and RoundCube, SquirrelMail and Horde (web applications). 

Our customers can also send emails through their websites using default PHP mailer or SMTP plugins - form submissions are one example. If form submission is set up, their customers will fill out the form and the website will send out an email automatically as a “quasi” MUA.

### Mail Transfer Agent/SMTP

Once an email is sent, an email client will communicate with an MTA - Mail Transfer Agent - the SMTP server. 

SMTP server utilizes software like exim, qmail or postifx to send a message to the recipient's mail server. The SMTP server will take an email message from a client and prepare it for a delivery by adding necessary headers like sender and recipient addresses and append the details required for a transmission. 

Afterwards, the SMTP server performs a DNS lookup to find the recipient server. Once the recipient’s mail server is identified, your email server establishes a connection with it using the SMTP (Simple Mail Transfer Protocol) which facilitates the transmission of email messages between servers over ports  25/465/587. The email message is transferred from the sender's server to the recipient's server through this connection.

The recipient's SMTP server receives incoming email messages from sender's SMTP server and verifies the validity of the email through various checkups such as:

- DNS record verification - SPF, DKIM, DMARC, PTR
- Server side SPAM rules

### Mail Delivery Agent

When the validity of the email is verified, recipient SMTP server will invoke MDA like Dovecot to store the incoming email in the proper user's mailbox.  Once the email is delivered, MUA utilizes IMAP/POP3 protocols to retrieve emails from a mail server for the user.

## POP3

Post Office Protocol 3 is an email protocol that clients use to retrieve email from a mail server. POP3 clients connect, retrieve all messages, store them on the client computer, and finally delete them from the server (although there is an option for email clients to leave the message on the server or simply download a local copy). POP3 treats the mailbox as a single store, and has no concept of folders. It also provides a completely static view of the current state of the mailbox and does not provide a mechanism to show any external changes in state during the session.
  
This design of POP and its procedures was driven by the need of users having only temporary Internet connections, such as dial-up access, allowing these users to retrieve email when connected, and subsequently to view and manipulate the retrieved messages when offline. A POP3 server listens on port number 110 for service requests or port number 995 for secure connection (POP3S).

## IMAP

The Internet Message Access Protocol allows a client to access and manipulate electronic mail messages on a server, as well as manipulation of mailboxes  in a way that is functionally equivalent to local folders. It includes operations for creating, deleting and renaming mailboxes; checking for new messages; removing messages permanently; setting and clearing flags (read, replied to, forwarded, deleted etc); searching; and selective fetching of message attributes, texts and portions thereof. IMAP4 server listens on port 143 which is an unsecure channel or port 993 which is an implicit TLS port.

Visual
![[Mail delivery.png]]

## Troubleshooting email delivery failure

Each step of this process can be a potential point of failure (that's why emails are so hard to troubleshoot). 

1. MUA: 
	Confirm with the client how their emails are being sent, is it an email or is it a form submission? 
		If it's an email, which email client are they using? Have they tried using the webmail client we offer? 
		If it's a form submission, how is it setup? Are they using a default PHP mailer or are they using an SMTP plugin? Access the admin page and review the configuration they have setup. 
		Check with them which mailbox the email is sent from and whether that is the only affected mailbox. Create a test mailbox and send an email to check whether the message is going through or not by sending it to your account.
2. MTA: 
	Check for delivery failures in the logs;
		Once you know which domain/mailbox are affected, you will be grepping the logs:
		`$ cat /var/log/send/current | tai64nlocal | grep -iC 5 <domain.com>` 
		This command will allow you to check for emails that were sent from a particular domain. 
		# Side note - it is also possible to check cat /var/log/smtp/current log, however this log won't reveal much of anything. For basic troubleshooting, check the send log.
3. MDA: 
	On managed apps we use dovecot. You can check the dovecot logs for information such as whether the user logging in is using IMAP/POP3 connection for example.
		`cat /var/log/dovecot/dovecot.log`

## Breakdown of the mailing process using qmail

Example of qmail log:
```shell
$ cat /var/log/send/current | tai64nlocal | grep 574692321
2023-07-26 14:28:27.917076500 info msg 574692321: bytes 16593 from <outlook_553EE1FF76D81E16@outlook.com> qp 5228 uid 108

2023-07-26 14:28:27.920988500 starting delivery 373: msg 574692321 to local myspareparts.com-maurice.niccoli@myspareparts.com

2023-07-26 14:28:27.921004500 status: local 1/10 remote 0/255

2023-07-26 14:28:27.921011500 starting delivery 374: msg 574692321 to local myspareparts.com-ben.veldman@myspareparts.com

2023-07-26 14:28:27.921023500 status: local 2/10 remote 0/255

2023-07-26 14:28:28.014981500 delivery 374: success: vdelivermail:_valiases_processed/did_0+0+1/

2023-07-26 14:28:28.014983500 status: local 1/10 remote 0/255
2023-07-26 14:28:28.046694500 delivery 373: success: vdelivermail:_valiases_processed/did_0+0+1/

2023-07-26 14:28:28.046768500 status: local 0/10 remote 0/255
2023-07-26 14:28:28.046819500 end msg 574692321
```

Step by step breakdown:

`$ tail /var/log/send/current | tai64nlocal`
Lookup command:
- `tai64nlocal`  <- Time formatting

`2023-07-26 14:28:27.917076500 info msg 574692321: bytes 16593 from <outlook_553EE1FF76D81E16@outlook.com> qp 5228 uid 108`

- `2023-07-26 14:28:27.917076500` <- Timestamp
- `msg 574692321`  <- Message number that can be used to tie log entries together and follow the process of any given message in the queue.
- `from <outlook_553EE1FF76D81E16@outlook.com>` <- The address to which any bounce messages will be sent if any of the recipients are unable or unwilling to accept the message.
- `qp 5228` <- process ID of the qmail-queue process which added the message to the queue. It's used to link the qmail-queue and qmail-send logs together.
- `uid 108` <- numeric UID under which the qmail-queue process was running when the message was added to the queue. 

`2023-07-26 14:28:27.920988500 starting delivery 373: msg 574692321 to local myspareparts.com-maurice.niccoli@myspareparts.com`

- `starting delivery 373` <- Message delivery has been started. 
- `to local myspareparts.com-maurice.niccoli@myspareparts.com` <- This message tells you whether this is a local or remote delivery and the recipient's mail address.

`2023-07-26 14:28:27.921004500 status: local 1/10 remote 0/255` 

- `status: local 1/10 remote 0/255` <- Every time a delivery process starts or ends, this message is sent to the log. In this example there are 10 concurrent local delivery slots available and 1 of those slots is being used, and there are 255 concurrent remote delivery slots available and 0 of them are being used

`2023-07-26 14:28:28.014981500 delivery 374: success: vdelivermail:_valiases_processed/did_0+0+1/` 

- `delivery 374: success: vdelivermail:_valiases_processed/did_0+0+1/ ` <- The delivery attempt 374 is finished and it was successful. The delivery code can be one of the following:
	- `success` <- For a local delivery, qmail-local was able to successfully process a delivery. This can mean one of three things:
		- Message was stored in one or more Maildirs, forwarded to zero or more other email addresses, or processed through zero or more programs. The extra info looks like `did_0+0+1` :
			- message was stored in 0 Maildirs
			- message was forwarded to 0 mail addresses
			- message was processed by 1 program.
	- `success` <-For a remote delivery, qmail-remote was able to contact a remote SMTP server and that server accepted the message. That's all it tells you, it does not guarantee that the other server will be able to deliver the message to the correct mailbox, only that it accepted the responsibility for getting the message to where it needs to be.
	 
	- `deferral`<- There was some kind of a temporary error which prevented the delivery from succeeding, but the delivery will be tried again after a delay. The extra information will usually tell us why the message was not accepted.
	  
	- `failure` <-Delivery did not succeed and there isa good reason to believe that it will never succeed, so qmail-send won't attempt to send a message again.  

- `2023-07-26 14:28:28.046768500 status: local 0/10 remote 0/255
  `2023-07-26 14:28:28.046819500 end msg 574692321` <- The queue is empty and the message is being deleted from the queue. 

Log locations:

- qmail-send `/var/log/send/current`
- qmail-smtp inc&out `/var/log/smtp/current`
- IMAP  (legacy SIP) `/var/log/imap4-ssl/current`
- POP  (legacy SIP)`/var/log/pop3-ssl/current`
- POP and IMAP (cloudhost and recent dedicated servers) `/var/log/dovecot`
- Inbound SpamAssassin scoring `/var/log/maillog`

*[MUA]: Mail User Agent
*[MDA]: Mail Delivery Agent
*[MTA]: Mail Transfer Agent
*[SMTP]: Simple Mail Transfer Protocol
*[IMAP]: Internet Message Access Protocol
*[POP3]: Post Office Protocol 3
*[MX]: Mail Exchange Records
*[SPF]: Sender Policy Framework
*[DKIM]: Domain Keys Identified Mail
*[DMARC]: Domain-based Message Authentication and Conformance