---
layout: post
title: "git log introduction"
description: ""
category: git
tags: [git]
---
{% include JB/setup %}

This blog is inspired by:[ History and Collaboration , from Youtube视频地址](https://www.youtube.com/watch?v=b8OrbpZqX4o&index=2&list=PLg7s6cbtAD15Das5LK9mXt_g59DLWxKUe)

Examples:

输出10个commit

    git log 0

输出5个commit

    git log -5

输出一行的简单格式的2个commit

    git log -2 --pretty=oneline 
    git log --pretty=oneline | medium | full | fuller
    git log - 4 --author=Matthew 