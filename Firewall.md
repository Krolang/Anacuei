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
#### OpenWrt
##### Firewall Zones
Make sure you are in the `LuCi` advanced config interface.

1. Head to NETWORK->FIREWALL
2. Find the row with the blue box that says "Guest"
3. Change the Input dropdown from "drop" to "reject"
5. Click the Blue "Save and Exit button"

To be honest, the GL-iNet config page set up most of the firewall for you.

##### MAC Filtering
This provides an extra layer of security, ensuring that only specific devices can connect to the main LAN.

1. Head to NETWORK->WIRELESS
2. Find the entry that corresponds to the Main Wi-Fi network. Click the Blue EDIT button.
3. Go to the MAC Filter tab.
4. In the dropdown, select "allow listed only"
5. A new dropdown will appear; select only necessary devices, such as the router itself, the Raspberry Pi, and the devices of your family (make sure they are secured as well.)
6. Click the green "Save" button, then the blue "Save and Apply"

From here, continue with the [[OpenWrt#Configuration|Configuration]].