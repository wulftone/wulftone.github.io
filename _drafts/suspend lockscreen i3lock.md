---
layout: page
title: "i3lock suspend arch linux"
date: 22/01/2015
categories: arch linux
---

Here's how to set i3lock as your arch linux lock screen on suspend action:

{% highlight bash %}
/etc/systemd/system $ cat i3lock.service
[Unit]
Description=Lock the screen on sleep
Before=sleep.target

[Service]
User=trevor
Type=forking
Environment=DISPLAY=:0
ExecStart=/home/trevor/bin/lock

[Install]
WantedBy=sleep.target
/etc/systemd/system $ sudo systemctl enable i3lock
{% endhighlight %}
