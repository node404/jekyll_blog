---
layout: post
title: "vim change global font "
description: ""
category: linux
tags: [vim]
---
{% include JB/setup %}

#change vim font 

In gvim, you can change the font using the Edit menu, Select Font. An alternative is to enter the command:

	:set guifont=*

Once you have a font you like, you want to make it the default in the future. Do

	:set guifont?

and Vim will display something like

	guifont=Lucida_Console:h11

Alternatively, enter the following to insert the current font setting into the buffer:

	:put =&guifont

Now put a line in your vimrc to set guifont to this value, like this:
	if has('gui_running')
	  set guifont=Lucida_Console:h11
	endif
If there is a space in the font name, such as

	guifont=Monospace 10

it is necessary to escape the space

	set guifont=Monospace\ 10

---

[Change font - Vim Tips Wiki](http://vim.wikia.com/wiki/Change_font)
