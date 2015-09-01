---
layout: post
title:  "Solve Crashplan stopping and starting problems on a Synology NAS"
date:   2015-08-31 15:19:03
categories: 
---
My headless Crashplan installation has been running smoothly for almost a year now, quietly working in the background to backup everything on my NAS to the Crashplan servers. There have been a couple of bumps in the road (usually when Code42 push out an update package or when Synology updates the DSM software), but solutions are figured out very quickly by some in the community, before an official fix is released to get everything back on track again.

Recently though my Crashplan started acting up and did not want to backup anything (quite a problem when you're only 50% of the way through the initial backup). The log showed the service constantly stopping and starting, and I was not able to find the solution as easily this time, so I thought it might be worth posting here on the off chance it could help somebody out.

![](https://raw.githubusercontent.com/skydusk/skydusk.github.io/master/assets/stop%20start%20log.png)

If your log looks similar to mine above then its likely Crashplan needs more memory allocated to it (this is especially necessary if your backup consists of several large files and spans a few terabytes or more).

Depending on how much memory you have available, you need to increase the `1024` value (the Java heap size). I changed it to `3072` as that was the maximum it would accept (a larger value of `4096` did not work for reasons unknown to me). You can edit the necessary files by SSHing into your NAS using [WinSCP](https://winscp.net/eng/index.php).

Ensure the Crashplan service is stopped before doing this.

- `/volume1/@appstore/CrashPlan/bin/runconf` change the value in both lines
- `/volume1/@appstore/CrashPlan/synopackage.vars` be sure to remove the `#` to uncomment the line

Restart the Crashplan package for the changes to take effect, and the backup should resume normally.

The above fix is recommended by [Code42 themselves](http://support.code42.com/CrashPlan/Latest/Troubleshooting/CrashPlan_Closes_Unexpectedly) as a solution if Crashplan repeatedly starts and stops. In addition, they also recommended values depending on the size of your backup.


|  Backup Selection Size | Recommended Memory Allocation (MB)  |
|---|---|
|Up to 1 TB or up to 1 million files   | 1024 (default)  |
|  1.5 TB or 1.5 million files | 1536  |
| 2 TB or 2 million files  | 2048  |
| 2.5 TB or 2.5 million files  | 2560  |
| 3 TB or 3 million files  |  3072 |

<br>
## Worthwhile places to bookmark##
- **[pcloadletter.co.uk ](http://pcloadletter.co.uk/2012/01/30/crashplan-syno-package/)**- patters is the creator of the Crashplan package and the comments section is helpful with how to solve various problems
- **[chrisnelson.ca ](http://chrisnelson.ca/?s=crashplan&searchsubmit=)**- chris usually has a workaround posted whenever Crashplan breaks
- **[forums.synology.com ](https://forum.synology.com/enu/)**- there is bound to be a Crashplan topic with someone experiencing the same problems, and if you're lucky a solution might be present
