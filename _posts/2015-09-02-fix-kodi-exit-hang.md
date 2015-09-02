---
layout: post
title:  "Fix Kodi's (XBMC) exit hang problem"
date:   2015-09-02 20:23:03
categories: 
---
For a while now Kodi has not been shutting down properly for me. This problem arose after upgrading XBMC to Kodi and installing several live TV add-ons. I haven't been able to locate what's causing Kodi to hang upon exit, and frankly can't be bothered with disabling add-ons trying to pinpoint the problematic one, so here is a quick and dirty fix that allows for Kodi to shut down clean and fast.

1. Navigate to your Kodi add-ons installation folder `C:\Program Files x86\Kodi\addons`
2. Find the folder for the skin you are using (default skin is Confluence) located in `skin.confluence`
3. In subfolder `720p` open the `DialogButtonMenu.xml` file and search for the word `Quit`
4. Change the line as follows
`<onclick>Quit()</onclick>` to `<onclick>System.Exec ("taskkill.exe /im Kodi.exe /f")</onclick>`

Kodi should now exit almost instantly as it's supposed to (and did before). Although this doesn't correct the underlying issue causing Kodi to hang after exit, it solves the problem for the time being, and thats good enough for me!

**Note:** If you are using a custom theme then it could have installed itself to a different location than the addons folder. If your skin folder is not present in `C:\Program Files x86\Kodi\addons` try looking in `C:\Users\Your Name\AppData\Roaming\Kodi\addons`

[Source](https://www.youtube.com/watch?v=6q-FRBoKWBE)
