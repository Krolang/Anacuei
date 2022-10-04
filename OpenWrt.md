## Building with an Off-The-Shelf Router
I used the GL-AR300M16 Mini Router, as it was very inexpensive, very compact, and had OpenWrt (An open-source Linux Router Operating system) installed. You will later install an alternate version with tor enabled, but the original OS is needed to do so.

## Firmware
The GL-iNet comes with OpenWrt preinstalled, but we will need to configure it to use tor.

## Setup
1. The first thing you're going to want to do is access the Admin panel. You do this by plugging the router into your modem (see the router's instruction manual for details), connecting to the WIFI network that shows up, then typing the Router's default IP address (192.168.8.1) into your browser's address bar. When prompted, log in with the credentials on the bottom of the unit.
2. In the Web Interface, navigate to the UPGRADE tab. Select "Local Upgrade", then update the firmware to its latest version.
3. Log into the router again if needed, and set a new Admin Password. Choose something extra secure, as that password protects all of your router's settings.
4. Power the pi, and connect it to the router's LAN port with an ethernet cable, then check the CLIENTS tab in the Web interface. Take note of the IP address and the MAC Address. If the Pi isn't showing up, you'll need to connect a monitor and keyboard to the pi to [[Raspberry Pi#Fixing Network interfaces|troubleshoot]].
5. Navigate to MORE SETTINGS -> LAN IP -> *Static IP Address Binding*. Input the IP and MAC addresses you recorded into their respective Fields, and click add. This ensures that your [[DNS]] server will always have the same IP address, and it's also a required step in setting up Pi-Hole.
6. Head to the Wireless tab and change Wi-Fi security to **WPA3-SAE**. This is a vital step in securing your network, as **WPA2** has many known vulnerabilities.
7. Add a Guest network for visitors. Allowing them to connect to your LAN network is a major security risk.
8. While you've been following the above steps, enough time should have passed for the repository list to update. Install `LuCi` in the Advanced settings tab. If you get an error saying `wget` is not installed, then try updating the package list in the applications tab. You may need to wait for repository lists to update again. **THIS SHOULD BE DONE BEFORE SETTING ANY CUSTOM DNS, Otherwise the errors will not end.**
9. Once `LuCi` is installed, you can use it to [[Firewall#Hardware Build|Configure the firewall]].

## Configuration
Follow these steps in order, as `wget` won't work if you have custom DNS set.

### 1. Wi-Fi Password
Set the name and password for both the LAN and Guest Wi-Fi (**MAKE IT DIFFERENT THAN THE ADMIN PASSWORD**). 

### 2. TOR
You'll need a Terminal for this.

SSH into the router by running

`ssh root@`*ip*`

Enter the admin password when prompted, then run the [[tor_config.txt|Tor configuration commands]], ignoring any lines in the file with a hashtag, as those are coments.

### 3. DNS
1. Navigate to MORE SETTIINGS -> Custom DNS server
2. Enable *DNS rebinding attack Protection* if it isn't already. 
3. Enable *Overriding DNS settings for all clients*.
4. Manually Input custom DNS servers. One of these should be the Raspberry Pi. the other should be set to the IP of a secure, privacy-oriented [[DNS#Upstream DNS|Upstream DNS server]].



### 4. Buttons
This is a matter of personal preference, but I recommend making the switch on the side of the unit toggle the tor connection. Your router should contain the necessary software packages to facilitate this. If not, you may not have installed the tor image. Check the [[OpenWrt#Firmware|Router Firmware]] section for steps on how to do this.

