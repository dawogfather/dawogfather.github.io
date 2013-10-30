---
layout: post
title: Selenium Server in AWS in 3minutes with vagrant
description: "Just about everything you'll need to create a selenium server setup for automated testing in the AWS cloud setup in minutes."
modified: 2013-10-29
tags: [aws vagrant devops cloud]
image:
  feature: abstract-3.jpg
comments: true
share: true
published: true
---

At my workplace, we had a dedicated Selenium Server which sits in the AWS cloud.  

We would use it to be able to run our test suites against a real web browser. 

By having a selenium server in the cloud we could then use [Madcow](http://madcow.4impact.net.au) or any other selenium/webdriver supported language for our web application testing requirements. 

But when AWS did updates ubuntu packages or anything changed, the system was started/stopped everything would come crumbling down again. 

I had heard, read and played a little with [Vagrant](http://www.vagrantup.com) which is basically a Developer Environment Virtual Machine management system but not used it yet. 

Then I stumbled upon this awesome article about [Running Headles Selenium With Chrome](http://www.chrisle.me/2013/08/running-headless-selenium-with-chrome/) by Chris Le 
which basically achieves what I was thinking about doing, but just needed a little tweaking. 

First I went off and installed latest version of vagrant and virtualbox. 

Then I installed the vagrant precise64 bit box by running...
```vagrant box add precise64 http://files.vagrantup.com/precise64.box```

Then I installed the vagrant aws provider plugin by running...
```vagrant plugin install vagrant-aws```

Next I created my Vagrantfile... again, slightly tweaked from Chris' to suit our requirements (e.g. ebs backed instance)

{% highlight ruby %}
# encoding: utf-8
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"    
  config.ssh.forward_agent = true

  config.vm.provider "virtualbox" do |v|
    v.gui = false      # adds a gui to virtuaBox locally
  end

  config.vm.provider :aws do |aws, override|
    override.vm.box = "dummy"               # overrides virtuabox config to use empty box as will load ami below...
    override.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"
    aws.access_key_id = '135124124124'      # Replace this with yours   
    aws.secret_access_key = 'XYZ'           # Replace this with yours
    aws.keypair_name = 'amazon-webdriver'   # the name of the keypair instance should use
    #aws.ami = 'ami-7747d01e'               # ubuntu 12.04 64bit precise instance backed
    aws.ami = 'ami-0145d268'                # ubuntu 12.04 64bit precise ebs backed
    aws.instance_type = 't1.micro'          # forced to t1.micro for us
    override.ssh.username = 'ubuntu'        # username override from default of vagrant
    override.ssh.private_key_path = './../../keys/amazon-key.pem' # path to my amazon pem file
    aws.security_groups = ['webdriver']     # name of the security group i have preconfigured in AWS with port 22 and 4444 open
    # aws.elastic_ip = 'someIPshouldGoHere' # was hoping to put an elastic IP in here but it never seemed to work        
    
    # below are the tags to help me easily identify these guys in the console
    aws.tags = {
      'Name' => 'vagrant-webdriver',
      'webdriver' => 'true'
    }
  end

  # tells vagrant to run the setup.sh when the shell is available
  config.vm.provision :shell, :path => "setup.sh"
  # tells vagrant to make the VM port 4444 accessible on host port 4444 (only works for VirtuaBox provider)
  config.vm.network :forwarded_port, guest:4444, host:4444

end
{% endhighlight %}

Basically we had a few more requirements, we wanted the remote server to be able to run firefox, phantomjs and chrome so to be able to do that I had to add a few more items to the setup.sh script.

It's included below: 

{% highlight bash %}
#!/bin/sh
set -e

if [ -e /.installed ]; then
  echo 'Already installed.'

else
  echo ''
  echo 'INSTALLING'
  echo '----------'

  # Add Google public key to apt
  wget -q -O - "https://dl-ssl.google.com/linux/linux_signing_key.pub" | sudo apt-key add -

  # Add Google to the apt-get source list
  echo 'deb http://dl.google.com/linux/chrome/deb/ stable main' >> /etc/apt/sources.list

  # Update app-get
  apt-get update

  # Install Java, Chrome, Xvfb, and unzip
  apt-get -y install openjdk-7-jre google-chrome-stable xvfb unzip firefox
  # apt-get -y install openjdk-7-jre google-chrome-stable xvfb unzip firefox phantomjs

  # Download and copy the ChromeDriver to /usr/local/bin
  cd /tmp
  wget "https://chromedriver.googlecode.com/files/chromedriver_linux64_2.2.zip"
  wget "https://selenium.googlecode.com/files/selenium-server-standalone-2.35.0.jar"
  wget "http://phantomjs.googlecode.com/files/phantomjs-1.9.1-linux-x86_64.tar.bz2"
  unzip chromedriver_linux64_2.2.zip
  mv chromedriver /usr/local/bin
  tar xjf phantomjs-1.9.1-linux-x86_64.tar.bz2
  mv selenium-server-standalone-2.35.0.jar /usr/local/bin
  sudo chmod +x /tmp/phantomjs-1.9.1-linux-x86_64/bin/phantomjs
  sudo ln -s /tmp/phantomjs-1.9.1-linux-x86_64/bin/phantomjs /usr/local/bin/phantomjs

  # So that running `vagrant provision` doesn't redownload everything
  touch /.installed
fi

# Start Xvfb, Chrome, and Selenium in the background
export DISPLAY=:10
cd /vagrant

echo "Starting Xvfb ..."
Xvfb :10 -screen 0 1366x768x24 -ac &

echo "Starting Google Chrome ..."
google-chrome --remote-debugging-port=9222 &

echo "Starting Selenium ..."
cd /usr/local/bin
nohup java -jar ./selenium-server-standalone-2.35.0.jar &
{% endhighlight %}

Then once I was done I could simply fire up a local ubuntu VM with all the stuff I need to run selenium tests against by exacuting 
```vagrant up```
at the command line. 

If I wanted my solution in the AWS cloud however, it was a simple matter of running 
```vagrant up --provider=aws```

Once it does it's thing I can ssh on to the VM by running 
```vagrant ssh``` 
which I needed to do to get the AWS public DNS IP. 
It will even rsync the current folder to the VM's /vagrant directory, so I can run all my tests right here on the VM if I wanted to.   

To run my webdriver (or remote Madcow in my case) tests from my host I can also simply point them to the remote server url endpoint of
```http://localhost:4444/wd/hub```
and then wait for my test results. 

Bam webdriver selenium test server setup and ready to go in minutes. 

Note: I also tried to configure an elastic IP setup for AWS provider run but was unsuccesful. I will update here when/if I get that working. 


