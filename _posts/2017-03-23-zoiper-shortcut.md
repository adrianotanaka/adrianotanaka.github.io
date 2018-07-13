---
title: Creating Zoiper shortcut
date: 2017-03-23 00:18:23 Z
categories:
- posts
---
## Creating Zoiper shortcut

Today I installed the Zoiper softphone in Fedora 25 (Cinnamon) but at the end, the desktop shortcut was not created.
So, this is the .desktop file to create:

{% highlight shell %}

#!/usr/bin/env xdg-open
[Desktop Entry]
Type=Application
Encoding=UTF-8
Name=Zoiper
Comment=Zoiper
Exec=/usr/bin/zoiper
Icon=/usr/share/pixmaps/zoiper-48.png
Terminal=false

{% endhighlight %}

Put this file in your home directory with Zoiper.desktop name
