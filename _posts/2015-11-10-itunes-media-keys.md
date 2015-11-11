---
layout: post
title:  "iTunes 12 and Media Keys"
date:   2015-11-10 23:51:03
categories: 
---
iTunes has been my music player of choice for a long time now. I've never found it to be bloated (usually had enough system RAM to cover it), and since I had an iPod, and now an iPhone with iTunes Match, it's always made sense.

One feature I love is using the media keys on my keyboard to navigate through music, this works when iTunes is in the background and allows for playing, pausing and skipping songs with one button press without even bringing up the iTunes window. However, since iTunes 12 this nifty feature stopped working altogether, I initially layed the blame at my keyboard and the SetPoint software, but it turns out the issue was with Google.

**How to restore media key functionality for iTunes 12**

1. In Chrome head to `more tools > extensions` and scroll to the bottom for `keyboard shortcuts`
2. Ensure no extensions have hijacked your media keys (in my case it was Google Play Music)
3. In iTunes go to `edit > preferences > advanced` & uncheck `enable full keyboard navigation` 
4. Go back and recheck the `enable full keyboard navigation` box and then close iTunes
5. Ensure the program is not set to 'run as an administrator' and then relaunch iTunes

And voilà, your media keys should work with iTunes 12 just like before.
