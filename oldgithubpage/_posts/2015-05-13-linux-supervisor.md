---
layout: post
title: "supervisor"
description: ""
category: linux
tags: []
---
{% include JB/setup %}

# supervisor, linux process manger

Supervisor is a process manager which makes managing a number of long-running programs a trivial task by providing a consistent interface through which they can be monitored and controlled.


As the root user, run the following command to install the Supervisor package:


	sudo apt-get install supervisor

	sudo service supervisor restart

enter cmd shellL
	sudo supervisorctl

add web console:

	[inet_http_server]
	port = 127.0.0.1:9001
	username = user
	password = 123

add scrpit conf(/etc/supervisor/conf.d/long_script.conf):

	[program:long_script]
	command=/usr/local/bin/long.sh
	autostart=true
	autorestart=true
	stderr_logfile=/var/log/long.err.log
	stdout_logfile=/var/log/long.out.log