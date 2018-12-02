---
excerpt: "This was my blog post related to this project.\r\n\r\nI was looking at the
  old live cd available at http://www.superunprivileged.org the other day when the
  thought of updating it came. They said that they had hacked the GNU Mach for creating
  the LiveCD. The packages available in the LiveCD are too out-dated.\r\n\r"
categories: [page, project]
layout: page
title: GNU HURD Live CD/ Installer
created: 1268976374
---
This was my blog post related to this project.

I was looking at the old live cd available at http://www.superunprivileged.org the other day when the thought of updating it came. They said that they had hacked the GNU Mach for creating the LiveCD. The packages available in the LiveCD are too out-dated.

I started re-mastering the ISO. They provided a script for this and it had to be slightly tuned to match our requirements. And I also upgraded the packages. The simple method was to use the packages from the qemu image that was installed using K16. Actually, instead of using just the packages I used the actual file system. Some of the directories like /dev should not be copied but must me created in the iso directly. The script replaces the runsystem file that is called after booting. This is the main file that is hacked. It can be used to launch a shell propmt. Also the translators for /server is started by this script. This was my first iso image. And to my surprise, without the GNU Mach hack, the ISO did boot.

It booted in qemu and gave me a shell. I found that I can do a lot of simple things using this shell. But file-system lacked the write permission. This was when I thouoght about creating a ramfs. But that was not implemented in Hurd. As youpi pointed out to me, a translator tmpfs was present that has function similar to ramfs. The difference is that ramfs is expandable while tmpfs is not. It seems ramfs is better as some people suggest.

It was at this time that Schwinge updated a page about storeio. He said that it can be used to create a ramdisk using a "copy with zero backend". The idea can be seen here. I used this script in a usual Hurd and found that it actually works. So I tried the same in the LiveCD I had remastered. And as expected it worked! I can now touch, mkdir, cp, mv etc onto this ramdisk. The ramdisk is created by allocating some space in ram, formating it to ext2 and then mounting it. The way the ext2fs.static translator is started is quite interesting out here.

I then learned about creating a chroot environment. I had not tried it before! So I tried it in a normal Hurd first found that I had to do a lot of work.. To be exact I had to do the following steps:

1. Copy folders like /bin, /servers, /hurd, /lib, /sbin etc to the mounted ramdisk from the LiveCD. IMP: Dont copy /dev
2. Create some directories like /dev, /etc, /media
3. Copy some of the shell scripts (these were created later on to simplify my works!).

Now we have all the required files and so we can chroot into the system. As a matter of fact, chroot can be used to run commands in general. For us it will be meaningful if we get a shell prompt running with / as the ramdisk. This can be got by running the command "chroot ramdisk sh". This means run the command sh with / as ramdisk. And i got a shell. When I did ls, I found that I can see only those files that I copied from the cd to ramdisk. I thought that everything will work out of box. But no! I had to set a lot of translators, create devices etc. The required steps are:

1. Run translators pflocal, pfinet, exec, password, proxy-defpager etc These can be seen in the native-install scripts.
2. Now cd into /dev and run MAKEDEV std. This will create a lot of devices like klog, stdin, stdout etc. And I didnt do this initially and created a lot of trouble. Thanks to Neal and Youpi! They should have been really irritated by my ignorance and lack of proper communication!! "You are not answering my question" was a statement that Youpi said too often!!!

Thats it. Now everything will work fine!!! Actually now we lack a password file in /etc. This can give us warning that the user cant be found. But dont worry. That aint a problem. But if you want to correct that copy the file /usr/shar/base-passwd/passwd.master from LiveCD to the ramdisk's etc/passwd.

I was then curious to find out whether I would be able to mount a hdd that is present in the machine that runs the LiveCD. I thus attached a hdd.img to hda to qemu. I created the chroot environment in ramdisk and ran an sh. I did the required initialisation and then created a node hd0. When I ran fdisk hd0, I found that the device was detected. I created a partition in it.

youpi: did you restart the kernel after partitioning ?
maheshm: i am running live-cd!
youpi: and so?
maheshm: whats the use in reboot?
youpi: that doesn't mean the kernel will magically re-read the hdd partition
maheshm: i had tried with (an already) partitioned disk initially (This has been edited)
maheshm: same error
maheshm: so tried fdisk
maheshm: fdisk said unable to read /dev/hd0

So you have to reboot after the partitioning. The kernel re-reading the partition is just a "yet-to-be-implemented" thing. (And I hope that it will be implemented soon. That will reduce the number of restarts during installation.) Again chrooted a shell and created a directory in /media, say hdd. Then, I tried mounting it. And it worked!!

This gave me confidence to proceed creating an installer for Hurd. I was reminded about the fact that GRUB is broken and I also had to learn about the Debian Installer as it exists for GNU/Linux. I though of not going into d-i now, but use the existing K16 installers baseGNU.tgz file and the steps it use. The LiveCD created was capable of replacing the Linux kernel from the K16 installer. I mounted the K16 installer ISO and took the baseGNU.tgz and put it in LiveCD's ISO (by editing the remaster script to include this file). Some more script editing was done in runsystem and user is given an option to choose between installation and LiveCD. Insatallation assumes too much as of now, that the target is hd0, use the whole hd0, create only hd0s1 (Is this M$ Window$ or what!! Will change it soon), partition it and make it /. Now waiting for Philip C to give his code that can be reused to a lot of extend.

---------------------------------------

Well, I was not aware of a lot of things then. Had checked the code in "parted" where I found that the function, that was supposed to tell the Mach to re-read the partition, was empty. Planning for a patch.

The code has been uploaded to Sourceforge svn. Use the following command to checkout the code.

svn co https://hurdlivecd.svn.sourceforge.net/svnroot/hurdlivecd hurdlivecd

(C, Shell scripts)
