+++
categories = ["Mac Management"]
date = "2015-10-08T19:48:15-08:00"
menu = ""
tags = []
title = "Our path to Mac Management"
+++
### Where we Started

> “In the beginning God created…” the Mac… “and it was good”

Ok maybe I went a little far in that one.  I came on staff at Real Life Church in 2012 and at that time we were mostly a PC shop with the exception being Pastors exclusively having a Mac (With Local Admin Access and no admin account for IT).  Over the next 2 years we realized that we got significantly more support requests on our PCs than for our Macs. We were running on an aging SBS 2008 server that was maxed out on licensing and we were continuing to grow.  After numerous conversations with my Director and leadership the decision was clear that we needed to make the transition to a 100% Mac shop in our church.  

<!--more-->

This decision was based on the need to purchase new hardware and spend hundreds of man hours (as the solo IT Admin) in building out a new server infrastructure to manage the windows Machines with no guarantee that it would cut down on the ongoing support needs.  The direction was clear at the beginning of 2014 that we would be transitioning to the Macs as soon as we had the capital to spend on them.  

Knowing this, I began to research and address ways to manage software, patches, and support the new influx of Macs.

###Introducing the Munki

![alt text](https://lh3.googleusercontent.com/-SZsDtJV44Ic/T8ytQjvo24I/AAAAAAAAAlc/A4b9DjdBWGk/s640/blogger-image-1072467874.jpg "Oh No, Monkeys on a Mac! - Image courtesy of http://nikiwallacedesign.blogspot.com/2012/06/think-first-design-later.html")


Ok so I’m not actually talking about a real Monkey here.  Munki is a set of tools that can be used to manage software installs on OS X Client Machines.  It was developed by Gregg Neagle (@gregneagle) at Walt Disney Animation Studios and was open sourced for anyone to use and contribute to.  I had heard about Munki a couple of times in discussions with Churches and other people about how to manage a Mac and finally decided to start looking into it.  In addition to do managed and optional installs as well as updates of 3rd party software, it is also capable of installing Apple Software Updates.

###Our Requirements for Staff Macs

One of the first things we decided on was some of the requirements and restrictions that we wanted in place on the Mac.  Every organization is different but this is where we settled.

1. No end user will be an administrator on their Machine

2. Only approved software can be installed and can be found inside Munki

3. No App Store apps allowed
One of the continuous debates is Admin vs No Admin regardless of platform (Mac vs PC).  We decided on the no admin approach because we did not want our staff being able to install unapproved software or changing system settings that could negatively affect their Machine.

###Challenges

I spent about 10 months developing our Munki infrastructure and learning how it worked.  Part of the reason for taking this long is when first evaluating it at the beginning of the year there was already progress being made on version 2 that turned Munki into a replica of the Apple App Store in terms of layout and usability.  

The primary opposition we faced was a number of employees responded with: “I’ve always used a PC and have never touched a Mac, how am I supposed to get my job done?”  We encouraged our users to keep an open mind and we spent time with them after migrating their data showing them the basics of how to use a Mac and spending 15-20 minutes showing them the basics on how to open documents and applications.  Within a few months I was hearing from everyone that they “loved” their Macs and are glad to have them.

###Issues with Non-Admins and Munki

After a few months in we started finding a few issues with our configuration and stance of no Local Admin access.  This included:

1. Users could not change their energy saver preferences

2. Users could not install home printers

3. Printers would “pause” when running out of paper or toner, and paper jams. Admin credentials were required to resume printing

4. Management of software updates became cumbersome and users were requesting software that would have been available in the Mac App Store

###Our Next Steps

We have been using Munki for a year now and when re-evaluating our setup and options we discovered that we were spending significantly more than we had anticipated by running our system on Amazon EC2.  We also wanted to address some underlying issues with configuration management, printers, and opening up the App Store.  This led us to make some new changes which I will highlight here and detail further in future blog posts.

We decided to go with the following systems to comprise our Mac Management Solution:

1. [**Simian**](https://github.com/google/simian), a backend solution to power the Munki client, hosted on Google App Engine and developed by Google MacOps, along with customizations to Munki to create a secure connection between the client and server.

2. [**Puppet**](https://puppetlabs.com/), a configuration management system that allows you to define the state of your Machines.

3. [**Munkireport-php**](https://github.com/Munkireport/Munkireport-php), a powerful system reporting utility that provides detailed system information on your Macs

4. [**Crypt**](https://github.com/grahamgilbert/Crypt), a Filevault Key Escrow solution built by Graham Gilbert for recording filevault recovery keys

5. [**Macnamer**](https://github.com/grahamgilbert/Macnamer), a web app and Mac script that runs on your Mac to force your Machine to follow a naming convention with an automatically generated number suffix. Another solution built by Graham Gilbert

6. [**Sal**](https://github.com/salopensource/sal), a Multi-Tennant reporting dashboard similar to Munkireport that provides another way of viewing system information.

7. [**Autopkg**](https://github.com/autopkg/autopkg), automates the download and manifest creation of software for import into simian.  This is not fully automated but allows me to simply copy paste into Simian to provide updates.

8. [**Autopkgr**](https://github.com/lindegroup/autopkgr), ties into Autopkg and checks for software updates every night and sends an email alert when software updates are installed

9. [**Screenconnect**](https://www.screenconnect.com/), an Unattended Remote Support software for Mac/PC/Linux.  We use this to be able to remotely support our Users.

10. **Not Yet Implemented** - [**Imagr**](https://github.com/grahamgilbert/imagr), an open source replacement to DeployStudio for workstation imaging and thin-provisioning.

Everything except Simian runs inside a single server on Digital Ocean inside of Docker Containers.  I plan on publishing additional information on my setup and instructions on how to create this setup in the near future.
