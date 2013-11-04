---
layout: post
title: "Fixing vagrant up hostonlyif error"
description: "A coderwall pro tip I found for hostonlyif errors"
tags: [virtualbox, pro, tip, coderwall, vagrant, link post]
comments: true
link: https://coderwall.com/p/ydma0q
share: true
---

If you're having wierd hostonlyif errors when trying to do 
```vagrant up``` then checkout this pro-tip on coderwall. 
It'll kickstart the required VirtualBox processes again and get you going again.
Basically you just restart VirtualBox by running 
```sudo /Library/StartupItems/VirtualBox/VirtualBox restart``` on OSX to fix it up