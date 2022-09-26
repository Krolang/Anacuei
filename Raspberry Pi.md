## Installing the Operating system
Take an 8GB microSD and insert it into your computer; you'll install the OS onto the microSD from the computer using the Raspberry Pi Imager (https://www.raspberrypi.com/software/) The version I used was Raspbian OS Lite (64 bit), as I didn't need a desktop for what I was using the Pi for. If you find it easier to use a GUI than a Command line, then you can use Raspian OS (64 Bit). However, this documentation uses the command line, so I recommend using a headless (comand line only) setup.

>[!NOTE]
You could also install PiHole on Docker, assuming you can get that working. 
## Troubleshooting Network Connectivity for the Pi
I had quite a bit of difficulty in getting [[Pi-Hole]] working, sometimes due to network issues, others due to missing repositories.

### Fixing Network interfaces
It may be the case that your Pi's network interfaces weren't configured before installing Pi-Hole, rendering it unable to install any of the packages. If you think you may have skipped this step, Check the [[Pi-Hole#1 Configure the Network Interfaces|Network Interface Configuration]] Section of this documentation

### Making the DNS server work
#### Fixing Missing Repositories
- If one of the repositories isn't installing, attempt to use a different installation method (I used bash, but it only worked after my seventh attempt to install was interrupted, and I have no idea why. This likely won't be the case on a newer model of pi).

#### Alternatively: Give up
- Depending on your [[Anacuei#Additional Reading|threat model]], It may be fine to forgo setting up Pi-Hole and use secure [[DNS#Upstream DNS|Upstream DNS servers]].