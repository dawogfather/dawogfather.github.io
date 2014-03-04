---
layout: post
title: "Maven missing on Mavericks"
description: "How to quickly put maven on mavericks"
tags: [maven, mavericks, mac, osx]
comments: true
share: true
---

Maven doesn't ship by default anymore on Mavericks, which is annoying but lucky homebrew makes it easy to get it installed.  

Just follow these two steps to install homebrew (if you haven't already got it)
and then install the maven brew...

Then sit back and enjoy some coffee. 

{% highlight bash %}
	#install homebrew if you don't have it already
	ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go/install)"

	#install maven
	brew install maven
{% endhighlight %}

