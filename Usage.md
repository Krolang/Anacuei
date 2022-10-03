# Stay Safe
Even the most secure of systems can be compromised by human error. Adversaries know this; that’s why there’s a whole branch of hacking called *Social Engineering*; If they can get you to unlock the door for them, why bother breaking in?
With that in mind, here are a few pieces of advice to help you stay safe:

## Usage Guidelines
- When browsing the internet for the various parts and resources you'll need for this project, you must assume that your government can access those records. Use anonymizing services like tor and TAILS OS to view the internet resources.
- Avoid using a cellular device to connect to this router, as your government may have Stingrays, which work to intercept cellular communications. Once they Identify the device connected to the router as yours, they can connect your identity to the router, and to any websites, usernames, accounts, etc. Also, Cellphones are very easy to geolocate, so they'll have all the aforementioned information *and* your location data. If you live in the US, Check the website of the American Civil Liberties Union for a map of which states use these devices (https://www.aclu.org/issues/privacy-technology/surveillance-technologies/stingray-tracking-devices-whos-got-them). If you don't live in the US, There are likely resources your country has too.
- Make sure your browser is configured to resist fingerprinting, which allows adversaries, Analytics, and attackers to uniquely identify your browser without saving a single identifier on your computer. And if they can identify your device, they can identify *you*.
- In the case of repressive governments, your ISP is the single biggest threat to your safety, as all your internet traffic goes through them. **Route both HTTP/HTTPS requests and DNS queries through tor to protect yourself**.
- Don't upload any revealing information, and that includes Metadata. Use a tool like Exiftool to check potential uploads for identifying metadata. This is more important than you think; *==people have been killed over metadata==*.
- ***Until you can prove otherwise, operate under the assumption that your actions are being monitored***.

### VPN Guidelines
**If you configure your device to use a VPN like RISEUP, it is imperative that you pay special attention to the following:**
	- When your internet traffic leaves the VPN exit point, it becomes just normal internet traffic. The tor project website says that it may actually be advantageous in certain circumstances to use a VPN over tor. However doing so poses a significant risk of data leakage and deanonymization unless you absolutely know how to configure it. for more information, check the Tor Wiki: https://gitlab.torproject.org/legacy/trac/-/wikis/doc/TorPlusVPN.
	- DO NOT USE THE ROUTER FOR TORRENTING. RISEUP is not designed for torrenting. Using RISEUP VPN to torrent WILL land you in trouble, and may leak information.