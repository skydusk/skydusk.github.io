---
layout: post
title:  "Migrating a Synology volume from SHR to SHR2"
date:   2015-08-27 16:08:03
categories: 
---
<br>
When I first setup my NAS I opted for SHR [(Synology Hybrid Raid)](https://www.synology.com/en-us/knowledgebase/tutorials/492) with 1 disk redundancy. Since then I’ve seen storage prices tumble, and with the future of big storage looking so promising (Samsung 16TB SSD anyone?) I realized SHR2 with 2 disk redundancy would have been a better option, after all the only constant with hard drives is that they are bound to fail sooner or later. Unfortunatley whatever you select when a volume is created cannot be changed, so the following is the next best solution when you want to migrate a volume to a different type of RAID

> `Old SHR setup`
> 
> - 5x 6TB drives (~24TB storage with only ~1.7TB free)
> - 3x Uninitialized 6TB disks on standby
> 
> `New SHR2 setup`
> 
> - 8x 6TB drives (~36TB storage with 15.2TB free)



My initial plan was to simply format the 3 unused disks and create a new SHR2 volume with them, but at least 4 disks are needed to create a SHR2 volume, so the easy option was not possible here.


## How to: ##

1. I first copied as much data as I could fit onto my PC’s various external drives from the NAS (Seagate’s 8TB Archive HDD really came in handy here). For large file transfers I would recommend a program called [Teracopy](http://www.codesector.com/teracopy) (its fast, reliable, supports pause/resume and has file validation)
2. In addition, I SSHd into the NAS with [WinSCP](https://winscp.net/eng/index.php) and copied the entire @appstore folder where all installed packages are stored. This makes things much easier as you will need to reinstall all downlaoded packages again.
3. Shut down the Synology and removed 1 drive (nervously I might add). Upon rebooting, the NAS signaled a drive was missing with a beep, and that the volume was degraded (the data was still fine as SHR provides 1 disk redundancy). I ackowledged the warnings to stop the beeping, and then everything carried on as usual despite there being a missing disk (gotta love redundancy eh?)
4. Inserted the removed drive into my PC and formatted it. Reinserted the now fresh drive into the NAS and created a SHR2 volume2 with the 3 uninitialized disks with a capacity of around 12TB. The data still left on volume1 that I could not copy to my PC at the start totalled around 10TB, which was small enough to fit onto the newly create volume2
5. Then it was just a matter or changing the location of that shared folder In the DSM control panel > shared folder > edit location. The NAS then began moving whatever was left on volume1 to volume2. The process is automatic and shows a progress bar, but you can also see that its working by checking the available space for volume2 (it should start slowly filling up with the data). Another way is to see disk activity in resource monitor, mine was reading and writing at 250-300mb/s
6. Once I confirmed all the data was now on volume2, I erased the old volume and began to expand volume2 using the 5 empty disks that had once formed volume1. Because Synology still gives you access to the NAS during volume expansion, this process is quite slow. I foind that not using the NAS and letting it focus on expanding the volume sped things up quite significantly. Only after it had completed did I copy all the data back over to the NAS from my PC (once again using [Teracopy](http://www.codesector.com/teracopy) as its very reliable)
7. The final step was reinstalling packages again from Synology’s package centre


Below is the system log as I went through the steps

![](https://raw.githubusercontent.com/skydusk/skydusk.github.io/master/assets/logs.png)



## Configuring Packages (backups and torrents) ##

**Crashplan:** Installing this was easy as most of the setup has been done the first time round. There is no need to change anything on the Windows client side either as long as the IP address of the NAS remains static. The only  minor issue is that your data is now located on volume2 instead of volume1, and the Crashplan client will tell you the files are missing. The solution is to add those exact same files and folders you had before to the backup selection as follows:

![](https://raw.githubusercontent.com/skydusk/skydusk.github.io/master/assets/selection.png)

Crashplan will then go through its data deduplication process and realize that its already backed up those files. After synchronizing file and block information the backup will resume, but at a much faster rate where it is just scanning through everything already uploaded. Once the backup is finished, you can remove the old files and folders which were located on volume1 from the selection if you don’t want to see the missing files notices anymore

**Transmission:** Just like Crashplan reinstalling the Transmission torrent client was easy. But just like Crashplan it thinks files are in locations on volume1. The solution is to edit all the .RESUME files in the @appstore​ folder you copied in step 2 (@appstore>transmission>var>resume) using [Notepad++](https://notepad-plus-plus.org/). I had 500+ files which I was able to open at the same time, and then do a find and replace on all of them changing any instance of “volume1″ to “volume2″. Then its a matter of copying those .RESUME files (as well as the .torrent files located in appstore>transmission>var>torrents back to the NAS using [WinSCP](https://winscp.net/eng/index.php) again. Transmission will pick up right where it left off like nothing has changed.
