---
layout: post
title:  "Solve Crashplan stopping and starting problems on a Synology NAS"
date:   2015-08-31 15:19:03
categories: 
---
My headless Crashplan installation has been running smoothly for almost a year now, quietly working in the background to backup everything on my NAS to the Crashplan servers. There have been a couple of bumps in the road (usually when Code42 push out an update package or when Synology updates the DSM software), but solutions are figured out very quickly by some in the community, before an official fix is released to get everything back on track again.

Recently my Crashplan started acting up and did not want to backup anything (quite a problem when you're only 50% of the way through the initial backup). The log showed the service constantly stopping and starting, and I was not able to find the solution as easily this time, so I thought it might be worth posting here on the off chance it could help somebody out.

![](https://raw.githubusercontent.com/skydusk/skydusk.github.io/master/assets/stop%20start%20log.png)

If your log looks similar to mine above then its likely Crashplan needs more memory allocated to it (this is especially necessary if your backup consists of several large files and spans a few terabytes or more).

Depending on how much memory your NAS has available, you need to change `1024` to `2048` in the following places (a larger value of `4096` did not work so I settled for `2048`). You can edit these files by SSHing into your NAS using [WinSCP](https://winscp.net/eng/index.php). 

Ensure the Crashplan service is stopped before doing this.

- `/volume1/@appstore/CrashPlan/bin/runconf` change the value in both lines
- `/volume1/@appstore/CrashPlan/synopackage.vars` be sure to remove the `#` to uncomment the line or it won't work

Restart the Crashplan package for the changes to take effect, and the backup should resume normally.


## Worthwhile places to bookmark##
- **[pcloadletter.co.uk ](http://pcloadletter.co.uk/2012/01/30/crashplan-syno-package/)**- patters is the creator of the Crashplan package and the comments section is helpful with how to solve various problems
- **[chrisnelson.ca ](http://chrisnelson.ca/?s=crashplan&searchsubmit=)**- usually has a workaround posted whenever Crashplan breaks
- **[forums.synology.com ](https://forum.synology.com/enu/)**- there is bound to be a Crashplan topic with someone experiencing the same problems, and if you're lucky a solution might be present
