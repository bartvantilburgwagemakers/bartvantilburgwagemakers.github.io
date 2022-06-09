---
title: Powershell snippets
layout: post
date:   2018-07-10 08:00:01 -0600
categories: scripting
tags: powershell
---
# {{title}}

- [{{title}}](#title)
  - [making a post request](#making-a-post-request)
  - [search in files](#search-in-files)
  - [add a profile](#add-a-profile)
  - [Change config Files](#change-config-files)
  - [Set environment vars](#set-environment-vars)
  - [Search in files](#search-in-files-1)

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

## Change config Files

```ps
    $ConfigFile = 'C:\Apps\example.exe.config'
    ## XML Node names and Attributes are CaSe SeNsItIvE!
    $addinLocation = "C:\Apps\Addins\"
    $XPath = "configuration/PluginSearchContext/add[@Location='$addinLocation']"
    $Attribute = "Location"

    $xml = [xml](Get-Content -Path $ConfigFile)
    $node = $xml.SelectSingleNode($XPath)
    If($node -eq $null){

        $child = $xml.CreateElement("add")
        $node = $xml.SelectSingleNode("configuration/PluginSearchPaths")

        AddIdAtribute
        $node.AppendChild($child)
        $xml.Save($ConfigFile)
    }

    function AddIdAtribute (){
        $AddNodes =$node.SelectNodes("add")
        $child.SetAttribute('Id', $AddNodes.Count+1)
        $child.SetAttribute($Attribute, $addinLocation)
    }
```

## Set environment vars

You can set a env var with the following line:

`[environment]::SetEnvironmentVariable("name","value", "User")`

And use `Machine` if it's for the machine level.

Set register key `Set-Itemproperty`

Disable quickEdit on console `Set-Itemproperty -path 'HKCU:\Console' -Name  QuickEdit -Value 0`

## Search in files


`Get-ChildItem -Exclude *.dll,*.exe,*pdb,*svg,*.csv,*.pdf  -Recurse | Select-String -Pattern "" -List  >LookInToo.txt`

## Search in event log

Replace `Print Spooler service` by the name of parts of it.

```
(Get-EventLog -LogName "System" -Source "Service Control Manager" -EntryType "Information" -Message "*Print Spooler service*running*" -Newest 1).TimeGenerated
```




## Tail watch end of file 
```powershell
Get-Content filename.log -Wait
```

## Recursively find files whose content matches a regex pattern and display the first 10 lines for context:

```powerahell
Get-ChildItem .\*.txt -Recurse | Select-String -Pattern 'MyPattern' -context 10,0
```
