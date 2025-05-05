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

Alas, we are thwarted again. Blocking a VPN is only slightly less trivial than configuring a DNS filter. Some servers are referenced by a domain, so the DNS filter catches them automatically. Others might have known static IP addresses, which can be added to the IP blacklist available on most corporate routers. Failing that, user traffic could be scanned for the structure of a known VPN protocol. Two of the most popular protocols are OpenVPN and Wireguard. Both are open-source, so the message format is very well-documented.

In addition to WARP, I used Mullvad for my tests. Both [WARP](https://developers.cloudflare.com/warp-client/warp-modes/) <sup>(through [BoringTUN](https://blog.cloudflare.com/boringtun-userspace-wireguard-rust/))</sup> and [Mullvad](https://mullvad.net/en/why-mullvad-vpn) use Wireguard.
The Mullvad website is actually very helpful for testing. They a

# Interlude - Fortiguard

The filter software at the organisation was not initially known to me. I couldn't exactly go probing around the network to find out.
Asking a member of staff was also not likely to get me anywhere. All I had for gathering information was the filter itself and a copy of Firefox for Android.
There are multiple ways to block a website. The simplest is just to return a dead address on the DNS query, such as 0.0.0.0.
More complex systems will redirect to their own server. That way, they can display a custom page to notify the user about the block. A consequence of this is that HTTPS will not work. The correct certificate is available only to the genuine server and only certificates with an untrusted issuer or an incorrect name would be available to the custom page. Fully-managed networks, as is common in enterprise, will have a central issuer of their own, which is trusted on all managed machines. That will solve HTTPS for those machines, but is not viable on unmanaged user devices, like the ones that frequent the organisational network. That is why I got certificate warnings instead of timeouts.

Coming back to the topic, I recall that the first redirect was an entirely blank page. It had no elements and was initially rather confusing.
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