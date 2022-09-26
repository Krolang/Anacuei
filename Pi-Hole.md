# Installing PiHole
## 1. Configure the Network Interfaces
Before installing PiHole, you'll need to set a static IP address for the Ethernet Interface. To do so, you'll need to  [[Configure Ethernet|edit the network config file]]
Initially, I faced quite a bit of difficulty setting Pi-Hole up either due to network interfaces not behaving or due to missing repositories. However, I ended up getting Pi-Hole working by first following the [[Pi-Hole Setup Guide]],
then proceeding with the instructions found in [[Pi-Hole#Configuring PiHole|"Configuring Pi-Hole"]]

If all else fails, my problem was solved by unplugging the pi from its power while in the middle of a direct bash install, then plugging it back in and doing the same install again. I do not know why this worked.

Once you install Pi-Hole, The Installer will guide you in setting up the Pi-Hole, and even gives you the login details to the web dashboard at the end.

## 2. Configuring Pi-Hole
1. Log into the web dashboard, and set a new Admin password (for obvious reasons).
2. Find a domain blacklist for the Pi-Hole. A blacklist comes preinstalled, but it isn't very suitable for our purposes. It may be tempting to use Regular Expressions to blacklist everything, only temporarily allowing domains through when you search for them. However, most modern websites use things called Content Delivery Networks (CDNs), and while blocking those would keep them from tracking you ,doing so would tend to break the entire page. to get around this, use a Firefox extension like **decentraleyes**.
3. Depending on your Internet Service Provider, you may not have access to certain modem settings needed to get Pi-Hole working. If this is the case, you need to set up the Pi-Hole as a DHCP server as well. Documentation for this process can be found here: https://discourse.pi-hole.net/t/how-do-i-use-pi-holes-built-in-dhcp-server-and-why-would-i-want-to/3026
4. To allow your router to assign IP addresses to the devices on its network, you'll need to put your MODEM in bridge mode, effectively forcing it to delegate DNS lookup, IP assigning, and routing to your router.
5. You may have done so in the setup process, but I've found that the anonymity settings don't persist after installation is completed. Check those settings, and **set the DNS resolver Privacy settings to "Anonymous."** This prevents the Pi from saving any logs that can be used against you, provided that any adversary targeting your system is unable to monitor your queries in real time (which is a compelling reason to [[DNS#AnonDNS|encrypt your DNS queries]] using DNS over TLS). Only change this setting if you need to debug something, and change it back as soon as you're done.

## 3. Turn the Pi-Hole into a DNS server.
You'll want to be able to hard-block any tracking domains that could collect any data from your browsing. To do so, follow [[Unbound Recursive DNS|this guide]].

## 4. Refine Security
Additional security measures can be found in the [[Pi-Hole Setup Guide]]. I highly recommend implementing them.
