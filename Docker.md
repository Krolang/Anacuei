Docker is an application that streamlines app deployment and reduces the ammount of workload and hardware you need to set up infrastructure. Instead of running different programs on Operating systems running in virtual machines on a hypervisor, various Docker images run in the Docker environment on a host OS.

It comes with security features that will help us secure our router, like internal networking to minimize the amount of ports open to the web.

We will use Docker to run the firmware for the router, DNS server, and the Domain Blocking, allowing our entire network infrastructure to be contained within a single computer. In theory, we could use Docker to implement any sort of infrastructure we want; we can configure a VPN, an Encrypted Mail Server, Proxies, an amnesic Log Server, or even a tor relay, all with a couple of commands.

I plan to publish a Dockerfile for those less technologically inclined, but this guide should contain everything you need to build the Dockerfile yourself.

# Installation
For this Project, I installed Docker from the Command line on Ubuntu. I've been told it is smoother on Fedora Linux, but I knew my way around Ubuntu well enough, and I planned to add Onionshare to the host OS, to allow file transfer over the tor network. This didn't end up working, so I plan to put it into another container.

I did the install on a desktop version of the 64-bit OS (if I had used a 32 bit OS, i wouldn't have been able to run Docker containers that used x64 kernels), so that I could use a Graphical Interface to troubleshoot things. However, since most of the installation steps were done via the command line anyway, the desktop environment is unnecessary, but certainly convenient.

>[!NOTE]
>OnionShare doesn't seem to be working on Ubuntu in its latest release. However, it can also be installed on Debian if one installs the snap package installer. Creating a docker container running a Debian machine in the background would allow you to host & receive files, chat, and host hidden sites over the tor network.
## Set up the repository

1.  Update the `apt` package index and install packages to allow `apt` to use a repository over HTTPS:

*(This updates the list of repositories your OS pulls from when it installs software, then installs software you'll need to verify that Docker did indeed write the software. This is done with GPG keys, which get installed in the next step)*
```
 sudo apt-get update


  sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

2.   Add Docker’s official GPG key:

*(Create a folder to put GPG keys)*
```
 sudo mkdir -p /etc/apt/keyrings
```

*(Add the GPG key to the folder)*
```COPY
     curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

3. Use the following command to set up the repository:
```COPY
echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## Install Docker Engine

1.  Update the `apt` package index, and install the _latest version_ of Docker Engine, containerd, and Docker Compose, or go to the next step to install a specific version:

```
 sudo apt-get update
 sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

 >[!info]
 > Receiving a GPG error when running `apt-get update`?
 > Your default umask may not be set correctly, causing the public key file for the repo to not be detected. Run the following command and then try to update your repo again: `sudo chmod a+r /etc/apt/keyrings/docker.gpg`.

>[!HELP]
>If you were unable to install Docker on Ubuntu. check the version. Ubuntu 21.10 necessitates extra kernel modules, which you can install with `sudo apt install linux-modules-extra-raspi`

## Start Docker
```
 sudo service docker start
```

## Test to see if Docker is Functioning
```
sudo docker run hello-world
```
This command downloads a test image and runs it in a container. When the container runs, it prints a message and exits.

Docker Engine is installed and running. The `docker` group is created but no users are added to it. You need to use `sudo` to run Docker commands.

## Install Docker Compose
Docker compose allows you to deploy multi-container applications with a single command. this will be incredibly useful for deploying your network quickly.

Currently, there is no support for arm64 processors, which are used by the Raspberry Pi. However, there's a shell script that can install it for us.

```COPY
sudo curl -L --fail https://github.com/OmarCloud20/docker-compose/releases/download/1.28.5/run.sh -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
If that script doesn't work, try the following:
```COPY
sudo curl -L --fail https://github.com/AppTower/docker-compose/releases/download/latest/run.sh -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
Test to see if it worked by running
```
sudo docker compose version
```
You should see a result similar to below:
```
docker-compose version 1.28.5, built c4eba1f
```
(it may not be the exact same).

Once you have confirmed that it does indeed work, reboot the Raspberry Pi with
```
sudo reboot
```

##  Add the user to the Docker group
In order for you to run docker without `sudo`, you need to add your user to the Docker group. Run
```
sudo usermod -aG docker [your account username]
```