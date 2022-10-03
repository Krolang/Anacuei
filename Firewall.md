>[!WARNING]
>This section is a Work in progress. I will document the firewall when i learn more about it. In the meantime, go through the config directories mentioned in [[tor_config.txt]] and read the comments to see what would be advisable for your threat model.
>For now, refer to the official OpenWrt documentation for firewall rules [here](https://openwrt.org/docs/guide-user/firewall/firewall_configuration)

# Principles
To start securing your network, we want to do a bit of isolation.
There are three main groups of network clients you need to think of when configuring the firewall: Local Area Network (LAN), Guest (people who don't need/shouldn't have access to the Router's Dashboard), and an IoT group for all your smart devices.
#### LAN:
This is the Firewall Zone that contains your router and you. It should be able to access the WAN Interface, but the WAN Interface shouldn't be able to access the LAN. This rule prevents you from attackers.

#### GUEST:
This is for everyone else. They should be able to access the WAN, but nothing else. Like LAN, the WAN shouldn't be able to access this.

#### IoT:
Ideally, you shouldn't have any always-listening devices phoning home to their creators. But if there are Wi-Fi enabled devices that don't need to access the internet (e.g. a robot vacuum) then they should be put into this zone. Their web interfaces need to be accessible from the LAN Zone.

## Setting up the Zones
### Hardware Build
