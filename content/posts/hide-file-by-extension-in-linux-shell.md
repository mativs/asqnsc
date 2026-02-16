+++ 
date = 2012-02-27T14:37:13-03:00
title = "Hide File by Extension in Linux"
slug = "hide-file-by-extension-in-linux" 
tags = ['shell']
categories = []
description = ""
+++


I was trying to hide the python compiled files .pyc when I run the ls linux command. By default, linux hides all files started with a dot.

The solution (for me) was to add/update this line in my .bashrc (in my home directory)

    alias ls='ls --color=auto --hide="*.pyc"'

Just to let you know, when you run

    ls -a

all files appear again. :D