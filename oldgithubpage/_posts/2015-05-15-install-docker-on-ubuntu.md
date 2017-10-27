---
layout: post
title: "install docker on ubuntu"
description: ""
category:docker
tags: [install, docker]
---
{% include JB/setup %}


Make sure you have intalled the prerequisites for your Ubuntu version. Then, install Docker using the following:

Log into your Ubuntu installation as a user with sudo privileges.

Verify that you have wget installed.

    $ which wget

If wget isn't installed, install it after updating your manager:

    $ sudo apt-get update $ sudo apt-get install wget

Get the latest Docker package.

    $ wget -qO- https://get.docker.com/ | sh

The system prompts you for your sudo password. Then, it downloads and installs Docker and its dependencies.

Verify docker is installed correctly.

    $ sudo docker run hello-world

This command downloads a test image and runs it in a container.

Don't forget to run service if you got an error

    $ sudo service docker start
