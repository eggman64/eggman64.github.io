---
layout: post
title:  "Tunnelling over SSH"
date:   2015-11-25 13:24:00 +0530
tags: tech networking
---

Sometimes I need to access and websites services in the US from the other side of the globe. A Digital Ocean server running in their NYC region makes this possible without a VPN.

Here's a nifty little command that will create a simple tunnel using SSH and the SOCKv5 protocol.

#### Create tunnel
 
`ssh -C2qTnN -D 8080 user@server_address`

#### Configure firefox


* Type `about:preferences#advanced` in the address bar
* Open the Connection Settings menu 
* Select Manual proxy configuration > SOCKS V5
* Enter SOCKS Host: 127.0.0.1 Port: 8080

![configure firefox](http://i.imgur.com/Elw1VPo.png "Firefox configuration")

Now let's ask [our favourite search engine](https://duckduckgo.com/?q=whats+my+ip) for our IP

For those wondering what all those options in the ssh command mean

    C compress data
    2 use SSH v2
    q run in quiet mode
    T disable pseudo-tty allocation 
    n redirect stdin from /dev/null
    N do not execute a remote command 
    D setup the tunnel on port 8080 (change this and in firefox if desired)

For details refer to the ssh [man](http://linux.die.net/man/1/ssh) page

More complex ssh tunneling configurations can be found [here](https://calomel.org/firefox_ssh_proxy.html)

