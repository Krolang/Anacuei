# ANACUEI
*Anonymous Networking And Communication Utilizing Evasive Infrastructure*

---
It is well established that we live in an era of unprecedented digital surveillance. It is a threat to the liberties of all citizens. So when the Supreme Court of the United States reversed Roe v. Wade, I knew that many people would be prosecuted or killed because of it, and that the problem would be made worse by mass surveillance. I realized that my knowledge of internet privacy put me in a position to dampen the impact of the ruling, and I knew that the best way to do so would be to provide a way for others to seek help and lifesaving treatment without fear of their data being used against them. I decided to create a privacy machine, the ANACUEI Router, to help women seeking abortions and to help people remain anonymous under a repressive regime.

This point was driven even further home when, during the development of the project, the Iranian Government shut off most of the country's internet to conceal human rights violations. If you are reading this, distribute this material. Translate it. Get it to the people who need it.

Privacy is your first line of defense against an entity that is trying to infringe upon your rights. Everyone deserves the right to privacy, to safety. All the components utilized are open source and audited for security, privacy, and peace of mind.

>[!WARNING]
A DISCLAIMER:
The project is still in its infancy, so **I cannot guarantee that it is free of any vulnerabilities.** However, I am working to get it audited as soon as I can, and I am working to implement better security as fast as I can.

Later iterations will allow one to utilize [[Docker]] to consolidate the system into one device while preserving security, or even utilize some new technology to bypass ISP surveillance. For now, the hardware build should be enough to foil any state law enforcement that's prowling around looking for people seeking an abortion. And for the brave Iranian Protesters, I am constantly updating this documentation as I learn more about what your government is capable of.

>[!IMPOERANT]
> [[Usage|USAGE GUIDELINES]]

## Hardware
- [[OpenWrt|GL-iNet Router]] or a [[Raspberry Pi]]. I used the GL-iNet because I only had one Pi, which didn't have a built in Wi-Fi chip (If you use a Pi for this part, you NEED one with a built in Wi-Fi chip).
>[!UPDATE]
>Setting up OpenWrt on a router with a built in Wi-Fi chip is no easier.
- [[Raspberry Pi]]. This is in addition to the one you may have used for the router, as this one will be used as a[[ DNS]] Server running [[Pi-Hole]]. Newer Versions are always preferable, but my old Pi 2 B v1.1 worked in the end (even though it has been discontinued).
- Ethernet Cables. You'll need two of these: one connecting the [[DNS]] to the router's LAN port, and the other connecting to the modem from the Router's WAN port.
- 8GB microSD card. This will usually come with the pi. It will contain the Raspberry Pi's Operating system.
NOTE: While I haven't completed it, It may be possible to run everything on a singular [[Raspberry Pi]] using [[Docker]]. I am in the process of doing this, and will update the documentation accordingly.

## Building the System 
### From Hardware (GL-iNet + RPi)
1. [[Raspberry Pi#Installing the Operating system|Install an Operating system on the Raspberry Pi]]
2. [[OpenWrt#Setup|Set up and Configure the Router]]
3. [[Pi-Hole#Installing PiHole|Install and configure Pi-Hole]]
4. [[Testing|Make sure that everything works]]
5. [[Firewall#Hardware Build|Configure the firewall]]

### Within Docker (Raspberry Pi 4 B) (Work In Progress, **Not Recommended**)
>[!WARNING]
>This method of building the system is HIGHLY experimental. I have not gotten it to work. I do not recommend this method. However, I have included the documentation in its unfinished state such that anyone who *can* get it working has something to jump off of.

1. [[Raspberry Pi#Installing the Operating system|Install the Host OS onto the Pi]] (follow the instructions that came with the pi for this).
	- Docker can only be installed on Raspbian through a convenience script, so I recommend choosing a different Linux distro so you can install from the repositories. Most of the tutorials recommended Fedora Desktop 33, but I used Ubuntu and just didn't install the graphical interface for docker. This ended up working very well.
2. [[Docker#Installation|Install Docker on the Host OS]]
	- These instructions might vary depending on your chosen distro. This documentation uses Ubuntu as the Host OS.
3. [[Containers|Set up the Containers]]
4. Configure the Virtual Network

>[!NOTE]
OpenWrt supports OpenVPN and Wireguard VPN. However, I do not suggest Implementing A VPN on this system, even one as private as RISEUP VPN. For details see the [[Usage#VPN Guidelines|VPN warning]].

## Additional Reading:
- Using LINDDUN to make a [[THREAT MODEL]]
	- https://www.linddun.org/linddun-threat-catalog
- An Activist's Guide to INFOSEC (Zine)
	- [[activist-info-sec-SCREEN.pdf]] (Please Translate if you can)
