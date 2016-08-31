---
layout: post
title:  "sniffing app traffic"
date:   2016-08-28 20:08:42 +0530
tags: tech netwroking
---

I'm always curious to see what lies under the hood of websites. Developer tools in Chromium and Firefox, provide a simple way to analyse the building blocks of websites. My favorite part - the network calls. _I can haz your API!_

With apps on the phone though, it's not as easy as a Ctrl + Shift + I. __mitmproxy__ a simple and powerful little tool to get this job done.

[mitmproxy](https://mitmproxy.org/) allows you to get between a computer and the internet - to examine data as the man-in-the-middle (mitm) without source or destination aware of it happened (proxy).  

For a detailed explanation how mitmproxy works, I'd suggest reading their [guide](http://docs.mitmproxy.org/en/latest/howmitmproxy.html).

[Setting up](http://docs.mitmproxy.org/en/stable/install.html) mitmproxy is a breeze on Linux, since it has support for all major distributions. Once installed, just fire it up from the command line on your computer and you're almost ready to go. The next step is to configure your client, which in my case was an android Nexus 5 running Android OS 6 Marshmallow.

#### Configure your Android device

* Ensure both your phone and computer (running mitmproxy) are on the same local network
* Copy the certificates from ~/.mitmproxy from the computer to your phone's storage
* Install the certificates from storage; Settings > Device > Security > Credential Storage
* Modify your phone's Wifi network settings to add Manual proxy
	* Proxy hostname: your computer's IP
	* Proxy port: 8080 
	* IP settings: Static
	* IP address: pick any valid IP address available on your network
	* Gateway: your computer's IP

And now that you can inspect traffic between your phone and the rest of the world.

Postscript: It's a bit unnerving when you see the sheer volume of network calls being sent from your device to locations across the globe. oO
