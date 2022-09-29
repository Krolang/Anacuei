Getting containers Running in Docker is incredibly easy. Make sure that you already [[Docker#Installation|set up Docker]] before following this part of the installation guide.

This guide will cover how to set up Pi-Hole and OpenWrt on a bridge network within Docker, as well as how to automate their deployment using Docker-Compose

# Pi-Hole
## 1. Disable Ubuntu's Internal DNS
Because we will be using the Pi-Hole as a DNS server, we need to disable Ubuntu's built-in Caching DNS Resolver; otherwise it will overwrite our own settings. This is done with the following commands:

```
sudo systemctl stop systemd-resolved.service
sudo systemctl disable systemd-resolved.service
```

## 2. Set up a Temporary DNS Server
However, you may now notice that you cannot resolve DNS queries at all. To fix this, you'll need to edit Ubuntu's `resolv.conf` file to use a different [[DNS#Upstream DNS|Upstream Provider]], at least for now while we get Pi-Hole set up.

Edit the file with nano by running
```
sudo nano /etc/resolv.conf
```
then find the line that says "nameserver" and change the IP to an upstream provider.

>[!WARNING]
>When I did this, everything was fine until I restarted the device, at which point I could not connect to the internet in any way. Until a fix is found for this, keep the system resolver enabled, and -- if it even would work -- make sure to set up the Pi-Hole as a DHCP server.

## 3. Pull the official Pi-Hole Docker image
```
sudo docker pull pihole/pihole
```

## 4. Create a Network Interface
This allows Docker containers to communicate with each other over a User-defined bridge network without needing to expose unnecessary ports to the internet.
```
sudo docker network create --driver bridge AnacInt
```
(AnacInt will be the name of the interface, but you can use anything here.)

## 5. Create two storage volumes
Docker containers dont come with persistent storage by default, so we need to give them some from the host. to do this, we need to create volumes on the Host (in this case, Ubuntu) for the Docker image to use for its configs, softwares, and scripts.
```
sudo docker volume create etc-pihole
sudo docker volume create etc-dnsmasq.d
```
The first volume stores the config files for Pi-Hole itself, while the second stores information relating to one of the tools Pi-Hole uses.

## 6. Make the container
>[!INFO]
>Typing a backslash at the end of a line will cause a new line to be entered. you could seperate the options with a space, but that gets very messy.
```COPY
sudo docker run -idt \
	--name EvDNS \
	--hostname anacdns \
	--network AnacInt \
	-p 53:53/tcp -p 53:53/udp \
	-p 80:80 -p 443:443 \
	-p 8080:8080 \
	--mount source=etc-pihole,target=/etc/pihole \
	--mount source=etc-dnsmasq.d,target=/etc/dnsmasq.d \
	--restart always \
	--
	pihole/pihole
```
This is a lot of syntax, so it may be helpful to know what each thing means.
- *sudo docker run* is telling docker to start running a container with the options **-i**, **-t**, and **-d**. 
	- `-i` makes the container interactive.
	- `-t` allocates something called a "pseudo-TTY"
	- `-d` tells docker to run the container in the background.
- The `--name`, `--hostname`, and `--network` options tell the container what name to display (to make it easier for us to work with it), what other computers should refer to it as, and what network it should be deployed on.
- The `-p` option maps container ports to ports on the host. if we didn't do this, we wouldn't be able to connect to anything.
	- Port 53 is used for DNS requests. It appears twice because DNS sometimes uses a protocol called TCP, but also might use one called UDP.
	- Port 80 and 443 allow us to receive HTTP and HTTPS requests. these ports allow you to access the Admin panel.
	- Port 8080 also uses HTTP, and can run a webserver as a non-root user. This (probably) transmits the block statistics to the client when they view the dashboard (I don't really know, but they're important for configuring Pi-Hole later).
- The `--mount` option tells Docker what parts of the host Filesystem to use for which volumes.
- `--restart` controls the behavior of the container. We configured it to always restart unless we manually stop it, and to remember if it should be running even after a restart of the host machine

## 7. Get the IP address of the Container
```
sudo docker inspect AnacInt
```
You should see some IP addresses. Find the one that says "IPv4Address" and write it down.
>[!NOTE]
>depending on how IP Assignment works within docker and whether OpenWRT overrides that, you may need to assign a static IP using the instructions in the  [[OpenWrt#2 DNS|GL-iNet setup]] section.

## 8. Set a New Password
Enter the Container's Command Line by running
```
sudo docker exec -it EvDNS bash
```
Once you're in, change the Pi-Hole admin password with this command:
```
pihole -a -p
```

## 9. Continue with configuration
Open up your web browser, and type the IP from earlier in the address bar, followed by `/admin`. Login with your password, and continue with configuration as normal, following the steps in [[Pi-Hole#2 Configuring Pi-Hole|Pi-Hole Configuration]] from section 2 onwards.

Once Pi-Hole Configuration is done, you can exit the container without stopping it simply by typing:
```
exit
```

To speed up this process, we could make a Dockerfile that runs all the necessary configuration upon startup. But unless you actually stop the docker service, configuration should persist.

## 10. Implement [[DNS over Tor]] on the Pi-Hole

# OpenWrt
Normally, People run Docker on OpenWrt, because there are apparently some complications. However, OpenWrt can only run docker if it is installed on x86 or server infrastructure. Given that most people don't have an entire server just lying around We need to containerize the router as well.

Unfortunately, we can't just use the same image that we used for the Hardware setup; it would be absurdly large and inefficient if it were used as a docker container directly. It can be done **as a last resort**, but you may see a drop in performance.

Fortunately, there is a particularly bright individual on the internet who goes by the pseudonym *oofnikj* who made a GitHub Repository for the sole purpose of running OpenWrt within Docker on a Raspberry Pi. I will link their documentation of the Pi installation, but there are a few things that should be done before following those instructions:
## 1. Download the Repository
The repository can be downloaded as a zip file from https://github.com/oofnikj/docker-openwrt by clicking the green code button's dropdown, then selecting the zip file download. Extract the contents of the .zip to the `home` directory on your Pi.

If you are using the Command line interface or an OS without a desktop interface, you're not going to have a file browser. To get around this, you'll need to copy (or write down) the URL in the dropdown instead of the .zip on another computer. Then in the command line, run `ch /` to make sure that you're in the home directory. Once there, run
```
mkdir {name of the folder you'll place the repository in}
git clone {Repository URL}
```

## 2. Make sure required tools are installed
A package called `make` is required to build the Docker Image from the repository, and other packages like `fakeroot` and `squashfs-tools` are needed to ensure it runs smoothly.

To check whether `make` is installed, open your command line and run:
```
make -version
```

If `make` isn't installed, you'll see an error like this:
```
bash: /usr/bin/make: No such file or directory`
```
If this is the case, you'll have to install `make` with the following command:
```
sudo apt install make
```

After this, run the commands
```
sudo apt install fakeroot
sudo apt install sqashfs-tools
```
to install the other two packages.

## 3. Build The image
navigate into the Extracted directory with
```
cd /openwrt-docker-master
```

Now follow the steps detailed in [[Oofnikj#Build|Oofnikj's Build Documentation]]

>[!NOTE]
>Rename `openwrt.conf.example` to `openwrt.conf`, otherwise the build command will fail.
>Also, change the `ARCH` entry in the config file to `armvirt64` (or whatever the OS you're deploying on uses).

## 4. Set up tor.
To do this, you'll want to either [[Tor#Implementing tor in an OpenWrt Container Manual|configure tor manually]] or [[Tor#Implementing tor in an OpenWrt Container Automated|Alter OpenWrt's Dockerfile]] to automatically set up tor for you.


>[!WARNING]
>REMEMBER TO SET YOUR PASSWORDS IN THE CONFIG FILES.