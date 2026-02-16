+++ 
date = 2013-12-05T15:07:16-03:00
title = "Migrating to Google Apps"
slug = "migrating-to-google-apps" 
tags = ["google"]
categories = []
description = ""
+++

New job, new challenges. Not so fun challenges but challenges anyway. 

<!--more-->

> Hey boss, what about using Google Apps?

And now you have to start migrating all the company e-mails; so just some tips after my headaches. 

### The DNS Problem
I found this very useful [article](http://wireload.net/2010/03/how-to-accomplish-a-smooth-email-migration-for-your-domain/) but I couldn't use it and I have to come up with a new approach.

1. Google gives you a test domain, something like *youcompany.com.test-google-a.com*. In your old e-mail provider forward all e-mail to the new accounts with this test domain. For example; pepe@youdomain.com will forward to pepe@yourdomain.com.test-google-a.com. 
2. Configure your Google App account and encourage people to start using Google but don't change your MX records. E-mails arrives at your old provider but are forwarded to your Google account inbox. 
3. When you are sure that all users are using Google you can change your MX records. Some e-mails will arrive to the old account and will be forwarded, and some mails will arrive to Google. After 48 hours you are done. 

The trick? Although you haven't changed the MX records, Google lets you send e-mails with your new domain.

### Old e-mails
This is really a pain in the ass. I haven't found anything that really works. 

If you have and IMAP provider I would go with [offlineimap](https://github.com/OfflineIMAP/offlineimap) solution althought I have to find a [patch]( http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=681494) to make it work with my old shitty provider. I've submitted a [pull request](https://github.com/OfflineIMAP/offlineimap/pull/54) on the github page.

If you have e-mails downloaded to the computer the best, but awful, solution it's Thunderbird. Just remember to compact the Thunderbird folder before start moving e-mails. I couldn't move more than a few thousandths of e-mails at a time. And yes, don't forgot to be patient, you will have to try many times.

### Contacts
Export, transform with spreadsheet and import.

### Filters
Craftsman work.

### Google docs
There is no way to transfer ownership from your *Gmail* documents to your new domain documents. Your only option is to share, duplicate, re-share and delete.
