DNS (Domain Name System)
========================

Humans access information online through domain names, like nytimes.com or espn.com. Web browsers interact through Internet Protocol (IP) addresses. DNS translates domain names to IP addresses so browsers can load Internet resources.

The process of DNS resolution involves converting a hostname (such as www.example.com) into a computer-friendly IP address (such as 192.168.1.1). An IP address is given to each device on the Internet, and that address is necessary to find the appropriate Internet device - like a street address is used to find a particular home. When a user wants to load a webpage, a translation must occur between what a user types into their web browser (example.com) and the machine-friendly address necessary to locate the example.com webpage.

## DNS lookup

1. A DNS lookup typically refers to the process of converting easy to remember names called domain names (like www.google.com) into numbers called IP addresses (like 192.168.2.1).
A user types the URL example.com into their web browser.
    1.1. Browser cache, device cache, hosts file, router and ISP cache
2. The user’s computer sends a request to the recursive resolver.
3. The recursive resolver then sends a request to the root nameserver which provides the address . of the TLD nameserver responsible for .com domain names.
4. The root nameserver returns the result of the TLD nameserver to the recursive resolver.
5. The recursive resolver sends a request to the .com TLD nameserver which provides the address of the authoritative nameserver responsible for the example.com domain.
6. The TLD nameserver returns the result of the authoritative nameserver to the recursive resolver.
7. The recursive resolver sends a request to the authoritative nameserver responsible for example.com which provides the DNS records requested.
8. The authoritative nameserver returns results to the recursive resolver.
9. The recursive resolver returns DNS records containing the IP address to the browser.
10. The browser makes a request directly to the IP address of the server hosting the website.

### Host file

A hosts file which is used by operating systems to map a connection between an IP address and domain names before going to domain name servers. This file is a simple text file with the mapping of IPs and domain names.
Can override DNS and redirect URLs or IP addresses to different locations. 

## DNS servers
### DNS recursor - recursive resolver

*The recursive resolver server is designed to receive browser queries and is in charge of adding new ones.*

The recursor can be thought of as a librarian who is asked to go find a particular book somewhere in a library. The DNS recursor is a server designed to receive queries from client machines through applications such as web browsers. Typically the recursor is then responsible for making additional requests in order to satisfy the client’s DNS query.
A recursive resolver (also known as a DNS recursor) is the first stop in a DNS query. The recursive resolver acts as a middleman between a client and a DNS nameserver. After receiving a DNS query from a web client, a recursive resolver will either respond with cached data, or send a request to a root nameserver, followed by another request to a TLD nameserver, and then one last request to an authoritative nameserver. After receiving a response from the authoritative nameserver containing the requested IP address, the recursive resolver then sends a response to the client.
During this process, the recursive resolver will cache information received from authoritative nameservers. When a client requests the IP address of a domain name that was recently requested by another client, the resolver can circumvent the process of communicating with the nameservers, and just deliver the client the requested record from its cache.
Most Internet users use a recursive resolver provided by their ISP.

### Root nameserver

*Used as reference and points to other specific locations.*

The root server is the first step in translating (resolving) human readable host names into IP addresses. It can be thought of like an index in a library that points to different racks of books - typically it serves as a reference to other more specific locations.
The 13 DNS root nameservers are known to every recursive resolver, and they are the first stop in a recursive resolver’s quest for DNS records. A root server accepts a recursive resolver’s query which includes a domain name, and the root nameserver responds by directing the recursive resolver to a TLD nameserver, based on the extension of that domain (.com, .net, .org, etc.). The root nameservers are overseen by a nonprofit called the Internet Corporation for Assigned Names and Numbers (ICANN).
Note that while there are 13 root nameserver hostnames, that does not mean that there are only 13 machines in the root nameserver system. There are 13 types of root nameservers, but there are multiple copies of each one all over the world, which use Anycast routing to provide speedy responses. If you added up all the instances of root nameservers, you’d have over 600 different servers.

### TLD nameserver

*Contains info on TLD (.com, .net…)* 

The top level domain server (TLD) can be thought of as a specific rack of books in a library. This nameserver is the next step in the search for a specific IP address, and it hosts the last portion of a hostname (In example.com, the TLD server is “com”).
A TLD nameserver maintains information for all the domain names that share a common domain extension, such as .com, .net, or whatever comes after the last dot in a URL. For example, a .com TLD nameserver contains information for every website that ends in ‘.com’. If a user was searching for google.com, after receiving a response from a root nameserver, the recursive resolver would then send a query to a .com TLD nameserver, which would respond by pointing to the authoritative nameserver (see below) for that domain.

* Generic top-level domains: These are domains that are not country specific, some of the best-known generic TLDs include .com, .org, .net, .edu, and .gov.
* Country code top-level domains: These include any domains that are specific to a country or state. Examples include .uk, .us, .ru, and .jp.


### Authoritative nameserver

*Should have info on the requested site (it contains the DNS zone).*

This final nameserver can be thought of as a dictionary on a rack of books, in which a specific name can be translated into its definition. The authoritative nameserver is the last stop in the nameserver query. If the authoritative name server has access to the requested record, it will return the IP address for the requested hostname back to the DNS Recursor (the librarian) that made the initial request.
When a recursive resolver receives a response from a TLD nameserver, that response will direct the resolver to an authoritative nameserver. The authoritative nameserver is usually the resolver’s last step in the journey for an IP address. The authoritative nameserver contains information specific to the domain name it serves (e.g. google.com) and it can provide a recursive resolver with the IP address of that server found in the DNS A record, or if the domain has a CNAME record (alias) it will provide the recursive resolver with an alias domain, at which point the recursive resolver will have to perform a whole new DNS lookup to procure a record from an authoritative nameserver (often an A record containing an IP address). 

<https://www.cloudflare.com/learning/dns/dns-server-types/> 


## DNS records

### A record

The record that holds the IP address of a domain. The "A" stands for "address" and this is the most fundamental type of DNS record: it indicates the IP address of a given domain.
The most common usage of A records is IP address lookups: matching a domain name (like "cloudflare.com") to an IPv4 address. This enables a user's device to connect with and load a website, without the user memorizing and typing in the actual IP address. 

* DNS AAAA records match a domain name to an IPv6 address. DNS AAAA records are exactly like DNS A records, except that they store a domain's IPv6 address instead of its IPv4 address.

### CNAME record

Forwards one domain or subdomain to another domain, does NOT provide an IP address. 
The ‘canonical name’ (CNAME) record is used in lieu (in substitution) of an A record, when a domain or subdomain is an alias of another domain. All CNAME records must point to a domain, never to an IP address. 

* CNAME restrictions - MX and NS records cannot point to a CNAME record; they have to point to an A record (for IPv4) or an AAAA record (for IPv6). 

### MX record

An MX record is a mail exchange record that directs email to a mail server. The MX record indicates how email messages should be routed in accordance with the Simple Mail Transfer Protocol (SMTP, the standard protocol for all email). Like CNAME records, an MX record must always point to another domain.

### TXT record

Lets an admin store text notes in the record. These records are often used for email security. Today, two of the most important uses for DNS TXT records are email spam prevention and domain ownership verification, although TXT records were not designed for these uses originally.
One domain can have many TXT records.

* #### SPF record
A sender policy framework (SPF) record is a type of DNS TXT record that lists all the servers authorized to send emails from a particular domain.
<https://www.proofpoint.com/us/threat-reference/spf>

* #### DKIM record 
DomainKeys Identified Mail (DKIM) is a method of email authentication that helps prevent spammers and other malicious parties from impersonating a legitimate domain.
<https://www.proofpoint.com/us/threat-reference/dkim>

* #### DMARC 
Domain-based Message Authentication Reporting and Conformance (DMARC) is a method of authenticating email messages. A DMARC policy tells a receiving email server what to do after checking a domain's Sender Policy Framework (SPF) and DomainKeys Identified Mail (DKIM) records.

Together, DMARC, DKIM, and SPF function like a background check on email senders, to make sure they really are who they say they are.

### NS record

NS stands for ‘nameserver,’ and the nameserver record indicates which DNS server is authoritative for that domain (i.e. which server contains the actual DNS records). Basically, NS records tell the Internet where to go to find out a domain's IP address. A domain often has multiple NS records which can indicate primary and secondary nameservers for that domain. Without properly configured NS records, users will be unable to load a website or application.

### SOA record

‘Start of authority’ (SOA) record stores important information about a domain or zone such as name of a primary DNS server (hostname of the primary DNS server for the zone, and it should contain a matching NS record), the email address of the administrator, when the domain was last updated, serial number (used by secondary DNS servers to check if the zone has changed), and how long the server should wait between refreshes.

Every DNS database file begins with a Start of Authority (SOA) resource record. Whenever you alter any data in a DNS database file, you must increment the SOA serial number by one integer.

For example, if the current SOA Serial Number in a data file is 101, and you make a change to the file's data, you must change 101 to 102. If you don't change the SOA serial number, the domain's slave servers do not update their copy of the database files with the new information. The master and slave servers would then be out of sync.

### SRV record

The DNS "service" (SRV) record specifies a host and port for specific services such as voice over IP (VoIP), instant messaging, and so on. Most other DNS records only specify a server or an IP address, but SRV records include a port at that IP address as well. Some Internet protocols require the use of SRV records in order to function.

### PTR record

Provides a domain name in reverse-lookups.  A DNS pointer record (PTR for short) provides the domain name associated with an IP address. A DNS PTR record is exactly the opposite of the 'A' record, which provides the IP address associated with a domain name.
DNS PTR records are used in reverse DNS lookups. PTR records store IP addresses with their segments reversed, and they append ".in-addr.arpa" to that. 

For example if a domain has an IP address of 192.0.2.1, the PTR record will store the domain's information under 1.2.0.192.in-addr.arpa.

### GLUE record

Glue records are DNS records created at the domain’s registrar. The record provides a complete answer when the TLD nameserver returns a reference for an authoritative nameserver for a domain. 
Creating a glue record, an A record served by the TLD nameserver, avoids circular references and allows for both DNS name resolution and listing the nameservers inside the domain itself.
Glue records can only be created at the domain registrar as the registrar controls the DNS settings for a given domain’s delegation. Every nameserver on the internet has its own glue record created by the domain’s owner.
In normal DNS resolution, when a resolver attempts to resolve a domain name, it first queries the root, which provides the top-level domain. Next, it queries the top-level domain servers, which provide the domain’s authoritative nameservers. Finally, it queries the authoritative nameservers for the domain to resolve the domain name. If the nameservers for a domain exist inside the domain itself, a glue record is needed to resolve the domain name.

<https://www.cloudflare.com/learning/dns/dns-records/>