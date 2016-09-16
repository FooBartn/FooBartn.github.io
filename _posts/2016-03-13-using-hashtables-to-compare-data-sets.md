---
layout: post
title:  "Using Hashtables to Compare Data Sets"
description: "Utilizing hashtables, otherwise known as key value pairs or dictionaries, to compare sets of data"
date:   2016-03-13 19:14:00
keywords: "PowerShell, hashtable, key, value, dictionary, data"
comments: true
category: PowerShell
tags:
- powershell
- data
- hashtable
---

Hey Everyone. While I was working on my [vCheck for UCS](https://github.com/FooBartn/vCheck-UCS/tree/dev) project I decided this method of comparing 
expected object data to actual object data might be a good share. Many of you may already know and use this, but perhaps someone will find the information 
useful in their journey through PowerShell.

# Problem
You are checking on the values of an object. You want to know if ANY the values don't match your expected output.

In our case we're going to look at $PsVersionTable, just because it's something you can all test on any machine. The output of $PsVersionTable should 
look something like this:

{% highlight powershell %}
PS D:\> $PSVersionTable

Name                           Value
----                           -----
PSVersion                      5.0.10586.122
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.10586.122
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
{% endhighlight %}

Alright, let's set some expected values. We'll assume you expect the Major PSVersion to be 4, Build version to be 8, and WSManStackVersion to be 3. 
(Not necessarily a real world situation).

# Possible Solution
You could use If..Then statements, right?

{% highlight powershell %}
If ($PsVersionTable.PSVersion.Major -ne 4) {
    Write-Output "Actual PSVersion is: $($PsVersionTable.PSVersion)"
}
If ($PsVersionTable.BuildVersion.Major -ne 8) {
    Write-Output "Actual BuildVersion is: $($PsVersionTable.BuildVersion)"
}
If ($PsVersionTable.WSManStackVersion.Major -ne 3) {
    Write-Output "Actual WSManStackVersion is: $($PsVersionTable.WSManStackVersion)"
}
{% endhighlight %}

**Results:**
{% highlight powershell %}
Actual BuildVersion is: 10.0.10586.122
Actual PSVersion is: 5.0.10586.122
{% endhighlight %}
But then you're writing an IF statement for every property. It may not be *that* big of a deal with only a couple values, but it's quickly going to get out of 
hand and end up being really hard to read. Not to mention it's already looking very redundant, and redundancy != efficieny in scripting.

# Better Solution
What if we used a hash table? We could use a loop to iterate through the keys for comparison.
First we'll create a hash table, which is really an array of key-->value pairs. This is important to know but the keys are not variables but **values**.

So let's create a hash table for our expected values:
{% highlight powershell %}
$ExpectedPsVersionHash = @{
    PSVersion = 4
    BuildVersion = 8
    WSManStackVersion = 3
}
{% endhighlight %}

> Notice the use of braces as opposed to paranthesis. This is how you create hash tables vs simple data arrays.

Call that variable and the output should look exactly like this:

PS D:\> $ExpectedPsVersionHash
{% highlight powershell %}
Name                           Value
----                           -----
BuildVersion                   8
PSVersion                      4
WSManStackVersion              3
{% endhighlight %}

The really important part here is the first column. Remember I state that these are Key-->Value pairs? Well, these are the "Keys". You might expect to get them by using
{% highlight powershell %}
$ExpectedPsVersionHash.Name
{% endhighlight %}
That will not work. They are **keys**, and so that is how they referred to:
{% highlight powershell %}
PS D:\> $ExpectedPsVersionHash.Keys
BuildVersion
PSVersion
WSManStackVersion
{% endhighlight %}

And how do we get the value of a key? Easy! For PSVersion it would be:
{% highlight powershell %}
PS D:\> $ExpectedPsVersionHash.PSVersion
4
{% endhighlight %}

Now we have our keys.. we have our expected values.. How do we compare them to actual data?
Through a loop of course!

{% highlight powershell %}
Foreach ($Key in $ExpectedPsVersionHash.Keys) {
    If ($PsVersionTable.$Key -ne $ExpectedPsVersionHash.$Key) {
        Write-Output "Actual $Key is: $($PsVersionTable.$Key)"
    }
}
{% endhighlight %}

We use the keys for loop iteration, and, because the key names in the $ExpectedPsVersionHash are identical to the property names of the $PsVersionTable object, 
we can use each $Key to compare both data sets.

Whole Script:
{% highlight powershell %}
$ExpectedPsVersionHash = @{
    PSVersion = 4
    BuildVersion = 8
    WSManStackVersion = 3
}

Foreach ($Key in $ExpectedPsVersionHash.Keys) {
    If ($PsVersionTable.$Key.Major -ne $ExpectedPsVersionHash.$Key) {
        Write-Output "Actual $Key is: $($PsVersionTable.$Key)"
    }
}
{% endhighlight %}

**Results:**
{% highlight powershell %}
Actual BuildVersion is: 10.0.10586.122
Actual PSVersion is: 5.0.10586.122
{% endhighlight %}
And that's why hash tables are awesome!

If you've come this far, then I truly hope this mini-lesson has done you some good. If it has then please let me know! Or even if not -- if you have other thoughts 
you'd like to share I am always open to any feedback.

Until next time.. **Keep PowerShell'n!**