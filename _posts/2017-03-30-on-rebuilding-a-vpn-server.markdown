---
title:  "On rebuilding a VPN server"
date:   2017-03-30 08:00:00
---

As mentioned in [connecting to Pokemon GO with OpenVPN]({% post_url 2016-08-02-on-connecting-to-pokemon-go-on-a-vpn %}), I've been running a personal OpenVPN server in DigitalOcean since late 2015. Recently, due to some bizarre behavior with Chromium and SSL on Google sites, I became a bit more paranoid and realized I hadn't updated the server in over a year. Oops. TunnelBlick was also complaining about using an unsecure cipher. Double oops.

Time to rebuild with more secure settings. I deleted the droplet in DigitalOcean and started over. My bash history is below. Unfortunately, I rebuild the server a few months before writing this post, so the rationale for some of the choices escapes me. I wish I [had taken notes]({% post_url 2017-03-25-on-note-taking %}) at the time.

    OVPN_DATA="ovpn-data"
    docker volume create --name $OVPN_DATA
    docker run -v $OVPN_DATA:/etc/openvpn --rm kylemanna/openvpn ovpn_genconfig -u udp://<YOUR_VPN_DOMAIN>  -C 'AES-256-CBC' -a 'SHA384' -T 'TLS-DHE-RSA-WITH-AES-256-CBC-SHA256'
    docker run -v $OVPN_DATA:/etc/openvpn --rm -it kylemanna/openvpn ovpn_initpki
    docker run -v $OVPN_DATA:/etc/openvpn --rm -it kylemanna/openvpn easyrsa build-client-full macbook nopass
    docker run -v $OVPN_DATA:/etc/openvpn --rm kylemanna/openvpn ovpn_getclient macbook > macbook.ovpn
    docker run -v $OVPN_DATA:/etc/openvpn -p 1194:1194/udp --privileged -e DEBUG=1 kylemanna/openvpn
    curl -L https://raw.githubusercontent.com/kylemanna/docker-openvpn/master/init/docker-openvpn%40.service > docker-openvpn@.service
    mv docker-openvpn@.service /etc/systemd/system/
    systemctl enable --now docker-openvpn@.service