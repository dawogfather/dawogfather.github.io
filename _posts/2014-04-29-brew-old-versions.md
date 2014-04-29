---
layout: post
title: Homebrew install specific version of formula
description: "Wondering how to install an older version of a homebrew formula"
tags: 
  - tip
  - homebrew
  - brew
  - previous
  - versions
comments: true
share: true
published: true
---

Recently wanted to install an older version of a homebrew formula...

After checking around on "StackOverflow: Homebrew install specific version of formula" thought I'd list out some of the steps for next time I need to do this.

Here goes:

Update the git repository containing homebrew formulas by running:
{% highlight bash %}
$ brew update
{% endhighlight %}	

Choose the specific version:
{% highlight bash %}
$ brew versions scala
2.9.1    git checkout 53a1f0b /usr/local/Library/Formula/scala.rb
2.9.0.1  git checkout cb1ab23 /usr/local/Library/Formula/scala.rb
2.9.0    git checkout 4002978 /usr/local/Library/Formula/scala.rb
2.8.1    git checkout 0e16b9d /usr/local/Library/Formula/scala.rb
2.8.0    git checkout fdb41a3 /usr/local/Library/Formula/scala.rb
2.7.7    git checkout 6a18e38 /usr/local/Library/Formula/scala.rb
2.7.6    git checkout a82e823 /usr/local/Library/Formula/scala.rb
2.7.5    git checkout e9dd256 /usr/local/Library/Formula/scala.rb
{% endhighlight %}	

For these to work you have to be in the working-directory:
{% highlight bash %}
$ cd /usr/local/Library/Formula
{% endhighlight %}	

Execute the git code for your version:
{% highlight bash %}
$ git checkout 4002978 /usr/local/Library/Formula/scala.rb
{% endhighlight %}	

And install:
{% highlight bash %}
$ brew install scala
{% endhighlight %}	

If you’ve already install an older version, you may have to brew remove scala

You may need to do some funky git reset's after this to get back to the latest versions too