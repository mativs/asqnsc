+++ 
date = 2014-09-04T15:10:18-03:00
title = "Raspberry Torrent Server"
slug = "raspberry-torrent-server" 
tags = ['raspberry']
categories = []
description = ""
+++


## Project Description

> This is my first project with a raspberry pi. It's not big deal and what is more I've already dismantled it. However, I wanted to write this post anyway.

I wanted a torrent server that would automatically download my favorites anime series without any human intervention and I prefer the idea of having my raspberry pi running all day instead of a full server. 

> Although it was my original idea, the external HDD configuration is outside of the scope.

The system is divided in two components; [deluge](http://deluge-torrent.org/) as my torrent client and [flexget](http://flexget.com/) as my automation tool for downloading and handling content.

I choosed *deluge* instead of *transmission* or *utorrent* because it let me set some useful configuration on each torrent; for example the destination folder, the file name, the ratio, and above all, to remove torrents after reaching a ratio limit.

And *flexget* it's just awesome. Let's see what happens when a new torrent appears on a torrents rss feed.

1. The .torrent file will be downloaded.
2. The series name, season and chapter's information will be detected by the torrent's filename.
3. Extra information for the chapter will be downloaded from <http://www.thetvdb.com>.
4. The .torrent file will be uploaded to deluge.
5. Some configurations will be set on the file that is being downloaded by deluge
  - The destination directory for the file written in this way: `<series_name>/<season_number>`
  - The file name written in this way: `<series_name>_S<season_number>E<chapter_number>`.
  - The upload limit ratio.
  - The *remove after upload limit* configuration.
6. A pushbullet message will be sent to my mobile device.

It's really awesome!!!

## Configuration

> The whole tutorial is based on a [Raspbian](http://www.raspberrypi.org/downloads/) image. 

### Install Deluge and Deluge Dependencies

```bash
sudo apt-get purge xserver.* x11.* xarchiver xauth xkb-data console-setup xinit lightdm lxde.* python-tk python3-tk scratch gtk.* libgtk.* openbox libxt.* lxpanel gnome.* libqt.* libxcb.* libxfont.* lxmenu.* gvfs.* xdg-.* desktop.* tcl.* shared-mime-info penguinspuzzle omxplayer gsfonts
sudo apt-get --yes autoremove
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install deluged deluged-webui
sudo apt-get install libgnutls28 libgnutlsxx28 libgnutls-dev
```

> The last line is about solving [this](http://www.raspberrypi.org/forums/viewtopic.php?t=72587&p=523131) issue.

### Create Deluge's Init Script

Add deluge user

```bash
sudo adduser --system --group --home /var/lib/deluge deluge
```

And then use it in [this](http://dev.deluge-torrent.org/wiki/UserGuide/InitScript/Ubuntu) tutorial to create deluge's init script.

Afterwards go to `<raspberry-pi-ip>:8112` to check that is working.

> The default password is `deluge`.

### Configure Deluge For Flexget Access

```bash
sudo -u deluge vi /var/lib/deluge/.config/deluge/auth
```

And add this line to end of the file

```bash
<a_username>:<a_password>:10
```

### Flexget instalation

```bash
sudo apt-get install python-pip python-dev
sudo pip install flexget
```

And then follow [this](http://flexget.com/wiki/Install) flexget tutorial.

### Flexget Configuration

You really should look at *Fleget* documentation but just for the record here is my anime flexget configuration file.

```yaml
templates:
  series:
    thetvdb_lookup: yes
    deluge:
      username: ausername
      password: apassword 
      main_file_only: yes
      ratio: 2
      removeatratio: yes
    exec: "mkdir -p \"{{ movedone }}\""
    pushbullet:
      apikey: "an-api-key"
      title: "Downloading {{ series_name }}"
      body: "Season {{ tvdb_season }}, Episode {{ tvdb_ep_id }}. Original title {{ title }}"
tasks: 
  anime_feed:
    priority: 1
    template: series
    rss:
      url: http://www.nyaa.se/?page=rss&cats=1_37&filter=2
      all_entries: no
    configure_series:
      settings:
        quality: "1080p"
        from_group:
          - Hiryuu
          - tsuki
          - yibis
          - horriblesubs
          - A-destiny
      from:
        listdir:
          - /path/to/anime/folder 
    set:
      path: "/path/to/download"
      movedone: "/path/to/anime/folder/{{ series_name }}/S{{ tvdb_season }}"
      content_filename: "{{ series_name }}.{{ tvdb_ep_id }}"
```

Hope it helps.