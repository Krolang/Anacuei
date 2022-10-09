>[!IMPORTANT]
>This part of the guide assumes that you have followed the  [[Containers#OpenWrt|setting up OpenWrt in Docker]]

>[!IMPORTANT]
>For people in Iran who need to use tor, I recommend reading their documentation thoroughly, as I am not too familiar with how your Country handles tor. Unless you know your ISP will not report you for simply using tor, refrain from using it, or find someone who knows how to secure it. It is better you play things safe than be sorry later.
>Here is a source I found on how to set up tor bridges: https://www.reddit.com/r/TOR/comments/xokbrb/iran_circumventing_censorship_with_tor_copy_from/

# What is tor?
Tor (The Onion Router) is a decentralized network designed to protect activists and whistleblowers, thwart surveillance and spying, and infuriate the NSA and FBI.

It is one of the best tools to preserve your privacy; the other tools use tor by default or by definition. This section of the guide will show you how to properly set up tor on your router.

# What NOT to do
Tor keeps you anonymous, so long as you don't do anything that could lead to correlation and identification. This includes:
- Browsing non-HTTPS pages
- Using search engines that collect user data
- Sharing personal information
- Using personal social accounts.
- Authenticating services from your cellphone
- Using an old version of Tor

## Implementing tor in OpenWrt (Manual config)
Manual configuration of tor on your router is probably the easiest, but more time-consuming, methods of configuration. Once you've built the image, Follow the instructions below to set up tor manually.

1. Run `docker exec -it [imagename]`
2. On another computer, open [[tor_config.txt]], and run the commands detailed in the file.

## Implementing tor in an OpenWrt Container (Automated)
This method is more involved, but it saves you time if your instance of docker stops unexpectedly, as it prevents your tor configs from being erased, as they are preconfigured at build time.

(although, your tor configurations disappearing upon a sudden shutdown might not be such a bad thing, especially if you don't want to let on the fact that you've been using it.)

>[!NOTICE]
>This section is a WIP. I am still trying to get this to work. Use the other method for now.
### 1. Get the Configuration Script
Assuming you have already navigated to your working directory, run `ls` to view the files in the directory. You should see one called `dockerfile`. This is the file that builds and configures the Docker image. to configure tor, we need to add some of our own code.


We want to use a series of commands from the OpenWrt wiki to configure the tor client. To do this we need to put the instructions into a shell file, that the dockerfile will then run.

I've included a copy of `torconf.sh` that you can transfer to the Pi using a USB. To move the file using the command line interface, you'll need to use the `cd` command to change move to the USB, then run 
```
mv torconf.sh [path/to/repository]
```
where the last bit is the path to the [[Containers#1 Download the Repository|repository]] you pulled from GitHub.

### 2. Tell the Dockerfile to use the script
Navigate back to the repository.

`cd /directory_name/openwrt-docker-master`

Open the Dockerfile with a text editor (`nano` is usually the go-to for Command-Line Users). it should look like this (but probably with different spacing):

```
FROM scratch
ADD rootfs.tar.gz /
RUN mkdir -p /var/lock

RUN opkg remove --force-depends \
dnsmasq* \
wpad* \
iw* && \
opkg update && \
opkg install luci \
wpad-wolfssl \
iw-full \
ip-full \
kmod-mac80211 \
dnsmasq-full \
iptables-mod-checksum

RUN opkg list-upgradable | awk '{print $1}' | xargs opkg upgrade || true

RUN echo "iptables -A POSTROUTING -t mangle -p udp --dport 68 -j CHECKSUM --checksum-fill" >> /etc/firewall.user

RUN sed -i '/^exit 0/i cat \/tmp\/resolv.conf > \/etc\/resolv.conf' /etc/rc.local

ARG ts
ARG version
LABEL org.opencontainers.image.created=$ts
LABEL org.opencontainers.image.version=$version
LABEL org.opencontainers.image.source=https://github.com/oofnikj/docker-openwrt

CMD [ "/sbin/init" ]
```

Move your cursor to the end of the first `RUN` command, type a space, followed by a BACKSLASH. Hit enter, then insert the following:
```
tor \
iptables-mod-extras \
kmod-ipt-nat6
```

This will install some packages we need to get the tor client working.

After the last `RUN` command, we'll need to insert the following:
```
COPY torconf.sh /etc/tor
RUN sh /etc/tor/torconf.sh
```

The `COPY` command tells docker to put our script in the container's filesystem at the path `/etc/tor/torconf.sh`, and the `RUN` command tells the docker container to run the setup script.

>[!idea]
>list torconf.sh in .dockerignore?
