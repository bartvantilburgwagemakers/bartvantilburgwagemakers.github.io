---
title: Listing assembly versions of a project 
layout: post
date:   2019-04-19 08:00:01 -0600
categories: 
tags: 
---
<sup><sup>written at {{date}} </sup></sup>

# {{title}}

Data generated in LinqPad using:

```
    "static readonly string[] ignorePrefixes = new[]{""CMG"", ""cmost"", ""CoFlow"", ""Drms""};
    void Main()
    {
        foreach(var dll in Directory.GetFiles(@""c:\Apps\CoFlow-Win-x64-18.0.4.41441-102259+R18_41441-release\Release"", ""*.dll""))
        {
            var fn = Path.GetFileNameWithoutExtension(dll);
            if (ignorePrefixes.Any(p => fn.StartsWith(p)))
            continue;
            Dump(dll);
        }
    }

    void Dump(string dll) {
    try {
        var an = AssemblyName.GetAssemblyName(dll);
        Console.WriteLine(""{0};{1};{2}"", Path.GetFileName(dll), an.Version, an);
    } catch{}
    }"
```