---
layout: post
title: "Install Ansible On Windows 10"
description: "Ansible on Python on Bash on Ubuntu on Windows"
date: 2017-01-12 18:00:00
comments: true
keywords: "Ansible, Windows, Python, Bash, Ubuntu"
category: Linux on Windows
tags:
- windows
- linux
- ansible
- python
- ubuntu
- bash
---

> Note: This assumes you already have Bash on Ubuntu on Windows enabled.

## Preface

*This is meant to be used in a test/dev situation.* In most [Ansible](https://www.ansible.com/) tutorials/guides 
I have seen, creating a linux distro VM as your test control server is a standard part of the process for people 
who have a Windows workstation. This doesn't have to be the case if you are running Windows 10. 
**This is not meant for production use.**

Happy Test/Dev-ing!

## Installing Ansible 2.2+
This will update all of your packages, add the ansible PPA repository, and install ansible
{% highlight bash %}
$ sudo apt-get update
$ sudo apt-get install software-properties-common
$ sudo apt-add-repository ppa:ansible/ansible
$ sudo apt-get update
$ sudo apt-get install ansible
{% endhighlight %}

You should see something like this:

![Apt-Get Install Ansible](/assets/images/ansible_windows/apt-install-ansible.png){: .center-image }

### Additional Requirements for Managing Windows Nodes
    You may skip this if you are not managing Windows nodes.

{% highlight bash %}
$ sudo apt-get install python-pip
$ sudo pip install "pywinrm>=0.1.1"
{% endhighlight %}

**That's it!**

You've officially installed Ansible 2.2+ on Windows. 

## Quick Test

### Add your host to the default hosts file. 
I happened to have a Vagrant Windows test box. To test that Ansible is working I did this:

Run:

{% highlight bash %}
$ sudo vim /etc/ansible/hosts
{% endhighlight %}

Add:

    [windows]
    192.168.57.3

Save and quit (:wq)

### Set Up Group_Vars

Run:

{% highlight bash %}
$ sudo mkdir /etc/ansible/group_vars
$ sudo vim /etc/ansible/group_vars/windows.yml
{% endhighlight %}

Add:
    
    ansible_user: vagrant
    ansible_password: vagrant
    ansible_port: 5985
    ansible_connection: winrm
    ansible_winrm_scheme: http
    ansible_winrm_cert_validation: ignore

### Test Connectivity

Run:

{% highlight bash %}
$ ansible windows -m win_ping
{% endhighlight %}

Successful Response:

    192.168.57.3 | SUCCESS => {
        "changed": false,
        "ping": "pong"
    }

## Another Tool In The Box

I don't know about anyone else, but I'm really beginning to enjoy the benefits of the Ubuntu subsystem on Windows.
Being able to run Jekyll for GitHub Pages, and now having Ansible directly on my machine... It's so. damn. awesome.
Truly the best of both worlds at your fingertips. Keep it coming Microsoft!

Cheers everyone! Hope you're having a happy start to your new year! Feel free to hit me up on twitter anytime.

-Joshua