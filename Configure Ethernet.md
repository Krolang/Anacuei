# Configuring a static IP on the eth0 interface.
## 1. Review Current network settings.
from the command prompt on Raspbian (or LXTerminal) type:
`ifconfig`
This will display the current network settings

## 2. Backup the current network Config File.
To make a copy of the network config file run the following:
`sudo cp /etc/dhcpcd.conf /etc/dhcpdcp.backup`
this will create a copy of the network file in case something goes wrong.

## 3. Modify the network settings.
Now you need to edit the config file. to run raspbian's text editor (nano) on the file, run the command
`sudo nano /etc/dhcpcd.conf`
Use the arrow keys to navigate to the top of the file, where you will insert four lines to give the ethernet interface a static IP. The lines you need to insert are as follows:

`interface eth0`
`static ip_address=`*the IP you told the router to give to the pi earlier*`/24`
`static routers=`*the Router's gateway IP address (same as the IP in the first line, but it ends in .1)*
`static domain_name_servers=`*if available, set this to the IP of an upstream DNS provider. This may get overwritten when you set up Pi-Hole*

Now save the file with CTRL+X

## 4. Restart the Pi
`sudo reboot`

## 5. Test the network
Connect the Pi to the router with ethernet, then attempt to ping another computer that is also connected to the router but over its WiFi.

`ping` *the computer's IP*