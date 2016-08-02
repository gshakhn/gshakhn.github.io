---
title:  "On connecting to Pokemon GO with OpenVPN AKA exchanging security fitness for physical fitness"
date:   2016-08-02 08:00:00
---

TLDR: It looks like Niantic blocked access to Pokemon GO from DigitalOcean IPs. That's where I run OpenVPN. Boo. I changed my VPN settings to not go through the VPN when connecting to Pokemon GO. See TLDR at bottom for config.

# Background

## Pokemon GO

I've been playing Pokemon GO a bunch in the last few weeks. If you haven't played, the premise is you walk around in the real world and see Pokemon on the phone which you can capture. There are also gyms you can fight for for a social aspect.

The game itself isn't that fun, but it's encouragement for me to go out, walk around, and see stuff. I've been meaning to lose some weight and Pokemon GO is a good motivation tool.

# OpenVPN

Late last year, I setup a personal VPN server running in DigitalOcean. I was traveling out of country and am semi-paranoid about public WiFi security. I'm using [this Docker container](https://github.com/kylemanna/docker-openvpn) running in DigitalOcean (which implies that I trust DigitalOcean more than a random WiFi access point). I would blog about the setup, but I mostly just followed [this blog post](https://www.digitalocean.com/community/tutorials/how-to-run-openvpn-in-a-docker-container-on-ubuntu-14-04).

I use the OpenVPN client on iOS and am usually connected. Main exceptions are when I restart my phone or when the connection times out. In these cases, the client doesn't always reconnect.

# Problem

Starting late Saturday evening (2016-07-30), I started having connection problems. The game servers haven't historically been performant, so I'm semi-used to that. Usually restarting the app would fix things. This time though, I kept getting a "servers are having trouble" screen. I mostly forgot about it and did other things during the weekend.

Late Sunday night, after reading some Reddit posts about connection issues, I tried clearing my Safari cookies and restarting my phone. I could play again! Yay. I went on my merry way.

Sometime Monday, I realized I wasn't connected to the VPN. Oops. Let's connect. Huh, I'm having troubles connecting to Pokemon GO again. That's weird. Maybe if I turned off the VPN? It works again. Repeat a few times to verify.

My current hypothesis (based on the recent Poke Vision shutdown and a Slack conversation in [Chicago Tech Slack](http://www.chicagotechslack.com/)) is that Niantic cut off access to the game from DigitalOcean (and probably other cloud providers) due to Poke Vision and other bots. Unfortunately my OpenVPN server is hosted in DigitalOcean...

# Solution

I sent a support request to Niantic to see if it was intentional, but it's doubtful they will respond. (I'll update this blog post if they do). Until then, I had to work around the problem.

I still want to use my VPN for most traffic, but I'm willing to compromise it just for the IP addresses used by Pokemon GO. If I connect to the game using my non-VPN IP address, the game should work. Hopefully the connection will be secure enough without VPN. Even if it isn't and I somehow get hacked, only the game should be affected. Not a huge deal. (Famous last words).

After doing some googling, I found some likely IP candidates [here](https://phineas.io/projects/goStatus/getCurrentNianticMitigationIPS.php). Random sites on the internet are great for finding IP addresses, but I wanted to double check them for myself. Time to learn how to use Wireshark! The following steps are semi-based on [this post](https://ask.wireshark.org/questions/17559/packet-capturing-application-for-the-iphone).

1. `brew install wireshark --with-qt` - Wireshark in brew doesn't come with a GUI by default. The `--with-qt` flag installs it with the QT GUI.
2. `brew install wireshark-chmodbfp` - You have to do this due to security/permission issues I don't fully understand.
3. Get the iPhone's UDID. [Awesome tutorial](http://whatsmyudid.com/) on how to do so.
4. Make sure `rvictl` is installed. I had to start Xcode up for the first time and it prompted to me to install some command line tools, including `rvictl`.
5. `rvictl -s <UDID>` - Starts remote packet capture on the iPhone.
6. Start Wireshark and listen on the `rvi0` interface.
7. Start Pokemon GO on your phone. Play around a bit to get some network connections going.
8. Look under Statistics -> Endpoints -> IPv4 to see what IP addresses your phone talked to.
9. Verify that one of the IPs is `130.211.14.80`, which is the IP of `pgorelease.nianticlabs.com`. The latter was listed [here](https://phineas.io/projects/goStatus/getCurrentNianticMitigationIPS.php) as one of the endpoints. Go random person on the internet!
10. Modify your `.ovpn` to bypass the VPN for that particular IP. See below for directions.

# TLDR: Config you actually care about

Add the following to your client's `.ovpn` file:

    route-delay 2
    route 130.211.14.80 255.255.255.255 net_gateway

The [docs](https://community.openvpn.net/openvpn/wiki/Openvpn23ManPage) will explain what the above lines do much better than me.

# Caveats

* The only IP I needed to bypass is 130.211.14.80. I tried a few others, but they weren't actually needed. This might change in the future. Plus Niantic's IP might change too.
* I have no idea what cheating detection algorithms Niantic has. I'm connecting to their services from both my regular phone IP and the IP of the VPN server. My physical location is in Chicago while my VPN is in New York. They might falsely flag me as a cheater. :( - Hopefully the algorithms will detect that I'm actually human and not walking all over the place 24/7.
