---
layout: post
title: "UCS Director and PowerShell"
description: "A power couple that also has a few marital issues"
date: 2017-03-19 23:00:00
comments: true
keywords: "PowerShell, Automation, Orchestration, UCS Director"
category: Automation
tags:
- automation
- orchestration
- ucs director
- powershell
---

## UCS Director Intro

For those of you that don't know, UCS Director is a [somewhat uncommon] orchestration engine from Cisco. I still think someone in marketing "whoopsed" on this one, in my opinion, by tying it directly to the Cisco Unified Computing System infrastructure. Why would you use it if you don't own Cisco UCS equipment? Well... because it has no dependency on or requirement that you even use Cisco's server hardware.

>"What's in a name? That which we call a rose
>By any other name..." `causes confusion`.

UCS Director can integrate with virtualization platforms, network equipment, storage arrays, server systems and more from a variety of vendors. It can also use PowerShell directly in its workflows, which is the facet we're going to focus on for this article. The only thing Cisco UCSD and Cisco UCS really have in common is the company.

## A "Power"ful Duo

PowerShell is everywhere.

- Cloud (AWS, Azure, Google, etc)
- Server OS (PowerShell for Windows, Mac, Linux)
- Configuration Management (DSC)
- Cisco UCS
- VMware

All of the above have their own PowerShell modules with cmdlets built to help you manage that particular technology. And for technologies without their own PowerShell cmdlets? If they have an API, then you can just write your own using built in cmdlets like Invoke-RestMethod and Invoke-WebRequest!

If you can't tell, I'm a huge fan. And I'd much rather write PowerShell than Cloupia Script to create custom tasks.

> Cloupia Script is a form of javascript used inside UCSD... we'll get into that another day.

Fortunately UCS Director can run PowerShell code on a remote machine, though you will have to install the PowerShell Scripting Agent (PSA) for UCSD somewhere in your environment first. It acts as a sort of proxy that actually runs the code.

This means that anything you could do with PowerShell, you can call from within a UCS Director workflow. That opens up a world of possibilities! Unfortunately, it isn't without some... issues. Though they can be worked around without too much difficulty. Hopefully the following information will help you out on your journey if you're starting to use PowerShell with UCS Director!

## A Quirky Relationship

Issues exist in all software. It's the nature of the thing. What is important to know is that Cisco is constantly working to improve their product. If you can get around these, UCS Director is a fantastic orchestration tool; and Cisco is constantly working to improve it.

> `IMPORTANT NOTE:`
> I'm told many of these issues are being addressed in UCS Director v6.5, which is currently in Beta. If you're going to Cisco Live, be sure to check out the UCS Director session by [Orf](https://twitter.com/ucsdguru)!

### Syntax

There are internal variables you can use; some are global, some are outputs from other tasks, etc. To use them in a PowerShell task, this is the syntax: ${UCSD_Variable}. The fact that these exist is fantastic. It makes using data that pertains to your current workflow so much easier.

The rub is in the syntax. PowerShell and UCSD are a bit at odds with variable identifiers here. Who gets priority? UCSD of course. This means that if you use any PowerShell variables that shouldn't be interpreted by Director, you have to escape them like this:

{% highlight powershell %}
\$Foo = 'Bar'
{% endhighlight %}

Because of this I usually make it a point to write as little PowerShell directly in UCSD as possible. I try to stick to calling scripts that live on the machine with the PSA agent installed.

But this also leads another "quirk" you have to take into account. Any directory on the PSA agent that you want to specify needs to have double '\' marks. You have to escape the escape character.

{% highlight powershell %}
\$Foo = "D:\\\\\\$Bar\\\Directory\\\\"
{% endhighlight %}

> `Note:` Currently the list of known global UCSD variables in the VM Context is not listed in UCSD itself. You can find a list [here](https://github.com/FooBartn/UCS-Director/blob/master/UCSD-Variables.txt), and I plan on creating a post about variables; how to find ones you might not know you have access to, as well as how to create some custom inputs/outputs of your own.

### Security

When running code on a remote machine, you're required to input that machine's IP address. You cannot use an FQDN. So the current communication trail for UCSD looks like this:

UCSD --> PowerShell Agent --> Remote IP

If you are familiar with PowerShell remoting, then you know that trying to do it via IP results in an error. Kerberos authentication does not allow IP addresses, so the authentication will automatically switch to NTLM. NTLM requirements:

- Configure the computer for HTTPS transport or add the IP addresses of the remote computers to the TrustedHosts list on the local computer.

As far as I know there is no way to get the PSA agent to add -UseSSL. So, even if you had an SSL certificate, you're out of luck there. That leaves WinRM TrustedHosts.

If you look at the bottom of the [UCS Director 6.0 PowerShell Guide](http://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/ucs-director/powershell-guide/6-0/b_Cisco_UCS_Director_PowerShell_Agent_Install_Config_60/b_Cisco_UCS_Director_PowerShell_Agent_Install_Config_60_chapter_011.html) you will see that it asks you to add * to the WinRM TrustedHosts of your PSA Server.

**Caution**

WinRM TrustedHosts is not a security model. It's a security suppression model.

From Microsoft:

*Please note that adding a server name to the TrustedHosts list should not be considered as any form of statement of the trustworthiness of the hosts themselves - as the NTLM authentication protocol cannot guarantee that you are in fact connecting to the host you are intending to connect to. Instead, you should consider the TrustedHosts setting to be the list of hosts for which you wish to suppress the error generated by being unable to verify the server's identity.*

If you put * in the TrustedHosts list, you are saying that your server should trust and connect to any remote server without having to verify its identity. Is it worth considering that another machine could impersonate the intended endpoint? That's up to you.

In the case of UCS Director, I currently prefer to only run scripts I keep on the PSA server itself. So I only put its own IP address in the WinRM TrustedHosts list. Yes, you actually have to do that. As of the latest release there is no way to run a PowerShell command natively on the PSA server.

### Error Feedback

When a PowerShell script fails during a workflow, the workflow log will simply show that the script failed. That is it. No explanation as to why.

To get any information pertaining to the fault that was raised, you must go to the PSA Server and check the PSA log file under the `C:\Program Files (x86)\Cisco Systems\Cisco PSA Service\` directory.

There will be a log.txt file and a list of previous log files that have rolled over and been "archived". When you open the log.txt file you will see session connection information. If you just ran a PowerShell command from UCSD and it failed, scroll to the bottom of this log to find the last session. You will see a line with the word ERROR in it. Or you can do what I do:

In my workflows, the "On Failure" action for all of my PowerShell tasks leads them to another PowerShell task. This task calls a script on the PSA server that parses the log for the last error and sends it all back with write-output commands that will show up in the UCSD log. This way you don't have to run to the PSA server every time a PowerShell task fails in Director.

You can find the code I use for that [here](https://github.com/FooBartn/UCS-Director/blob/master/Get-LastPSError.ps1).

## Exemplī Grātiā

Here are a few syntax examples for using PowerShell in UCSD.

- Using Code Directly in UCSD

{% highlight powershell %}
\$ScriptParams = @{
    To = '${SUBMITTER_EMAIL}'
    From = 'ucsdirector@somewhere.com'
    Subject = '${VM_NAME}'
    Body = 'Something about this VM'
    SmtpServer = 'smtp.somewhere.com'
}

Send-MailMessage @ScriptParams
{% endhighlight %}

- Calling Script on the PSA Server

{% highlight powershell %}
\$ScriptPath = 'D:\\Scripts\\New-VMFile.ps1'

\$ScriptParams = @{
    VMName = '${VM_NAME}'
    DCName = '${VM_CLOUD}'
}

Invoke-Expression -Command "\$ScriptPath @ScriptParams"
{% endhighlight %}


## Just Getting Started

I hope to be making a lot more posts about UCS Director, especially in regards to PowerShell integration. This was mainly an intro to the pairing with "things you should know" thrown in. Hopefully I'll get time to work on some more detailed examples and maybe even export a workflow or two. I hear there are some really good things coming in 6.5, so I'll definitely do my best to provide some updated info when that goes public!

As always, feel free to hit me up on Twitter, LinkedIn, etc.

Cheers!
Joshua







