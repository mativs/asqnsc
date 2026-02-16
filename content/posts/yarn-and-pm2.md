+++ 
date = 2022-04-28T11:47:21-03:00
title = "Yarn on PM2: Missing ) argument or unexpeted token '('"
slug = "yarn-and-pm2" 
tags = ['js']
categories = []
description = ""
+++

I have stumbled upon this problem for the second time, so I am writing this post just for me and for anyone that is looking in google for something like this:

`SyntaxError: missing ) after argument list running yarn on pm2`

or maybe something like this

`syntax error near unexpected token '(' running yarn on pm2`

The problem here, is that you´ll find on internet, after reading some git issues and some answers on stackoverflow, that the problem has something to do with how pm2 runs your script.

[Some](https://stackoverflow.com/a/67014596) links will tell you that you need to remove `--interpreter bash` from your script.

And [others](https://stackoverflow.com/a/51902772) will tell your that you need to add `--interpreter bash`.

Depending on your problem, one of the oposing two options will fix your issue.

However, if you work with other people, you will find that adding (or removing) `--interpreter bash` will solve your problem but generate a problem on all the others machines.

So, let my future me remember this.

The problem here is how you install (or update) yarn on your machine.

If you install it using for example `npm install -g yarn` your yarn versión will be installed as a js lib. However, if you install it using other methods, for example `sudo apt install yarn` it will be installed as a bash script.

So, what you have to enforce, in all the people working on your project, is to install yarn in the same way and then add or remove your `--interpreter bash` argument

Hope it helps
