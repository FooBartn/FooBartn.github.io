---
layout: post
title: "LCP VLAN Bug During Firmware Upgrade"
description: "A bug when creating using LAN connectivity policies and initial vNIC templates"
date: 2016-02-20 18:42:00
comments: true
keywords: "Cisco, UCS, Bug, Networking"
category: Troubleshooting
tags:
- cisco
- ucs
- bug
- networking
---

Ran into an interesting bug recently when upgrading firmware from 2.13a to 2.25c. Unfortunately the bug title / description leaves **MUCH** to be desired. 
It's almost as if they tried to put two bugs under one ID.

**Issue:** vNICs are provisioned from a LAN Connectivity Policy whose NICs were created with initial vNIC templates. 
Due to this the VLANs are able to be lost on the LAN Connectivity Policy.

**Detail:** A Lan Connectivity Policy is created using initial vNIC templates. This allows for the VLANs to either be removed from the LCP or, if the 
initial vNIC template is created after the LCP, not to be copied to the LCP correctly. When the LCP is applied to a Service Profile UCSM looks at both the 
LCP **and the initial templates assigned** to create the vNICs. This is an incorrect behavior that allows you the believe it is functioning correctly. 
The next time the SP is modified *at all*, UCSM will delete the VLANs under the Service Profile because they are not under the associated NIC in the LAN Connectivity Policy.

**Resolution:** Either validate that the VLANs exist correctly in your LCP before making any configuration changes to a Service Profile, or, 
perhaps a better solution, do not use initial templates with LAN Connectivity Policies.


> Note: Cisco updated the aforementioned bug ID. It still does not mention that 2.13a is affected, even though they told me it is, but at least they provided a better 
explanation of the issue.

Reference: [Bug ID: CSCut60360](https://tools.cisco.com/bugsearch/bug/CSCut60360)
