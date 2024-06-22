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
## DNS servers
### DNS recursor
### Root nameserver
### TLD nameserver
### Authoritative nameserver
## DNS records
### A record
### CNAME record
### MX record
### TXT record
#### SPF record
#### DKIM record
#### DMARC record