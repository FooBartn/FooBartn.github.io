---
layout: post
title: "Install Jekyll On Windows 10 (The Awesome Way)"
description: "Jekyll on Ruby on Bash on Ubuntu on Windows"
date: 2016-09-07 20:40:00
comments: true
keywords: "Jekyll, Windows, Ruby, Bash, Ubuntu"
category: Linux on Windows
tags:
- windows
- linux
- jekyll
- ruby
- ubuntu
- bash
---

>*UPDATE:* Troubleshooting added to the bottom of the page

# Don't Be BASHful
Or is that Be BASHful? I'm not sure. That joke got away from me already.

First order of business: 

- Enable "Bash on Ubuntu on Windows"

That's right. CLICeption. Managing a Jekyll static site on Windows just got a whole lot easier! This is a fast track setup (no pictures). Assumes you know at least a little about what you're doing.

## Windows Dev Mode

First, enable Windows Dev Mode.
Run this in an **Elevated** PowerShell window:
{% highlight powershell %}
$DevModeKey = 'HKLM:\Software\Microsoft\Windows\CurrentVersion\AppModelUnlock\'
Set-ItemProperty -Path $DevModeKey -Name 'AllowDevelopmentWithoutDevLicense' -Value 1 -Type DWord
Set-ItemProperty -Path $DevModeKey -Name 'AllowAllTrustedApps' -Value 1 -Type DWord
{% endhighlight %}

Now run the App Wizard. Type this in PowerShell:
{% highlight powershell %}
& appwiz.cpl
{% endhighlight %}
Or just Start--> Run --> appwiz.cpl

* Select Turn Windows features on or off (Left Sidebar)
* Scroll down and check the box labeled "Windows Subsystem for Linux Beta"
* Allow your computer to Restart

Upon restart you want to run bash.exe, either from PowerShell or you can hit the Windows key and type "bash.exe"

You will get a terms of service window saying this is a Beta Feature. Type 'y' to continue. Don't back out on me now!

Follow the instructions to create a user account and password. **DO NOT** use lxrun /install /y. **DO NOT** use root as your user. JUST DON'T DO IT! Use Sudo.. same as any sane person would on a straight Linux distro.

Now you should have a fully functional "Bash on Ubuntu on Windows" application. Woot!

## A Little System Prep
This will update all of your packages, then install git, along with some other tools.
{% highlight bash %}
sudo apt-get update && apt-get upgrade
sudo apt-get install build-essential git-core curl
{% endhighlight %}

## Install Ruby 2.3.1
The system may ask for your password multiple times when RVM wants to start installing packages

Prerequisites:
{% highlight bash %}
sudo apt-get install libgdbm-dev libncurses5-dev automake libtool bison libffi-dev
{% endhighlight %}
GPG Key:
{% highlight bash %}
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
{% endhighlight %}
Use curl to get pull down RVM
{% highlight bash %}
curl -sSL https://get.rvm.io | bash -s stable
{% endhighlight %}
Load RVM on login with .bashrc
{% highlight bash %}
echo " " >> ~/.bashrc
echo "# This loads RVM into a shell session." >> ~/.bashrc
echo ". ~/.rvm/scripts/rvm" >> ~/.bashrc
{% endhighlight %}
Re-source .bashrc
{% highlight bash %}
source ~/.bashrc
{% endhighlight %}
Install Ruby via RVM
{% highlight bash %}
rvm install 2.3.1
{% endhighlight %}
Tell RVM to use 2.3.1 by default. Use ruby -v to verify.
{% highlight bash %}
rvm use 2.3.1 --default
ruby -v
{% endhighlight %}

After that is done you can install Bundler
{% highlight bash %}
gem install bundler
{% endhighlight %}

And finally:

## Install Jekyll
{% highlight bash %}
gem install jekyll
{% endhighlight %}

>*Note:* Please keep case in mind with the gem commands. gem install Jekyll -ne gem install jekyll

And that's it!
You're officially running *Jekyll* on *Ruby* on *Bash* on *Ubuntu* on *Windows*.

Bet you never thought THIS day would come! Welcome to the new Microsoft. And I think the party is just getting started.

>**Note:** The RVM section comes from this document: [Rails Setup Doc](https://gorails.com/setup/ubuntu/16.04). Visit it for more helpful hints; like how to set up SSH access to GitHub via certificate.

## Troubleshooting
Issues:

* **ArgumentError: parent directory is world writable but not sticky:**
    * After installing Ruby via RVM and then Bundler, you may get an error when running <code>bundle install</code>
    * To resolve the issue, run this from the BASH prompt: <code>find ~/.bundle/cache -type d -exec chmod 0755 {} +</code>
* **jekyll 3.0.1 Error:  Invalid argument - Failed to watch**
    * You will get this if you try to run <code>jekyll serve</code> by itself. This is due to the fact that filesystem watchers and inotify support are currently not implemented (but they are on their way!). Check: https://github.com/Microsoft/BashOnWindows/issues/216 for more info.
    * To resolve this issue run <code> jekyll serve -w --force_polling</code>
Please note the underscore.


Till next time!