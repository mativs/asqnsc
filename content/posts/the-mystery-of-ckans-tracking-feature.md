+++ 
date = 2013-02-26T15:05:49-03:00
title = "The Mystery of CKAN's tracking feature"
slug = "the-mystery-of-ckans-tracking-feature" 
tags = ['ckan']
categories = []
description = ""
+++


Don't waste your time diving into your [CKAN's](http://ckan.org/) source code, just add this setting to your ".ini" file

    ckan.tracking_enabled = true

And don't forget to run

    python setup.py develop

And daily

    paster tracking update
    paster search-index rebuild