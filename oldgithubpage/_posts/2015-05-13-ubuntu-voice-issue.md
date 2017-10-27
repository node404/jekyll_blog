---
layout: post
title: "ubuntu voice issue"
description: ""
category: linux
tags: [ubuntu, linux]
---
{% include JB/setup %}

#How to solve ubuntu voice issue

My ubuntu device doesn't output the voice through my hdmi, here is the solution based on [sound - Ubuntu refuses to output audio via HDMI - Ask Ubuntu](https://askubuntu.com/questions/112512/ubuntu-refuses-to-output-audio-via-hdmi/232407#232407)


You can try installing program "PulseAudio Volume Control" 

	sudo apt-get install pavucontrol

then

	pavucontrol

In tab Configuration select profile mentioning HDMI.

The desktop sound output should immediately switch to HDMI.

---

backfall solution 2

With the HDMI cable plugged in, run the following utility:

speaker-test -c 2 -r 48000 -D hw:0,3
(for others, the hw:x,y is x = card, y = device)

This just runs a sound test with static bouncing back and forth. Once it has played out of each speaker, hit Ctrl+c to stop it, and then check in your sound settings to see if the HDMI output is now listed under the internal device.

That was all I had to do to get it working. If it doesn't work for you, check out this tutorial for upgrading alsa to the ppa version.