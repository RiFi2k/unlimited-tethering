# Unlimited Tethering

Bypass tethering caps or throttling on cell phone unlimited data plans. Potentially cancel your internet and route your whole home though your unlimited data plan.

Inspired from [XDA](https://forum.xda-developers.com/) forum threads.
[Working Options for Unlimited Tetherting, Hotspot, Carrier Check Bypass Methods](https://forum.xda-developers.com/showthread.php?t=3825144)
[Unlimited WiFi Tethering](https://forum.xda-developers.com/showthread.php?t=2914057)

## Requirements

* **Unlimited data plan**
* **Ability to hotspot your phone**
* **Computor or Router**

This is not a carrier specific method, I use Verizon but this should generally work with any carrier. It also doesn't matter if they throttle or cap you at 15GB or something, that is what we are about to work around.

If you are going to go the router method it will be a lot more work but the router will handle all the traffic routing and you can just connect any device in your house to your router and it will just work. If you are just going to use your PC then you can generally have this running in ~15 to 30 minutes.

I have personally used anywhere from 80-150GB of data with this method consistantly for 6+ months and never been throttled one time while my traffic was going through the tunnel.

## Overview

1) Download [Termux](https://termux.com/) app and [install openssh](https://wiki.termux.com/wiki/Remote_Access) on it.

2) Configure authentication as explained [here](https://wiki.termux.com/wiki/Remote_Access) for SSH. If you don't already have a keypair it explains how to set up an ssh keypair and use it to authenticate to your phone from a PC. I personally used my existing SSH public key and made a folder / file `~/.ssh/authorized_keys` on Termux and dropped it in there with something like `curl "https://github.com/rifi2k.keys" > ~/.ssh/authorized_keys` if you already have your public keys on github here.

2) Hotspot your phone.

3) run `ifconfig` inside Termux to get your current tethering local IP. It will be the only 192.x.x.x spit out and generally for andriod will be ending in 192.x.x.42. Save this.

4) Run `sshd -dD` inside Termux which starts an openssh server in debug mode to audit traffic. Your looking to see something like this as output from the above command.

```
debug1: Bind to port 8022 on ::.
Server listening on :: port 8022.
debug1: Bind to port 8022 on 0.0.0.0.
Server listening on 0.0.0.0 port 8022.
```

5) Now pop onto a PC and connect it to your hotspot.

6) Now SSH tunnel all the traffic from the device back through the openssh server your running on the Termux app. Now that you are on the same local network you can SSH tunnel into our saved IP address and port from earlier `192.x.x.42:8022` or similar.

I use [sshuttle](https://github.com/sshuttle/sshuttle) which already handles most of the [gotchas with tcp over tcp etc](https://sshuttle.readthedocs.io/en/stable/how-it-works.html). and which also has a solution for [Windows](https://sshuttle.readthedocs.io/en/stable/windows.html) and linux.

As long as you make sure all your traffic passes through the tunnel it 100 percent shows that all your internet is being used by Termux app not your hotspot app so you need no other spoofing of hops or anything because to your phone and carrier you are just using a bunch of data in termux, you do it right you will never be throttled.

## Contributing

Anyone can feel free to contribute any working method or enhancement to this method. If anyone wants to help with the router stuff have at it. Just open up a pull request.

## Issues

Issues or enhancement requests should go [here](https://github.com/RiFi2k/unlimited-tethering/issues).
