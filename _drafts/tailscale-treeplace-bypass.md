---
layout: post
author: milkydelta
title: "Using Tailscale to circumvent network content filters"
---

# The Problem
I frequent an organisation that offers a WiFi network for personal devices. That network, because it is an organisational network, has filters enabled that block certain websites from loading. 

I first encountered this with my Google newsfeed. ***The algorithm*** has determined that I am interested in technology, PC gaming and open-source software. Some of the sites that write stories in these areas would present mysterious errors about insecure connections. Tapping past these would bring me to blank pages. Certain apps, like Reddit (don't judge me), Discord, and Whatsapp, would also fail to load.

I've watched enough Computerphile to know what a [man-in-the-middle](https://youtu.be/-enHfpHMBo4) attack is, so the certificate warnings quickly got my brain turning. I concluded that the network's default DNS server must be checking queries against some blocklist and colluding with the router to intercept matching requests so they may be logged and blocked.

I did not like this.

# Workarounds