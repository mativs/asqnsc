+++ 
date = 2026-02-21T18:57:27-03:00
title = "Never trust a float"
slug = "never-trust-a-float"
tags = ["python"]
categories = []
thumbnail = "images/tn.png"
description = ""
+++

You should never, but never trust a float. Yes I'm being a little bit extreme, but this is an effective rule.

I can still remember myself saying something totally, but totally wrong. I guess we can only truly learn by failing. 

> Hey guys, this cannot be floating point issue, we are just multiplying. It's not like we are dividing something and receiving an imprecise result.

Well, the ending of the story is quite obvious; my mistake.

Once you know this, saying it out loud seems a little obvious — but at the time, I had no idea.

What's the result of this in python?

```python
9 * 0.3
```

Tada!!

```python
2.6999999999999997
```

So, I learned two important things here:

First, understand things to the bone, not just a thin top layer of understanding. Or at least, be aware of your weak commitment and don't assert things that will make you sound like an idiot.

Second one, and what brings us here, is to never trust a float. Of course, there are plenty of cases where using a float is correct and way better, but, I would say, if you are reviewing code (yours or from another one, human or AI) you should be aware of even the slightest simple operation. Especially if you are later using the original value or the result for a comparison with another float value. 

So, if you see a float, raise your internal alarms and dig deep — check if any precision errors could impact your business

Hope it helps.



