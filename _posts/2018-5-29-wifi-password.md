---
title: Show Wifi security key
layout: post
date:   2018-05-29 08:00:01 -0600
categories: Windows
tags: wifi win10
---
# {{title}}

If you want to show a security key for a known wifi network use:

```sh
netsh wlan show profile "network name" key=clear
```

use ```netsh wlan show profile``` to show the list of known wifi networks
