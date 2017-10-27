---
layout: post
title: "ubuntu basic"
description: ""
category: linux
tags: [ubuntu,]
---
{% include JB/setup %}

1. How to autostart a script when boot to ubuntu

try to add this to /etc/rc.local before exit 0

./path_to_btsync/btsync --config /path_to_config/btsync.conf

2. How to check linux version

ubuntu:

	lsb_release -a

centos/redhat:

	cat /etc/*release*

kernel:

	uname -a


3. How to check user and group?

	groups <username>
	whoami

