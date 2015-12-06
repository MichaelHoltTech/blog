+++
categories = [ "Unifi Video", "Ubiquiti", "Raspberry Pi"]
date = "2015-08-29T09:50:39-08:00"
menu = ""
tags = []
title = "Pi Cam Matrix v2"
+++

After a few days in production I discovered that over time the Pi's would lose connection to the RTSP feeds.  The only two ways to resolve this were to reboot the Pi or SSH into the Pi.  Because of this I started back at the beginning and was able to develop another way of running this matrix that would automatically reload the RTSP feed if it was lost.  The initial Pi Setup instructions are identical to my last post, [Raspberry Pi Camera Matrix](http://www.mholt.tech/blog/2015/08/21/raspberry-pi-camera-matrix/), the differences come in with setting up the new code base.

For those that are interested in how to make this happen here's the instructions and code:

**What you'll need:**

- Raspberry Pi2 (I got the Canakit Raspberry Pi 2 - Complete Starter Kit off of Amazon, comes with everything you need for this project)
- Unifi NVR Software running the most recent stable release
- HDTV or Computer Monitor capable of 1080p
- MicroSD to SD Card Adapter (Optional, only needed if you have to re-install raspbian)
- Keyboard for initial setup

<!--more-->

### Instructions
1. **Put together and connect you PI2**

    Refer to the quick-start guide that is in the Box in order to get started with your Raspberry Pi and install Raspbian using the pre-loaded NOOBS installer on the SD Card

1. **Configure Raspbian**

    Once you arrive at the Raspi-Config screen you should change the default password (Option 2), set a hostname (Option 8 - A2), set Memory split for video to 512 (Option 8 - A3), and overclock (Option 7 - Pi2).

    After this is done click finish and you will be dropped to the command line. I have found the overclocking to help with the videos loading initially, otherwise i've found it to take a minute or two for cameras to play smoothly.

    For more information on raspi-config, see <https://www.raspberrypi.org/documentation/configuration/raspi-config.md>

1. **Get the code**

    Clone the PiCamMatrix repo into /home/pi using

    ``git clone https://github.com/MichaelHoltTech/PiCamMatrix.git``

    Note: If you wish to use a different path, you need to ``Script=`` in every file to point to the path that the scripts reside.

1. **Edit the files for your configuration**

    - cd into PiCamMatrix by entering:

         ``cd ~/PiCamMatrix``
    - edit cams.txt by typing:

         ``nano cams.txt``

    - replace <rtsp_url_using_ip> with the RTSP url's for your cameras (I recommend using the Low Quality to save bandwidth and using an IP Address instead of FQDN to ensure no issues arrise.)  This url can be found in the NVR under RTSP Service when editing a camera

    - exit nano by hitting ctrl + x then entering y to save and enter to keep the filename

    - Configure a Static IP address in ``/etc/network/interfaces``.  I highly recommend wired instead of wireless.  If you need some instructions on setting a static IP this tutorial is very easy to follow: <http://www.modmypi.com/blog/tutorial-how-to-give-your-raspberry-pi-a-static-ip-address>

1. **Create SymLinks to init.d**

    - While still inside of ~/PiCamMatrix run this in order to link the init.d scripts to /etc/init.d:

      ````
      sudo ln -s /home/pi/PiCamMatrix/init.d/camtopleft /etc/init.d/camtopleft
      sudo ln -s /home/pi/PiCamMatrix/init.d/camtopright /etc/init.d/camtopright
      sudo ln -s /home/pi/PiCamMatrix/init.d/cambottomleft /etc/init.d/cambottomleft
      sudo ln -s /home/pi/PiCamMatrix/init.d/cambottomright /etc/init.d/cambottomright
      ````

1. **Enable the scripts to automatically start at boot**

    - Run these commands while still inside ~/PiCamMatrix in order for the cameras feeds to automatically load at boot:

    ````
    sudo update-rc.d camtopleft defaults
    sudo update-rc.d camtopright defaults
    sudo update-rc.d cambottomleft defaults
    sudo update-rc.d cambottomright defaults
    ````

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

  ``nano ~/cams.txt``
