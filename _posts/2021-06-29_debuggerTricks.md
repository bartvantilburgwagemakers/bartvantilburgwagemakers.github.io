---
title: Debugger Tricks
layout: post
date:   2019-01-01 08:00:01 -0600
categories: 
tags: Debugger C# VisualStudio
---
<sup><sup>written at {{date}} </sup></sup>

# {{title}}  

If yhou wan't to set a breakpoint by code but only if you have a debugger attached you can use this snippet: 

```csharp

if(System.Diagnostics.Debugger.IsAttached)
{
    System.Diagnostics.Debugger.Break();
}
```

Or `System.Diagnostics.Debugger.Launch()` can be used to start up a debugger and attach it