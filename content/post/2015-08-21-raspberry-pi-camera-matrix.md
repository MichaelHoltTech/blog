+++
categories = [ "Unifi Video", "Ubiquiti", "Raspberry Pi"]
date = "2015-08-21T14:46:39-08:00"
menu = ""
tags = []
title = "Raspberry Pi Camera Matrix"
+++

## I have posted version 2 of this script [HERE](http://www.mholt.tech/blog/2015/08/29/pi-cam-matrix-v2/)
Over the last few months we've been upgrading our Security Cameras, replacing a 10 year old vitek system with the Ubiquiti Unifi Video system.  We just finished installing the last of these Cameras and that included the need to find a new solution for our Children's Cameras to be displayed onto two TV's.  I found a Forum Post on the Ubiquiti Community that documented one person's script to display a [2x2 Camera Matrix on a Raspberry Pi](http://community.ubnt.com/t5/UniFi-Video/Tutorial-Raspberry-Pi-4-Cam-Matrix-Viewer-Appliance/m-p/1253309#U1253309)

So I went ahead and order 2x Raspberry Pi's to power both of these displays and thought "Perfect, someone else has done the leg work all I have to do is plug in the RTSP URL's and presto".  Unfortunatly, I ran into some issues with screen causing the video to jump a few seconds instead of playing smoothly.  I also couldn't get the code working as an init.d so I took a different approach using rc.local.

For those that are interested in how to make this happen here's the instructions and code:

** What you'll need:**

- Raspberry Pi2 (I got the Canakit Raspberry Pi 2 - Complete Starter Kit off of Amazon, comes with everything you need for this project)
- Unifi NVR Software running the most recent stable release
- HDTV or Computer Monitor capable of 1080p
- MicroSD to SD Card Adapter (Optional, only needed if you have to re-install raspbian)
- Keyboard for initial setup


### Instructions
1. **Put together and connect you PI2**

    Refer to the quick-start guide that is in the Box in order to get started with your Raspberry Pi and install Raspbian using the pre-loaded NOOBS installer on the SD Card

1. **Configure Raspbian**

    Once you arrive at the Raspi-Config screen you should change the default password (Option 2) and set a hostname (Option 8 - A2).

    After this is done click finish and you will be dropped to the command line. Overclocking is also possible, but not required or recommended for what we are doing.

    For more information on raspi-config, see <https://www.raspberrypi.org/documentation/configuration/raspi-config.md>

1. **Get the code**

    You can get the code from GitHub by entering:

    ````
    git clone https://gist.github.com/dde9de96b6a4d72b7f58.git raspicams
    ````
1. **Edit the files for your configuration**

    - cd into raspicams by entering:

         ````
         cd ~/raspicams
         ````
    - edit cams.sh by typing:

         ``nano cams.sh``

    - replace <rtsp_url_using_ip> with the RTSP url's for your cameras (I recommend using the Low Quality to save bandwidth and using an IP Address instead of FQDN to ensure no issues arrise.)  This url can be found in the NVR under RTSP Service when editing a camera

    - exit nano by hitting ctrl + x then entering y to save and enter to keep the filename

    - edit network-interfaces and set static IP address configuration for eth0 (Optional but Highly Recommended)

1. **Copy the files into place**

    - While still inside of ~/raspicams run this in order to copy all of these files into place:

      ``sudo chown root:root boot-config.txt network-interfaces rc.local cmdline.txt & sudo chmod +x * & sudo cp boot-config.txt /boot/config.txt & sudo cp cmdline.txt /boot/cmdline.txt & sudo cp rc.local /etc/rc.local & sudo cp cams.sh ~/cams.sh``

    - If you have also configured a static IP in network-interfaces run this command to copy that file into place:

      ``sudo cp network-interfaces /etc/network/interfaces``

1. **Reboot the Pi**

    At this point all of your configuration is done.  All that's left to do is reboot the Pi and let the cameras load up.  Enter this command to tell the Pi to reboot:

    ``sudo reboot``

### Troubleshooting

- **Display Issues:**

  If you're having strange display issues, such as banding, flickering, or the display just shuts off, it's somewhat expected for the Raspberry Pi.  To troubleshoot, you're going to have to make some changes to the /boot/config.txt file in order to force certain modes and resolutions.   The included config.txt works for my setup but your milage may vary depending on your TV/Monitor.

- **Starting with a Fresh Raspbian Install:**

  This is where you are going to need that option MicroSD to SD Card adapter.  I'm not going to cover how to do that because the instructions are easily found on the internet.  Easiest ones to follow are at <https://www.raspberrypi.org/documentation/installation/noobs.md>

- **Changing RTSP URL's:**

  This easiest way to update your RTSP URL's is to edit the files that were copied from the git folder onto your pi.  This can be done by running:

  ``nano ~/cams.sh``
