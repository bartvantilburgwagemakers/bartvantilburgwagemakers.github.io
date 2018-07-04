---
title: Procesmangement in .Net
layout: post
date:   2018-07-03 20:00:01 -0600
categories: c#, .Net, windows
tags: c# .net win
---
# {{title}}

## Get the username of a proces

Apparently i don't get a value when using Process.StartInfo.UserName. Why i have to look up?
But this also helps.

```csharp

        private string GetProcessOwner(int processId)
        {
            var query = "Select * From Win32_Process Where ProcessID = " + processId;
            ManagementObjectCollection processList;
            using (var searcher = new ManagementObjectSearcher(query))
            {
                processList = searcher.Get();
            }

            foreach (var o in processList)
            {
                var obj = (ManagementObject)o;
                object[] argList = { string.Empty, string.Empty };
                var returnVal = Convert.ToInt32(obj.InvokeMethod("GetOwner", argList), CultureInfo.InvariantCulture);
                if (returnVal == 0)
                {
                    var domain = argList[1];
                    var user = argList[0];
                    return string.Format(CultureInfo.InvariantCulture, "{0}\\{1}", domain, user);
                }
            }

            return "NO OWNER";
        }
```

## isResponsive

