---
layout: post
title: "From PowerShell to C#"
description: "A new developer's story on the road from PowerShell to C#"
date: 2018-05-10 20:00:00
comments: true
keywords: "PowerShell, Automation, CSharp, C#"
category: Development
tags:
- automation
- powershell
- csharp
- C#
---

## Intro
It has been over a year since I last wrote a blog post. Seems strange to think of where I was then, compared to where I am now. 

- I turned 31.
- Two of my kids are edging closer to double digits in age. 
- My youngest is a year closer to starting school, which will lead to a very quiet house. 
- And everything about my career has changed. Well.. not *everything*.

## Break;
Last year I wrote a post about becoming more of developer who managed operations with code. I was getting into the groove of creating real tools instead of simple scripts. 

I fell in love with CICD and had working pipelines using Jenkins, Pester, and PSDeploy. Managed Windows environments with server-based PowerShell DSC profiles and automated builds... man that was cool. I blogged about PowerShell, UCS, and UCS Director. I even got brave enough to put in a pull request here and there on Github.

And then I stopped. 

For about a year I've been completely dark. Little to no Twitter, no blog posts, no community involvement, and no meetup groups. Not that I expect a mass of people to have noticed, it's just what happened.

Why? For the first time I had a title with the word "Developer" in it. I spent a better part of the first year learning how NOT to do things in object-oriented programming.

## The PowerShell Factor
As I said before, I came from a world where I was (for many clients):

- Managing Cisco UCS 
- Windows environments
- UCS Director Portal

I was doing as much as I could with code, but being a writer of code was my choice, not my responsibility. And it was put to me, in no uncertain terms, that I was a "one off" doing things that the company had no intention to invest in.

Now, I said that writing code was a choice, but I guarantee that I would've lost my mind without it. I owe my sanity to PowerShell.

> && Probably Even My New Job
> **Okay. Definitely my new job.**

- **Without PowerShell:** I wouldn't have done Pester. 

Syntax being irrelevant, the concept of unit testing and mocking is not an easy one to grasp. It can take a moment to wrap your head around. But once you understand, you can apply that to almost any testing framework. The only thing you have to relearn is syntax.

- **Without PowerShell:** If someone asked me about a CICD pipeline, I would've said "I'm sorry, I don't work in oil and gas". 

Around the time I was learning PowerShell "DevOps" was starting to catch on in the community. I read The Phoenix Project and I fell in love. I wanted to DevOps all the things. The world of single button builds was at my fingertips. And it was awesome. It would've been more awesome if I'd been able to rub a little DevOps on some other people around me without them wiping it off, but that's part of the reason I'm no longer there... and thus irrelevant.

- **Without PowerShell:** I would've had a harder time grasping classes and inheritance. 

To be honest, when PowerShell v5 released with classes I thought, "Why? What could I possibly need that for?" Turns out it came in quite handy for doing things a little more... "method"-ically. Hah. Figuring out that I could create a base class, then inherit and override methods in a child class was kicka**. 

I already understood variables, loops, and branching when I started playing with PowerShell. If I hadn't, those would've made the list as well. I'm sure I've learned other concepts from PowerShell that've helped, but I can't think of any more at the moment.

In light of that, thank you Jeffrey Snover, et al. Without your creation I'd be straight up that creek people tend to go without paddles. 

## The Switch()
To be fair, there's still a bit of a gap. PowerShell doesn't *really* prep you for thinking in an OOP way, despite its paradigm of making everything an object with properties. You don't learn about interfaces, or composition, or abstractions vs indirections. You might learn a tiny bit about inheritance and classes, if you have an occasion to dive that deep. But when you take your first glance at a C# class that implements two interfaces and uses a generic type with a "where" interface constraint, you may start sweating a little bit.

And that's okay. If PowerShell focused on all of that, it wouldn't have the same accessability. It'd just be...PowerC#. I'm sure a vast majority of users ignore classes and inheritance features altogether. PowerShell, in its current state, is the best automation and management tool Windows has seen... Ever. But if you want to take it one step further, it's also a great springboard into C# development.

I did, and it's one of the most exciting decisions I've ever made.

> **Note:** I'm not suggesting you should learn PowerShell before C#, but if you find yourself heavily invested in PowerShell and you really enjoy it at a deeper level than just creating one-line scripts... you shouldn't be afraid to take that next step, even if it's for the fun / experience.

## Return;
I've been head-down learning, consuming every little piece of knowledge I can to become a better developer; primarily in the realm of Asp/.Net Standard/Core, OOP principles, design patterns... Even dabbled in a little Python and some JS frameworks. Now seems like a good time to come back. You know, because now I know **EVERYTHING**!

... Ummm... Just kidding. I know a fraction of a quarter of a percent of everything. Who knows, maybe my perspective will be a light in the dark for other people who are afraid they may get eaten by a grue.

> **Note:** In case it wasn't obvious, I definitely am that big of a nerd.

Everything I put out there is going to be from the perspective of the new kid on the block.. bring on all the comments about how I'm doing it wrong! I don't mind. Tell me why and how you would do it and we'll all be the better for it.

You may see a few changes around the site as I get everything back together. As always, thanks for reading. As always, feel free to hit me up [@FooBartn](https://twitter.com/foobartn) or leave a comment below.

Take Care and Happy Coding!



