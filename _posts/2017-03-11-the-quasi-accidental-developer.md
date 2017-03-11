---
layout: post
title: "The Quasi-Accidental Developer"
description: "A story of falling into development from an ops guy"
date: 2017-03-11 11:00:00
comments: true
keywords: "PowerShell, Operations, Development, DevOps"
category: Development
tags:
- operations
- development
- devops
- powershell
- pester
---

## I'm Not A Developer!

You know, it's funny. I've been writing PowerShell for over 5 years now, but I would've never
considered myself a developer; I might've chuckled at anyone who so much as hinted at the idea. 
That word had to be reserved for the people who buried themselves in languages like c++ or java. 
They were the ones burning the midnight oil in earnest as they endeavored to craft the next 
"best" game or application of all time, carving out a niche of digital history with the artistic 
prowess of Donatello on a keyboard.

Yah, that's a little dramaticized, I know. But a lot of the time we define things in extremes, 
especially when it comes to ourselves. We look at the most visible programmers we see out in 
the world and think, "That's definitely not me. I don't fit the criteria for that designation."

## A Subtle Shift In Reality

I may have originally gone to school for Computer Science, but I've been a straight Ops guy going on 
9 years now... Or so I thought. It hit me recently that I might have quasi-accidentally crossed a threshold
into the realm of development; as seen through the eyes of "normal" Ops guys anyway.

The thing is, it didn't happen all at once. There was no "big bang". No confetti. No banner across the sky
that said "Welcome to the Developer's Club!" Unless of course you count the constant glazing over of your
colleagues eyes when you start talking about code.

For me, it happened organically out of a desire to make things more efficient; to do less repetitive work.
And once managers and teammates started seeing the benefits, they began requesting me to develop more. 
I was constantly improving my techniques and my code, trying to make everything run better. I loved it. 
I still do. It became a running joke to say "I bet Josh has a script for that."

## Scripters -ne Developers

Script. That's somehow considered a "dirty" word not worthy of any recognition. At one time, by someone
I worked with, I was subjected to a monologue about the difference between programmers and scripters. In short
summary, scripters were just wannabe hacks writing pokey little lines of code. Programmers were the real deal.
Something in me didn't agree with it, but I let it go because I was a little insecure and thought maybe he was right.

**He was wrong.**

Sure, there are degrees of proficiency, just as with anything. And there are different areas of expertise. There are 
game developers, desktop app developers, web app developers, mobile app developers...the list goes on. Somewhere in 
there lies a somewhat un-named and unused title that we'll just call "infrastructure developer".

A loose definition might be: One who automates the deployment, configuration, monitoring, maintenance, etc of any 
technical infrastructure such as AWS, Azure, Windows, Linux, Cisco UCS, VMware.. and on.. and on.

If you've been keeping up with the world, this might sound strangely similar to what people are calling "DevOps
Engineers". I don't agree with that title either, but that's a topic for another day.

## Realizing How Far I'd Fallen Down The Rabbit Hole

It wasn't until recently that I noticed I had crossed some imaginary line in the Wonderland of infrastructure
development.

First, I'm currently the only PowerShell guy on my team. I use TFS and Jenkins in a CI/CD pipeline to deliver 
PowerShell modules etc. 

If you have some colleagues that dabble in PowerShell, have you ever tried telling them to write modules instead 
of just scripts? Or asking them to use [CmdletBinding()] so that switches like -Verbose can be utilized? And the
big one: How about requiring that they write Pester unit tests for their code?

In my experience, this usually results in a deer in the headlights look followed by complete and utter silence.

**They think you're crazy**. They think you should be sent straight to the looney developer bin. Do not pass go. Do not
collect 200 dollars. And leave your Systems Engineer badge at the door. You don't belong here anymore.

That's a mild exaggeration of course. To be fair, everyone seems to love reaping the benefits of what I do. 
It's just that, when they're offered the red pill or the blue pill, they run screaming instead.

Fortunately for me, I love every bit of it. I love the infrastructure. I love the code. I love writing code for
the infrastructure. I am an infrastructure developer. It's what I do. 

## Defining A Developer

I saw a question on Twitter a while back during one of [@ThePracticalDev's](https://twitter.com/ThePracticalDev) 
\#DevDiscuss hours: How do you know when you're a developer?

In my opinion, it's simple: Have you ever written code that has been used, especially by someone else, for a specific
purpose? Guess what, you're a developer. But if you write 10 lines of code do you magically become Johnny the Wonder
Developer?

![Johnny the Wonder Developer](/assets/images/accidental_developer/wonderdev.jpg){: .center-image }

Probably not. But it's a start. The point is, developer shouldn't be this big magic word people are afraid of.
It's not an appointment you get from on high. You don't have to have done unit tests or CI/CD pipelines, or any 
myriad of other popular practices, to consider yourself a developer. If you continue writing code you'll probably 
want to get there, but the usage of tools isn't what defines a dev. Code does. So, if you write code, own it. 

And welcome to the party.







