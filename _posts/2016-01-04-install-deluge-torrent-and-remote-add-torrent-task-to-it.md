---
layout: post
title: "install deluge torrent and remote add torrent task to it"
description: ""
category:
tags: []
---
{% include JB/setup %}

sudo adduser --system  --gecos "Deluge Service" --disabled-password --group --home /var/lib/deluge deluge

sudo adduser <username> deluge

sudo adduser kodi deluge

sudo passwd deluge


su deluge

cp ~/.config/deluge/auth ~/.conf/deluge/auth.bak
vim ~/.config/deluge/auth

deluge-console "config -s allow_remote True"
deluge-console "config allow_remote"

exit

deluged

do it in client....


Preferences -> Interface and disable Classic Mode


Create a new entry with Add button:
Hostname is your server's IP.
Port should be default 58846.
Username and Password are those added to the deluged config auth file.

sudo systemctl restart deluged

Ok. done.  Now you can add torrent just like you do it on your local computer, but the torrent task will be start in your remote server.

Is that cool!



Document based on:
* [UserGuide/ThinClient – Deluge](http://dev.deluge-torrent.org/wiki/UserGuide/ThinClient)
* [UserGuide/Service/systemd – Deluge](http://dev.deluge-torrent.org/wiki/UserGuide/Service/systemd)
* [Installing/Linux/Ubuntu – Deluge](http://dev.deluge-torrent.org/wiki/Installing/Linux/Ubuntu)
