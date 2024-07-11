---
layout: post
author: milkydelta
title: "Using Tailscale to circumvent network content filters"
---

# Prelude - The Problem
I frequent an organisation that offers a WiFi network for personal devices. That network, because it is an organisational network, has filters enabled that block certain websites from loading. 

I first encountered this with my Google newsfeed. ***The algorithm*** has determined that I am interested in technology, PC gaming and open-source software. Some of the sites that write stories in these areas would present mysterious errors about insecure connections. Tapping past these would bring me to blank pages. Certain apps, like Reddit (don't judge me), Discord, and Whatsapp, would also fail to load.

I've watched enough Computerphile to know what a [man-in-the-middle](https://youtu.be/-enHfpHMBo4) attack is, so the certificate warnings quickly got my brain turning. I concluded that the network's default DNS server must be checking queries against some blocklist and colluding with the router to intercept matching requests so they may be logged and blocked.

I did not like this.

# Chapter 1 -"Proxy Avoidance"

## DNS

If the problem starts with DNS queries, the most obvious solution is to specify an alternative DNS server. That way, the filter can't know what websites I'm searching. Changing this is easy enough. Most devices will allow you to give at set two alternatives in their DHCP configuration panel. The most popular options available are Cloudflare 1.1.1.1, Google 8.8.8.8, and 9.9.9.9 by Quad9. Pop your favourite two into the configuration and you're off to the races, right?

No.

The universe is rarely so simple.

Someone on the filter design team came to the same conclusion we did. DNS runs unencrypted on port 53, so any request we send to any DNS server can be trivially redirected to the internal filter. I would need to be much smarter than that if I wanted to view my Reddit karma any time soon.

## VPN

If the network router itself is analysing your traffic to put redirection blockades in your way, the next step is to obfuscate your traffic. Ideally, all of it. A VPN can achieve this.

If you haven't touched the internet in a few years, you have likely been lucky enough to avoid the torrent of advertisements and sponsor spots for services like NordVPN, SurfShark, and PIA, so you might not know what a VPN is. The explanation I would give is that a VPN is a connection to a server somewhere that allows access to the network resources of that server. These resources can be other computers using the VPN, devices on the server's local network, or (importantly for us) the server's path to the internet. VPN connections are secured by encryption, so nobody between your device and the server can understand any of your communication.

This is good. A successful connection to a VPN server would make my traffic entirely unintelligible to the network filter. I chose to first test WARP by cloudflare, which routes you through the nearest cloudflare server. Those things are everywhere, so performance is usually very admirable.

I installed the app and 
<!---notes on mullvad
notes on discovery of fortiguard lookup

tailscale backstory
tailscale overview
wow, I love tailscale
DERP
Exit node
London
500 ping
Proxy Avoided

They did something about the control plane
nvm I have a workaround

steam deck

I have told nobody-ish, don't worry.
-->