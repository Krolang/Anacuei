## Model
I used the GL-AR300M16 Mini Router, as it was very inexpensive, very compact, and had OpenWrt (An open-source Linux Router Operating system) installed. You will later install an alternate version with tor enabled, but the original OS is needed to do so.

## Firmware
The Firmware I used that came with tor preinstalled can be found here: https://openwrt.org/toh/views/toh_fwdownload?dataflt%5BBrand*%7E%5D=GL&dataflt%5BModel*%7E%5D=GL-AR300M&dataflt%5BVersions*%7E%5D=v1.4.0. The firmware gets downloaded to your computer, then *locally* installed through the router's Web Interface.

## Setup
1. The first thing you're going to want to do is access the Admin panel. You do this by plugging the router into your modem (see the router's instruction manual for details), connecting to the WIFI network that shows up, then typing the Router's default IP address (192.168.8.1) into your browser's address bar. When prompted, log in with the credentials on the bottom of the unit.
2. In the Web Interface, navigate to the UPGRADE tab. Select "Local Upgrade", then upload the [[GL-iNet Router#Firmware|tor-enabled firmware]] you downloaded earlier.
3. Log into the router again if needed, and set a new Admin Password. Choose something extra secure, as that password protects all of your router's settings.
4. Power the pi, and connect it to the router's LAN port with an ethernet cable, then check the CLIENTS tab in the Web interface. Take note of the IP address and the MAC Address. If the Pi isn't showing up, you'll need to connect a monitor and keyboard to the pi to [[Raspberry Pi#Fixing Network interfaces|troubleshoot]]
5. Navigate to MORE SETTINGS -> LAN IP -> *Static IP Address Binding*. Input the IP and MAC addresses you recorded into their respective Fields, and click add. This ensures that your [[DNS]] server will always have the same IP address, and it's also a required step in setting up Pi-Hole.
## Configuration
### 1. Wi-Fi Password
Set the name and password for the Wi-Fi (**MAKE IT DIFFERENT THAN THE ADMIN PASSWORD**). Leave guest Wi-Fi off.
### 2. DNS
1. Navigate to MORE SETTIINGS -> Custom DNS server
2. Enable *DNS rebinding attack Protection* if it isn't already. 
3. Enable *Overriding DNS settings for all clients*.
4. Manually Input custom DNS servers. One of these should be the Raspberry Pi. the other should be set to the IP of a secure, privacy-oriented [[DNS#Upstream DNS|Upstream DNS server]].
### 3. Buttons
This is a matter of personal preference, but I recommend making the switch on the side of the unit toggle the tor connection. Your router should contain the necessary software packages to facilitate this. If not, you may not have installed the tor image. Check the [[GL-iNet Router#Firmware|Router Firmware]] section for steps on how to do this.
### 4. Firewall
Configure your Firewall to only allow the web interface to be accessed from within the network (if it isn't already configured like that). See the Firewall documentation for more details: https://docs.gl-inet.com/en/3/tutorials/scp/.