---
layout: post
title: "docker https daemon"
description: ""
category: docker
tags: [docker]
---
{% include JB/setup %}


#Protecting the Docker daemon Socket with HTTPS

HOST-IP:172.17.42.1
VM-IP:172.17.0.2


## gen ssl key and ca

    openssl genrsa -aes256 -out ca-key.pem 2048


    openssl req -new -x509 -days 365 -key ca-key.pem -sha256 -out ca.pem
    ```Common Name:  172.17.42.1```

    openssl genrsa -out server-key.pem 2048

    openssl req -subj "/CN=172.17.42.1" -new -key server-key.pem -out server.csr

    echo subjectAltName = IP:172.17.42.1, IP:172.17.0.2, IP:127.0.0.1 > extfile.cnf

    openssl x509 -req -days 365 -in server.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out server-cert.pem -extfile extfile.cnf

    openssl genrsa -out key.pem 2048

    openssl req -subj '/CN=client' -new -key key.pem -out client.csr

    echo extendedKeyUsage = clientAuth > extfile.cnf

    openssl x509 -req -days 365 -in client.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out cert.pem -extfile extfile.cnf


---

## change file mod

    rm -v client.csr server.csr

    chmod -v 0400 ca-key.pem key.pem server-key.pem

    chmod -v 0444 ca.pem server-cert.pem cert.pem

----

## start server

    sudo docker -d --tlsverify --tlscacert=ca.pem --tlscert=server-cert.pem --tlskey=server-key.pem -H=172.17.42.1:2376


# test

    sudo docker --tlsverify --tlscacert=ca.pem --tlscert=cert.pem --tlskey=key.pem -H=172.17.42.1:2376 version

---


## Secure by default

    $ mkdir -pv ~/.docker
    $ cp -v {ca,cert,key}.pem ~/.docker
    $ export DOCKER_HOST=tcp://172.17.42.1:2376 DOCKER_TLS_VERIFY=1

## edit /etc/default/docker

    DOCKER_OPTS="--tlsverify -H=unix:///var/run/docker.sock -H=0.0.0.0:2376 --tlscacert=/home/ubuntu/ca.pem --tlscert=/home/ubuntu/server-cert.pem --tlskey=/home/ubuntu/server-key.pem"

    service docker restart


---


[Running Docker with HTTPS - Docker Documentation](https://docs.docker.com/articles/https/)

[Quickstart · shipyard](http://shipyard-project.com/docs/quickstart/)
