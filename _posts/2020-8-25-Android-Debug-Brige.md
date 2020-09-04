---
title: Android Debug Bridge (adb) tips
layout: post
date:   2020-08-25 08:00:01 -0600
categories: android
tags: android, adb 
---
<sup><sup>written at {{date}} </sup></sup>

# {{title}}  

## back up all 

`adb backup -f “D:\myfolder\myapp.ab” -all -apk -nosystem`

## restore

`adb.exe restore FullBackup.ab`

## list all apps

`.\adb.exe shell pm list packages`

## copy sdcard 

`.\adb.exe pull /sdcard "C:\Users\Bart\Desktop\motog5\platform-tools\Sabastiaans tablet\sdcard"`