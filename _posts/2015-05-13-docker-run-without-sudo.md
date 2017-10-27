---
layout: post
title: "docker run without sudo"
description: ""
category: docker
tags: [docker, linux]
---
{% include JB/setup %}



The docker daemon always runs as the root user, and since Docker version 0.5.2, the docker daemon binds to a Unix socket instead of a TCP port. By default that Unix socket is owned by the user root, and so, by default, you can access it with sudo.

> Starting in version 0.5.3, if you (or your Docker installer) create a Unix group called docker and add users to it, then the docker daemon will make the ownership of the Unix socket read/writable by the docker group when the daemon starts. The docker daemon must always run as the root user, but if you run the docker client as a user in the docker group then you don't need to add sudo to all the client commands. As of 0.9.0, you can specify that a group other than docker should own the Unix socket with the -G option.

Warning: The docker group (or the group specified with -G) is root-equivalent; see Docker Daemon Attack Surface details.

Example:
Add the docker group if it doesn't already exist.

	sudo groupadd docker

Add the connected user "${USER}" to the docker group. Change the user name to match your preferred user.

	sudo gpasswd -a ${USER} docker

or

	sudo usermod -aG ${USER} docker

Restart the Docker daemon:

	sudo service docker restart

If you are on Ubuntu 14.04 and up use docker.io instead:

	sudo service docker.io restart

Either do a newgrp docker or log out/in to activate the changes to groups.

PS:
check a linux user group:

	groups <username>

check current linux user group:

	groups

---

Based on :

* [Ubuntu - Docker Documentation](https://docs.docker.com/installation/ubuntulinux/#installing-docker-on-ubuntu)
* [How can I use docker without sudo? - Ask Ubuntu](http://askubuntu.com/questions/477551/how-can-i-use-docker-without-sudo)
* [See Which Groups Your Linux User Belongs To](http://www.howtogeek.com/howto/ubuntu/see-which-groups-your-linux-user-belongs-to/)