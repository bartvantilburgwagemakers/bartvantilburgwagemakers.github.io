---
title: Change AppConfig values with PowerShell
layout: post
date:   2019-01-01 08:00:01 -0600
categories: scripting
tags: powershell AppConfig
---
<sup>written at {{date}} </sup>

# {{title}}  

To update a webconfig or a app config file.
We can use the following scrip in powershell:

```ps
$ConfigFile = 'C:\Apps\demo\demo.config'
## XML Node names and Attributes are CaSe SeNsItIvE!
$XPath = "appSettings/add[@key='connectionString']"
$NewValue = "TheOtherConnectionString"

$xml = [xml](Get-Content -Path $ConfigFile)
$xml.SelectSingleNode($XPath).SetAttribute("value", $NewValue)
$xml.Save($ConfigFile)
```

This will use Xpath syntax to go to the correct xml tag. [More info about that can be found here](https://www.w3schools.com/xml/xml_xpath.asp)