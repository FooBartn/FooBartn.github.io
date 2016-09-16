---
layout: post
title: "Jenkins TFS-Plugin Credential Bug"
description: "Jenkins tfs/git bug that causes NTLM authentication via the service account no matter what credentials you specify"
date: 2016-08-26 15:31:00
comments: true
keywords: "Jenkins, TFS, Git"
category: Troubleshooting
tags:
- tfs
- git
- jenkins
---
>**Update:**
>The issue has been resolved in release 5.2.0

# Issue

So you installed the TFS-Plugin into your Jenkins instance running on Windows Server 2012 R2. You followed [these nice instructions](https://github.com/jenkinsci/tfs-plugin) to create credentials, and add your TFS collection. Easy peezy! Then you clicked Test Connection and... you got this:

![](/assets/images/jenkins_ntlm/tfsautherror.png)

Excuse the black bars. In place of those, in your version of the message, you will most likely see that it isn't trying to use the credentials you specified in the credentials box. It is trying to use the **Local SYSTEM** account that the Jenkins service is running under.

AH! For some reason the plugin seems to *completely ignore anything you put in the credential box*

How Rude!

# Workaround

Change the user your Jenkins service account is running under to a user with access to your TFS collection.

By doing so I was able to specify "- none -" in the credentials box and still access the TFS collection successfully.


>**Bug ID:**  [JENKINS-37710](https://issues.jenkins-ci.org/browse/JENKINS-37710)

