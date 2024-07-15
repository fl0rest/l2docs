## Domain name

Simply put, a domain name (or just ‘domain’) is the name of a website. It’s what comes after “@” in an email address, or after “www.” in a web address. If someone asks how to find you online, what you tell them is usually your domain name.
Domain names are typically broken up into two or three parts, each separated by a dot.
To the left of the `TLD` is the second-level domain (`2LD`) and if there is anything to the left of the `2LD`, it is called the third-level domain (`3LD`). 
For Google’s US domain name, ‘google.com’:

* ’.com’ is the `TLD` (most general)
* ’google’ is the `2LD` (most specific)

Difference between a domain name and a `URL`? A uniform resource locator (`URL`), sometimes called a web address, contains the domain name of a site as well as other information, including the protocol and the path. For example, in the `URL` ‘https://example.com/learning/’, example.com’ is the domain name, while ‘https’ is the protocol and ‘/learning/’ is the path to a specific page on the website.

### FQDN - Fully qualified domain name

Check - <https://en.wikipedia.org/wiki/Fully_qualified_domain_name>

A fully qualified domain name (`FQDN`) is a part of the uniform resource locator (`URL`). As the name suggests, it is the full name of an entity in the internet framework, including a host and a computer.

`FQDNs` are usually used in any interaction on the internet as they are easier to remember than IP addresses. Below are several scenarios of when to use an `FQDN`:

* Getting an SSL certificate. A secure sockets layer (`SSL`) protects a connection between a web server and a browser. An `SSL` certificate is issued to an `FQDN`, so you may not be able to use `SSL` services properly without it.

* Connecting to a remote host. You can make a remote host or a virtual machine by specifying any `FQDN`, enabling the DNS to look at its DNS table and locate the server. If you use only the hostname to connect to a server, your application may not be able to resolve the hostname.

* Accessing a specific domain service or protocol. Activities transferring information across a network generally involve the DNS, including pointing to an `FQDN`. An example is when you connect to a File Transfer Protocol (FTP) or an email server.

* Migrating to a new server. If you want to migrate your service to a server with a different IP address, using an `FQDN` instead of an IP address lets you quickly change the DNS record and reduce outages in the IP address changes.