---
layout: post
title: "docker basic"
description: ""
category: docker
tags: [docker]
---
{% include JB/setup %}


##Getting a new image

	$ sudo docker pull centos
	Pulling repository centos
	b7de3133ff98: Pulling dependent layers
	5cc9e91966f7: Pulling fs layer
	511136ea3c5a: Download complete
	ef52fb1fe610: Download complete
	. . .

##Listing images on the host

	$ sudo docker images
	REPOSITORY       TAG      IMAGE ID      CREATED      VIRTUAL SIZE
	training/webapp  latest   fc77f57ad303  3 weeks ago  280.5 MB
	ubuntu           13.10    5e019ab7bf6d  4 weeks ago  180 MB
	ubuntu           saucy    5e019ab7bf6d  4 weeks ago  180 MB
	ubuntu           12.04    74fe38d11401  4 weeks ago  209.6 MB
	ubuntu           precise  74fe38d11401  4 weeks ago  209.6 MB

##run a container we refer to a tagged image like so:

	$ sudo docker run -t -i ubuntu:14.04 /bin/bash
	$ sudo docker run -t -i ubuntu:12.04 /bin/bash
	$ sudo docker run -t -i centos /bin/bash


Finding images
One of the features of Docker is that a lot of people have created Docker images for a variety of purposes. Many of these have been uploaded to [Docker Hub](https://hub.docker.com/) . We can search these images on the [Docker Hub](https://hub.docker.com/) website.

---

Based on:
[Working with Docker Images - Docker Documentation](https://docs.docker.com/userguide/dockerimages/)