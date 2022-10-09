>[!WARNING]
>Setting your DNS to an obscure provider may provide better data security, but it also has a high likelihood of making you easily identifiable. The actions you take to secure your DNS information depends greatly upon your threat model. For example, Local law enforcement likely don't have the capacity to trace a client's DNS packets from a website, but an authoritarian government that pushes censorship might.

DNS stands for Domain Name System. It is what turns a URL to an IP address that your computer and router can use. It is for this reason that DNS Security is extremely important to get right.

## Upstream DNS
No matter what, your DNS queries will have to go to another DNS server, many of which keep logs. but there are some that don't. In my research, I've found a site called [OpenNic](https://servers.opennic.org/), which lists community-hosted DNS servers, many of which have Security and anonymity as their highest priority. There are two in particular that caught my eye:

### AnonDNS
They don't keep logs, and they support a variety of encryption options such as DoT (DNS over TLS) and DoH (DNS over HTTPS). I couldn't get DoH working, but I'll include the info for it in case you can.
>[!AnonDNS]
DoH: https://dns.anondns.org (port 443)
DoT: 54.13.59.77 (port 853)

### ParrotSec DNS
This DNS provider keeps a small log backlog, but the only way for an adversary to be able to read those logs would be for them to exploit a backdoor in the infrastructure of a literal Cybersecurity Firm.
>[!ParrotSec DNS]
51.178.92.105

## Complications
In my testing I had discovered that my ISP was hijacking my DNS requests to use their own servers, and attempting to contact a ton of ports on my computer. I soon discovered that my ISP is well-known for DNS hijacking. Currently, I know of two ways that this can be circumvented:

1. Route DNS over Tor, to prevent them from being able to do anything with it.
2. Put the Modem into bridge mode, while also setting the ANACUEI router up as an authoritative DHCP server.

Make sure your web browser doesn't also have its own DNS set.

## Things to Avoid When choosing an Upstream Provider:
- Avoid any Upstream servers provided or hosted by large companies like Google, Amazon, or Cloudflare (which is owned by google), as they are prime targets for law enforcement seeking to access your data.
- Avoid any DNS provided by your ISP; They also have been found to work with Governments. 
	- THIS IS ESPECIALLY TRUE FOR IRANIAN PROTESTERS, AS MOST IF NOT ALL ISPs ARE EITHER GOVERNMENT OWNED, OR ARE PRESSURED INTO DISCLOSURE. FOR ANY PROTESTERS IN IRAN, YOU WILL NEED TO ROUTE YOUR DNS THROUGH THE TOR NETWORK. REFER TO [[tor_config.txt]] FOR INSTRUCTIONS.
- Choose Open Source when you can, as it forces whoever is running the service to be transparent about what they implement.
- Check if they keep logs. If they don't say, assume they do.
- ALWAYS check if your DNS is being overwritten. This could be the fault of your modem, your router, your browser, or even your own damn computer.
- **ALWAYS Evaluate the service according to your [[THREAT MODEL]]!**