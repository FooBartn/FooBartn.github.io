---
layout: post
title: "B200 M4 Failed Discovery"
description: "B200 M4 failed discovery with message stating that the PciAddress of the Storage Controller was not found"
date: 2015-06-29 18:02:00
comments: true
keywords: "Cisco, UCS, Bug"
category: Troubleshooting
tags:
- cisco
- ucs
- bug
---

Ran into a Cisco UCS bug recently. It has a fairly strict hardware/firmware requirement, which is probably why there wasn't any information when I did a google search for error message.

You need to be running UCSM 2.23d and using a B200 M4 without a RAID controller or HDD installed.

So the scenario is that upon either initial discovery or re-discovery (as it happened to me) it fails with the message "PciAddress of Storage Controller not found"

I initially tried:

* Re-acknowledge
* CIMC Reset
* CMOS Reset

and finally

- Disassociate / Reassociate

All to no avail. Apparently I missed the simple solution: Decommission the blade *before* you re-acknowledge it.

Reference: [Cisco Bug: cscut99437](https://tools.cisco.com/bugsearch/bug/cscut99437)