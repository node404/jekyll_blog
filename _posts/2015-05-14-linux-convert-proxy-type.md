---
layout: post
title: "linux convert proxy type"
description: ""
category: linux
tags: [linux, proxy, ubuntu]
---
{% include JB/setup %}

# covert socks proxy to http proxy

## install polipo and configure

First, start out by making sure you have universe enabled, up to date package lists and downloading the polipo package (it has no dependancies).

	sudo apt-get update
	sudo apt-get install polipo

Now to edit the polipo config file. This is found in /etc/polipo/config, and by default is the only file there to configure.

Now I'm going to list the required changes to the config file - all 2 of them!. Remember to customise these as required :). Its also worth noting that theres lots of other options to play with, and you should feel free. these are just the 2 you need to change to get going.

	### Basic configuration

	# Add your proxy's address
	proxyAddress = 192.168.0.1

	# Allow from anyone in the 192.168.0.* range to connect to your proxy
	allowedClients = 192.168.0.0/24

Restart the service, and we are done!

	sudo /etc/init.d/polipo restart


## client config

###First things first - APT.  ```may not working on Ubuntu 14.04, I'm not try yet```

This one is stupidly easy - Open a terminal, and type the following (it creates a blank file)

	sudo vi /etc/apt/apt.conf

In the file, add

	Acquire::http::Proxy "http://192.168.0.1:8123";

as the only line.

###Firefox:

Open the browser, click Edit -> Preferences. Click the 'connection settings' button, and click on 'manual proxy configuration'. In the top field add the following:

	HTTP Proxy:    192.168.0.1        Port:    8123

PS, if you go and set up privoxy as well, this port is changed. One step at a time though!

###GNOME:

Click System -> Preferences -> network proxy. Click Manual proxy configuration, and put in the same details as above :

	HTTP Proxy:    192.168.0.1        Port:    8123

Log out and in to apply GNOME settings, and your done!


---
[Polipo - Community Help Wiki](https://help.ubuntu.com/community/Polipo)