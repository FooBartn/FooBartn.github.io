---
layout: post
title: "Adding Dynamic WPF Content With PowerShell"
description: "Adding a little magic to WPF GUIs with PowerShell, allowing you to change the GUI dynamically after calling ShowUI"
date: 2016-08-21 17:17:00
comments: true
keywords: "PowerShell, WPF, GUI, Dynamic"
category: PowerShell
tags:
- powershell
- gui
- wpf
---

# Hello Again, World
It has been a while. Life has a way of magicking off with your time, especially when you have five little wizards to look after. Feeling: Harry Potter-ish. Can't you tell?

Anywho, I thought we'd play with some GUI stuff today. 

>**Note:** If you need help creating basic GUIs for PowerShell, venture over to this great article by [@FoxDeploy](https://twitter.com/FoxDeploy): https://foxdeploy.com/2015/04/10/part-i-creating-powershell-guis-in-minutes-using-visual-studio-a-new-hope/

# Crafting Our GUI

We will be using this [Base GUI Script](https://gist.github.com/FooBartn/dd62af73ed5703f121ea5688243009cb) as a starting point. Download and follow along!

> **Note:** For those of you who like to read the last page of a novel first, you can download the final script here: [New-AddButtonGUI.ps1](https://gist.github.com/FooBartn/ad3d9b6118c0397a0338ed19858d178d)

Our GUI framework is going to look like this:

- Main Window
  - StackPanel
      - < Where we will add buttons >
  - Button to Add Buttons

And here's the raw XAML code, directly out of Visual Studio 2015, that we're going to be working with:

{% highlight xml %}
<Window x:Class="WpfApplication3.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:WpfApplication3"
        mc:Ignorable="d"
        Title="MainWindow" Height="344.151" Width="232.521" ResizeMode="NoResize">
    <Grid>
        <StackPanel x:Name="StackButtonsHere" HorizontalAlignment="Left" Height="231" Margin="22,10,0,0" VerticalAlignment="Top" Width="170"/>
        <Button x:Name="AddAButton" Content="The Add-A-Button Button" HorizontalAlignment="Left" Margin="22,260,0,0" VerticalAlignment="Top" Width="170"/>
    </Grid>
</Window>
{% endhighlight %}

Place this XAML code in the here string at the top of the base script and you'll be ready for the next step.

# The Un-Enchanted Window
You've plugged the XAML code into your script, so if you run it you should get this: 

![Main Window](/assets/images/dynamic_wpf/MainWindowBase.PNG)

Sweet! A Window! Ooo, and you even have an Add-A-Button button! Which might be cool... if it added buttons.

Close this window and go back to your PowerShell prompt. If you type Get-Variable WPF*, you should get this as a result:


{% highlight powershell %}
Name                           Value
----                           -----
WPFAddAButton                  System.Windows.Controls.Button: The Add-A-Button Button
WPFStackButtonsHere            System.Windows.Controls.StackPanel
{% endhighlight %}
And there we have our button and our stackpanel variables.

So how do we get our muggle of an add-a-button button to actually *and magically* add buttons?

# Events!

No. Not like Quidditch. But I like where your head is at.

System.Windows.Controls namespace objects (buttons, textboxes, etc) can listen for events. Run:
{% highlight powershell %}
$WPFAddAButton | Get-Member -MemberType Event
{% endhighlight %}
So many events! 111 to be exact. We're really only going to focus on one right now: Click!

To make our button "listen" for clicks, we're going to **Add** a click event to our PowerShell code. Like so:
{% highlight powershell %}
$WPFAddAButton.Add_Click({
   #What to do when button is clicked
})
{% endhighlight %}
Now that the button is listening, we can tell it what to do when clicked. First, we need to create a new button object in PowerShell

{% highlight powershell %}
$NewButton = New-Object System.Windows.Controls.Button
{% endhighlight %}

> **Note:** For a list of available System.Windows.Controls classes, visit: https://msdn.microsoft.com/en-us/library/system.windows.controls(v=vs.110).aspx

Then we need to add it to our stackpanel. We do this by calling the AddChild method. Doing so uses the stackpanel as the parent, and adds our new button inside it.

{% highlight powershell %}
$WPFStackButtonsHere.AddChild($NewButton)
{% endhighlight %}

So the whole event looks like this:
{% highlight powershell %}
$WPFAddAButton.Add_Click({
    $NewButton = New-Object System.Windows.Controls.Button
    $WPFStackButtonsHere.AddChild($NewButton)
})
{% endhighlight %}

Let's try it!

![](/assets/images/dynamic_wpf/TinyButtons.PNG)

Oh my.. those buttons are tiny. That just won't do. Perhaps we should consider changing the height of our summoned buttons?

{% highlight powershell %}
$WPFAddAButton.Add_Click({
    $NewButton = New-Object System.Windows.Controls.Button
    $NewButton.Height = 20
    $WPFStackButtonsHere.AddChild($NewButton)
})
{% endhighlight %}
![](/assets/images/dynamic_wpf/BiggerButtons.PNG)

Eureka! Wait.. Blank Buttons. What am I supposed to do with blank buttons?

# Chaining Spells Together
What if I added a text box for naming them? That requires changing up our XAML code a little, so here's the new block:
{% highlight xml %}
<Window x:Class="WpfApplication3.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:WpfApplication3"
        mc:Ignorable="d"
        Title="MainWindow" Height="344.151" Width="232.521" ResizeMode="NoResize">
    <Grid>
        <StackPanel x:Name="StackButtonsHere" HorizontalAlignment="Left" Height="231" Margin="22,10,0,0" VerticalAlignment="Top" Width="170"/>
        <Button x:Name="AddAButton" Content="The Add-A-Button Button" HorizontalAlignment="Left" Margin="22,277,0,0" VerticalAlignment="Top" Width="170"/>
        <TextBox x:Name="ButtonName" HorizontalAlignment="Left" Height="23" Margin="48,249,0,0" TextWrapping="Wrap" Text="Button Name Here" VerticalAlignment="Top" Width="120"/>
    </Grid>
</Window> 
{% endhighlight %}

Run the script, close the window, and let's check our variables again..

{% highlight powershell %}
Get-Variable WPF*

Name                           Value
----                           -----
WPFAddAButton                  System.Windows.Controls.Button: The Add-A-Button Button
WPFButtonName                  System.Windows.Controls.TextBox: Button Name Here
WPFStackButtonsHere            System.Windows.Controls.StackPanel
{% endhighlight %}

Aha! A Button Name Box! Perfect! Let's make the new button's name and content be whatever is in that box. To do so we utilize the Name and Content properties of our $NewButton.

{% highlight powershell %}
$WPFAddAButton.Add_Click({
    $NewButton = New-Object System.Windows.Controls.Button
    $NewButton.Name = $WPFButtonName.Text
    $NewButton.Content = $WPFButtonName.Text
    $NewButton.Height = 20
    $WPFStackButtonsHere.AddChild($NewButton)
})
{% endhighlight %}
> **Specialis Revelio**: One could get the available properties of a button (or any other control) by using: <code>New-Object System.Windows.Controls.Button | Get-Member -MemberType Property</code>

A little flourish... and voila!

![](/assets/images/dynamic_wpf/Gryffindor.PNG)

Merlin's Beard! More muggle buttons! What good are those?

Well, how about we make the text box change to the name of whatever button we pressed?

We know the name of each new button, because it comes from the text box. So we can create a new event nested inside the original Add_Click. We'll have to grab the actual button object we just added to the stack panel. 

{% highlight powershell %}
    # Get the button you just added
    $AddedButton = ($WPFStackButtonsHere.Children | 
    Where-Object {
        $_.Name -eq $WPFButtonName.Text
    })
{% endhighlight %}
Here we're just finding the child object we added to the stackpanel by referencing the name that you used in the text box. Next, we'll need to add an event to that button.
{% highlight powershell %}
    # Add Click Event
    $AddedButton.Add_Click({
        [System.Object]$Sender = $args[0]
        $WPFButtonName.Text = $Sender.Name
    })
{% endhighlight %}
I don't know if you noticed, but there's some serious sorcery [sounds like a good name for something] happening here. The Add_Click event actually passes the control object into the event as $args[0]. So we assigned it as $Sender, and then told it to change the text on the button name box to be whatever $Sender.Name is. This way, whichever button you click, it pulls directly from that button's properties.

# You're a [PowerShell] Wizard!
You've now made a GUI that has dynamically created content, where each new object actually has a function of its own. Might not be the most useful example, but perhaps it will lead to bigger and better things!

![](/assets/images/dynamic_wpf/Finito.PNG)

Thanks for reading, and I wish you the best of luck in your future Wizarding adventures!