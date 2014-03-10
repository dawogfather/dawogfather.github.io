---
layout: post
title: "Linux Disk Space Hogs"
description: "Find the fat ducks"
tags: [mac, osx, linux, unix]
comments: true
share: true
---

Sometimes you need to find the disk space hogs on a linux machine. 
Think of them as the big fat ducks if you like. 
Then you can just add this to your profile and recursively find the top 10 fat disk hogs. 

{% highlight bash %}
	#setup a shortcut to call the command
	alias ducks=du -cks * |sort -rn |head -11
{% endhighlight %}

