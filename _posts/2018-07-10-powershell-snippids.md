---
title: Powershell snippets
layout: post
date:   2018-07-10 08:00:01 -0600
categories: scripting
tags: powershell
---
# {{title}}

Powershell is one of the most overlooked windows apps for ops. This approach doesn’t have any extra features but can be perfect for opening a quick commandlet window and keeping an eye on the status of a file.
Use the following simple syntax to show the tail end of a log file in real-time.
Get-Content myTestLog.log –Wait
You can also filter the log right at the command line using regular expressions:

use `$` to prefix var's;

Get-ChildItem alias gci

-WhatIf show effect

Unblock-Files in folder 

Gci | Unblock-File un blocks all files in folder

switch parameter use a : to give the to a method

```powershell
Get-Content myTestLog.log -wait | where { $_ -match “WARNING” }
```

Set global var from function:
`$global:name`
Else your only adjusting the scoped var.

## making a post request

```powershell
Invoke-WebRequest -Uri $url -OutFile $file -Headers $Headers -Method POST -Body $body 
```

## search in files

Example to search all .config files for log and group them by file path.

```powershell
Get-ChildItem -recurse -Filter *.config | Select-String -pattern "log" | group path | select Path
```

## add a profile

In `C:\Users\[username]\Documents\WindowsPowerShell` you can add a file profile.ps1 that's loaded when opening a new session.
So here you can Set-Alias to often used functions.