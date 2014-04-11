---
layout: post
title: Compiling Native Extensions under Xcode 5.1
description: "Recently updated to xcode 5.1 and now everything breaks as clang has changed. This is how you can get around the “clang: error: unknown argument” error"
tags: 
  - tip
  - extensions
  - compile
  - clang
  - xcode
  - native
  - osx
  - link post
comments: true
link: "http://blog.manbolo.com/2014/03/26/xcode-5.1-breaks-python-native-extensions-and-ruby-gems"
share: true
published: true
---

Trying my hand of late with python and ruby and other bits and pieces but found myself hitting a lot of clang compiler issues when trying to install ruby gems etc on my mac, specifically this sort of thing:

{% highlight bash %}
compiling generator.c
linking shared-object json/ext/generator.bundle
clang: error: unknown argument: '-multiply_definedsuppress' [-Wunused-command-line-argument-hard-error-in-future]
clang: note: this will be a hard error (cannot be downgraded to a warning) in the future
make: *** [generator.bundle] Error 1
{% endhighlight %}	

Luckily after some googling [manbolo.com](http://blog.manbolo.com) to the rescue, Xcode 5.1 it appears has changed something from a warning to an error as explained in more detail on the blog post [here](http://blog.manbolo.com/2014/03/26/xcode-5.1-breaks-python-native-extensions-and-ruby-gems). 

Anyway whack in an 

{% highlight bash %}
export ARCHFLAGS=-Wno-error=unused-command-line-argument-hard-error-in-future
{% endhighlight %}

and you should be right next time you try to 

{% highlight bash %}
gem install pg
{% endhighlight %}

or of course you may need to do this if your using sudo

{% highlight bash %}
sudo ARCHFLAGS=-Wno-error=unused-command-line-argument-hard-error-in-future gem install helios
{% endhighlight %}

Hope this helps you too.