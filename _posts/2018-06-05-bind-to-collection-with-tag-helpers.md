---
layout: post
title: "Bind to Collection with Tag Helpers"
description: "Using tag helpers to easily bind data to a list property of a model"
date: 2018-06-05 21:00:00
comments: true
keywords: "Asp.Net Core, .Net Core, dotnet core, CSharp, C#, Razor, Tag Helpers"
category: Development
tags:
- razor
- asp.net core
- csharp
- C#
---

## Short and Sweet

I'm *that* guy that usually sends long emails, tells long stories, etc... But I'm working on that. So in the spirit of briskness: A short on using tag helpers to bind to a collection property on a model!

## Code being used

Our model:
``` csharp
namespace BindToList.ViewModels
{
    public class FormViewModel
    {
        public IList<string> Name { get; set; }
    }
}
```

Our form:
``` html
@model FormViewModel

<form asp-controller="Home" asp-action="Form" method="get">
    @for (int i = 0; i < 3; i++)
    {
        <label>Name@(i + 1)</label><input asp-for="Name[i]" /> <br />
    }
    <button type="submit">Register</button>
</form>
```

Our controller:
``` csharp
public IActionResult Form(FormViewModel model)
{
    return View(model);
}
```

## In Action

![](/assets/images/taghelper_list/bindtolist.gif){: .center-image }

## And Voila

The controller sees the values and reads them in as members of the same collection.

## Notes on the Code

### Form CSHTML

The @ symbol means that what comes after it should be expressed in C#, not output to the renderer as html. If you use HTML tags inside the expression, it will evaluate what's inside those as html/text instead. I didn't want it to ignore the number after the name, though, so I had to use an @() to make sure Razor evaluated that piece as an expression, not as text.

The actual HTML it will output:

``` html
<form method="get" action="/Home/Form">
    <label>Name1</label><input type="text" id="Name_0_" name="Name[0]" value="" /> <br />
    <label>Name2</label><input type="text" id="Name_1_" name="Name[1]" value="" /> <br />
    <label>Name3</label><input type="text" id="Name_2_" name="Name[2]" value="" /> <br />
    <button type="submit">Register</button>
</form>
```

>*IMPORTANT NOTE:* You MUST start at 0 or the model will not bind.

### Controller

It's not doing anything but sending the data you put in back to you. You hit submit, it gets the model data, and then returns the view with the data you sent in. Normally you'd want to check ModelState, etc... But I need to stop here otherwise this might just keep going forever ^__^

### Model

I used an IList, but you can use another collection like IEnumerable if it suits your purpose.

## Cheers

And that's it! Short and simple, but hopefully useful.

Have a happy weekend! 