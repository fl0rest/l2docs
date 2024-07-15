# SPF/DKIM/DMARC EXPLAINED:

SPF, DKIM, and DMARC: The Three Musketeers of Email Authentication

In the world of email security, SPF, DKIM, and DMARC are three essential protocols that help prevent spam and phishing attacks by verifying the authenticity of email messages.

**SPF** verifies the authenticity of an email sender's domain

**DKIM** verifies the authenticity of an email message using digital signatures

**DMARC** provides additional features for email authentication and reporting, and specifies how receivers should handle failed authentication attempts

Domain-based Message Authentication, Reporting, and Conformance (DMARC) works with Sender Policy Framework (SPF) and DomainKeys Identified Mail (DKIM) to authenticate mail senders to your recipient mail servers (the personnel you are sending emails to).

These are three protection mechanisms that help maintain mail purity as it travels to your clients, but is not an automatic/universal measure in many cases across many service providers.

DMARC ensures the destination email systems trust messages sent from your domain. Using DMARC with SPF and DKIM gives organizations like yours protection against spoofing and phishing emails. DMARC helps mail systems that are receiving your messages decide what to do with those nasty "messages" pretending to be from your domain that fail SPF or DKIM checks.

## How do SPF and DMARC work together?

An email message can contain multiple originator or sender addresses. These addresses are used for different purposes. For example, look at these addresses:

- "Mail From" address:
This identifies the sender and says where to send return notices if any problems occur with the delivery of the message (such as non-delivery notices). The Mail From address appears in the envelope portion of an email message and isn't displayed by your email application, and is sometimes called the 5321.MailFrom address or the reverse-path address. You won't see it unless you examine the headers, but believe me, it's there.

- "From" address:
This is the address displayed as the From address by your mail application. The From address identifies the author of the email. That is, the mailbox of the person or system responsible for writing the message. The From address is sometimes called the 5322.From address.

SPF uses a DNS TXT record to list the specific authorized sending IP addresses and services for a domain. Normally, SPF checks only get performed against the 5321.MailFrom address. The 5322.From address (remember, that's the second one) isn't authenticated when you use SPF by itself, which allows for a scenario where a user gets a message that passed SPF checks but has a spoofed 5322.From sender address. 

This is no fun for anybody, because it's a huge security hole in the phishing and spam realm.

The 5321 and 5322 numbers are highly boring geek-speak referring to the Request For Comments (RFC) on these provided by the Internet Engineering Task Force (IETF). If you'd like to tiptoe through the tulips of Internet pain, I leave them here for your reference:

<https://datatracker.ietf.org/doc/html/rfc5321>
<https://datatracker.ietf.org/doc/html/rfc5322>

Let me give you an example, and in this one the sender addresses are as follows:

Mail from address (5321.MailFrom): badguy@phishing.nastyplace.com

From address (5322.From): security@yourhappydomain.com

If SPF is configured, then the receiving server does a check against the Mail from address badguy@phishing.nastyplace.com. If the message came from a valid source for the domain phishing.nastyplace.com, then the SPF check passes. By "valid source", that means that the originating server exists, not that it's actually yours. See the problem already in progress? Since the email client only displays the From address, the user sees this message came from security@yourhappydomain.com. With SPF alone, the validity of yourhappydomain.com was never authenticated.

When we use DMARC, the receiving server will additionally perform a check against the From address. In the example above, if there's a DMARC TXT record in place for yourhappydomain.com, then the check against the From address fails because the "Mail from" is a mismatch against the legitimate domain.

That deceptive email is bounced or quarantined, and the crisis you never knew you had is diverted.

Once we've set up SPF and DMARC, we need to set up DKIM. 

DKIM lets you add a digital signature to email messages in the message header that certify the message in fact originated from your mail server and nowhere else. It's a security measure for your domain specifically, similar to a house or car key, except more cryptographic and messier looking. If you don't set up DKIM and instead allow the mail server to use their default DKIM configuration for your domain, DMARC may fail. This failure can happen because the default DKIM configuration will use the standard nexcess.net domain as the 5321.MailFrom address, not your specific domain. This creates a mismatch between the 5321.MailFrom and the 5322.From addresses in all the email sent from your domain.

There are other hairs we could split here that include third-party services, but this is meant to provide an overview of these three records and why they must be intentionally and correctly implemented.

If you need to verify or adjust these settings for your own domain or another, I recommend using the DMARCly tool (<https://dmarcly.com/tools/>), which makes it easy to generate and check your settings. By doing so, you'll be able to protect your professional image and maintain a positive reputation.

After all, no one wants to end up in the spam folder - except, perhaps, spammers! We'll leave it to them to fend for themselves.

## Example of a proper DMARC record:

PROPER DMARC
The current DMARC for domain.com is:
```
"v=DMARC1; p=quarantine; rua=mailto:postmaster@domain.com; sp=none; aspf=s; adkim=s; fo=1;"
```

```p=quarantine```: This specifies the policy for emails that fail DMARC authentication. In this case, the policy is to quarantine (i.e., mark as spam) emails that fail authentication.

```rua=mailto:postmaster@domain.com```: This specifies the email address to which aggregate reports should be sent. In this case, the reports will be sent to postmaster@domain.com.

```sp=none```: This specifies the policy for subdomains. In this case, the policy is to ignore any subdomains that are not explicitly listed in the DMARC record.

```aspf=s; adkim=s;``` These specify the alignment methods for SPF and DKIM.

```fo=1```: This specifies the forensic OAuth reporting mode. In this case, it is set to 1, which means that forensic reports will be sent to the email address specified in the rua parameter.

## TL;DR
 
Emails that fail DMARC authentication should be quarantined, reports should be sent to ```postmaster@domain.com```.
The policy for subdomains is to ignore any subdomains that are not explicitly listed, and checks are aligned at subdomain level.

Setting alignment checks at the root level can be more restrictive, but it may also result in the rejection of some legitimate emails that do not meet the specified alignment criteria, potentially causing issues with email delivery.

!!! warning 
    We don't want to deal with directly configuring DMARC and the potential repercussions of our actions.
    It is typically the responsibility of the client and their developers to review and set up the DMARC policy.

Our job is to set up the most generic/default DMARC configuration:

```
Value: "v=DMARC1; p=none;"
```

*[SPF]: Sender Policy Framework
*[DKIM]: DomainKeys Identified Mail
*[DMARC]: Domain-based Message Authentication, Reporting, and Conformance