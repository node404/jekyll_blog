---
layout: post
title: "ubuntu pptp client"
description: ""
category: linux
tags: [ubuntu, vpn]
---
{% include JB/setup %}

#install pptp client on linux 

my environment is ubuntu 14.04 LTS 64bit:



## im trying this:

[PPTP VPN client setup with pptpclient - ArchWiki](https://wiki.archlinux.org/index.php/PPTP_VPN_client_setup_with_pptpclient)


## this is not working


[origin article address](http://www.jamescoyle.net/how-to/963-set-up-linux-pptp-client-from-the-terminal)


##SET UP LINUX PPTP CLIENT FROM THE TERMINAL
A Virtual Private Network, or VPN, allows the client computer to connect to a remote local network to use it’s resources such as printers and file shares. There are several types of VPN such as PPTP and LP2SEC with varying types of protection. PPTP is not the most secure type of VPN but its the easiest to set up.

PPTP has numerous security risks which means that the data you are transferring through your VPN can easily be unencrypted. L2TP/IPsec is becoming the standard VPN technology of choice. PPTP should not be used unless security of each end point and the data transferred is not required.

Take the quick VPN Poll to tell us what type of VPN you use.

This tutorial assumes you have a PPTP server already set up with the following details:

Hostname: pptp.jamescoyle.net
Username: pptpuser
Password: pptppassword

Open a Terminal and install the required PPTP client packages.

	apt-get install pptp-linux network-manager-pptp

Create a credentials file with the username and password of the PPTP server:


	vi /etc/ppp/chap-secrets

Add your entry using the below attributes

[USER] – user name to log in to the VPN server
[SERVER] – name of server to use, PPTP in our case.
[SECRET] – password of the above [USER].
[IP] – ip of the server, * means all IPs.

	[USER]    [SERVER]    [SECRET]    [IP]

Example:

	pptpuser    PPTP    pptppassword    *

Create a file which will be executed when the PPTP connection is started. This can contain additional commands to run when the connection is started such as adding new routes or firewall exceptions.


	vi /etc/ppp/ip-up.d/route-traffic

The below examle script adds a route from the PPTP connection to any computers on the PPTP servers local network with IPs in the 10.0.0.0 or 192.0.0.0 ranges. This means that on the PPTP client, any machines on the above IP ranges will be accessible. This script may not be required for your environment and is simply used as an example. Note: a route should automatically be added to your VPN gateway.

	#!/bin/bash
	NET1="10.0.0.0/8"
	NET2="192.0.0.0/8"
	IFACE="ppp0"
	route add -net ${NET1} dev ${IFACE}
	route add -net ${NET2} dev ${IFACE}

Allow execution of the script:

	chmod +x /etc/ppp/ip-up.d/route-traffic

Add the PPTP client connection pool and any additional settings which are required. The connection name, jamescoyle.net, can be changed to suite your connection. 

	vi /etc/ppp/peers/jamescoyle.net

Add the details of the PPTP server. The below are the basic options required to connect to the server using mppe-128 encryption. Edit the below attributes to match your environment:

[USER] – user name to log in to the VPN server
[HOST] – host name or IP address of the PPTP server.

	pty "pptp [HOST] --nolaunchpppd"
	name [USER]
	remotename PPTP
	require-mppe-128
	file /etc/ppp/options.pptp
	ipparam jamescoyle.net

You must add rules to your firewall to allow connections to and from this interface as well as through your existing public interface to make the PPTP connection.  The below rules open all traffic on the new pptp interface using iptables. You may need to change this once the connection has been tested to increase security.

	iptables -A INPUT -i pptp -j ACCEPT

Finally you will need to start your PPTP client connection. Use pon and poff to start and stop your PPTP client. Replace [CONNECTION] with the name you gave to the file in /etc/ppp/peers/.

	pon [CONNECTON]
	poff [CONNECTION]

