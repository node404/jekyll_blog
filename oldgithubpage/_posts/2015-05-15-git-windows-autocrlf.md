---
layout: post
title: "git windows autocrlf"
description: ""
category: git
tags: [git, windows]
---
{% include JB/setup %}

##Global settings for line endings

The ```git config core.autocrlf``` command is used to change how Git handles line endings. It takes a single argument.

On Windows, you simply pass true to the configuration. For example:

    git config --global core.autocrlf true
    # Configure Git on Windows to properly handle line endings

You can also provide a special ```--global``` flag, which makes Git use the same settings for line endings across every local Git repository on your computer.

##Per-repository settings


Optionally, you can configure the way Git manages line endings on a per-repository basis by configuring a special ```.gitattributes``` file.


Here's [an example of the file](https://github.com/github/developer.github.com/blob/fd5c0e653323bfad50eda21f8915155293b4293d/.gitattributes) in the GitHub Developer's Guide.

A ```.gitattributes``` file looks like a table with two columns:

On the left is the file name for Git to match.
On the right is the line ending configuration that Git should use for those files.


    # Set the default behavior, in case people don't have core.autocrlf set.
    * text=auto

    # Explicitly declare text files you want to always be normalized and converted
    # to native line endings on checkout.
    *.c text
    *.h text

    # Declare files that will always have CRLF line endings on checkout.
    *.sln text eol=crlf

    # Denote all files that are truly binary and should not be modified.
    *.png binary
    *.jpg binary

---

[Dealing with line endings - User Documentation](https://help.github.com/articles/dealing-with-line-endings/)

[git - Line endings with cygwin and Github for Windows - Stack Overflow](http://stackoverflow.com/questions/15717077/line-endings-with-cygwin-and-github-for-windows)
