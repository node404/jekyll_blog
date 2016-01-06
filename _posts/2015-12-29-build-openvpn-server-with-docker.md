---
layout: post
title: "尝试搭建openvpn server（with docker）"
description: ""
category:
tags: [openvpn, docker, linux]
---
{% include JB/setup %}

I always intersted in build services in my own VPS.
specially things like a server can help me free browser the internet.
But sometimes the connection between my own pc and the vps server are not quite good. So things not always go luck.

!!! this tutorial is not done yet

I find out openvpn is quite useful these days. So I'm thinking I will build one of my own. First I find some tutorials by google：

1. [How To Set Up an OpenVPN Server on Ubuntu 14.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-openvpn-server-on-ubuntu-14-04)
2. [Installing OpenVPN on Ubuntu Server 12.04 or 14.04 using TAP – Super Library of Solutions](http://www.slsmk.com/getting-started-with-openvpn/installing-openvpn-on-ubuntu-server-12-04-or-14-04-using-tap/)
3. [OpenVPN - ArchWiki](https://wiki.archlinux.org/index.php/OpenVPN#Routing_all_client_traffic_through_the_server)

Then I picked one of my VPS for the test. It was installed with Ubuntu 14.04 and has 512MB memory. Here are the steps of how I did it:

1st, I install the openvpn and easy-rsa:

    $ apt-get install openvpn easy-rsa

2nd, change the value of ```net.ipv4.ip_forward=1``` in /etc/sysctl.conf, then do ```sysctl -p```

3rd, copy ```/usr/share/easy-rsa/``` to  ```/etc/openvpn/``` and generate the openvpn keys in the directory of :

    $ make-cadir /etc/openvpn/easy-rsa (ps: this seems another way to do this)

OR

    cp -r /usr/share/easy-rsa/ /etc/openvpn
    mkdir /etc/openvpn/easy-rsa/keys
    openssl dhparam -out /etc/openvpn/dh2048.pem 2048

4th, change the directory to easy-rsa, then do the following command:

    cd /etc/openvpn/easy-rsa/
    source vars
    ./clean-all
    ./build-dh
    ./pkitool --initca
    ./pkitool --server server
    cd keys
    openvpn --genkey --secret ta.key
    ln -s openssl-1.0.0.cnf openssl.cnf     
    ./pkitool --initca

OR

    ./clean-all
    ./build-ca
    ./build-key-server server

5th, cp the generated keys to ```/etc/openvpn```:

    cp server.crt server.key ca.crt dh1024.pem ta.key /etc/openvpn/

6th, cp the ```server.conf``` sample to ```/etc/openvpn/server.conf```

    gunzip -c /usr/share/doc/openvpn/examples/sample-config-files/server.conf.gz > /etc/openvpn/server.conf

7th, change some value of the ```server.conf``` file:

    ;push "redirect-gateway def1 bypass-dhcp"

to

    push "redirect-gateway def1 bypass-dhcp"

    ;push "dhcp-option DNS 208.67.222.222"
    ;push "dhcp-option DNS 208.67.220.220"

to

    push "dhcp-option DNS 208.67.222.222"
    push "dhcp-option DNS 208.67.220.220"  (change to some dns server you trust)

    ;user nobody
    ;group nogroup

to

    user nobody
    group nogroup

8th,  start and check the openvpn service:

    service openvpn start
    service openvpn status


9th, generate key of client:

    ./build-key client1

10th, copy the sample and change the value of client.ovpn:

    cp /usr/share/doc/openvpn/examples/sample-config-files/client.conf /etc/openvpn/easy-rsa/keys/client.ovpn

    remote my-server-1 1194
    user nobody
    group nogroup

    # SSL/TLS parms.
    # . . .
    #ca ca.crt
    #cert client.crt
    #key client.key
    <ca>
    (insert ca.crt here)
    </ca>
    <cert>
    (insert client1.crt here)
    </cert>
    <key>
    (insert client1.key here)
    </key>
