---
categories: [tech-blog]
layout: post
title: Hurd Installation... Its cool
created: 1181406120
---
'Installing GNU/Hurd On a Linux System'

GNU/Hurd is an advanced kernel that is still under contruction. No official release of the same has been made yet. It works more or less in a network fashion. A micro-kernal like GNU/Mach is the base for the GNU/Hurd. A collection of Hurds together services the requests of the user. This is the main difference from the traditional monolithic kernels like Linux. Hurds communicate with each other using the micro-kernel. Currently there is a working package available for installation, though it comes with a big warning saying that 'Use at your own risk'. Its still unstable and a lot of works are to be done. The only thing that has been ported top the Hurd is the Debian. This means that the Debian packages may be run with Hurd as the Kernel. Again, not all the packages have been ported yet and so you cant expect a smooth GNOME or KDE desktop. The available desktop is 'fvwm', a very old one. It also has a million bugs, it seems...

Installation as explained by the team seems to project that its too dangerous to install the Hurd if you are a newbie. Well, dont go by their words. Have the courage to learn. Its actually quite simple, especially if you already have a Linux running. If you dont have a Linux, better contact Kiran. He and his team has made an installer for the Debian GNU/Hurd K-14 CD. The CD itself is quite an easy and user-friendly one. Here i will explain how to install the GNU/Hurd if you already have a Linux running on your system.

Requirements:

1. A computer with some Linux version.

2. 1 to 2 GB of free hard-disk space.

3. Debian GNU/Hurd installation disk, part 1.

4. Lots and lots of courage.

The most important point to note is that currently Hurd supports approx. 2 GB only. and the File System must be ext2fs. In the Linux box run parted or gparted to do this. Make a partion of size 2GB and ext2 file format. You must be root! Lets say that you are going to install in hda2. Run the following command to make the partion a Hurd's property. It will also make it ext2. (I am not sure wheather this is required)

       `mke2fs -o hurd /dev/hda2`

Now edit the GRUB. This may be done at a later stage, but its better to do it now. Add the following to the `menu.lst` in the `/boot/grub/` if you are running Debian. I think its in `/boot/grub/grub.conf` in RH systems(?). Add the following to it...
```sh
         title        GNU/Hurd

        kernel         (hd0,1)/boot/gnumach.gz root=device:hd0s2 -s

        module         (hd0,1)/hurd/ext2fs.static \

                   --multiboot-command-line=${kernel-command-line} \

                   --host-priv-port=${host-port} \

                   --device-master-port=${device-port} \

                   --exec-server-task=${exec-task} -T typed ${root} \

                   $(task-create) $(task-resume)

            module         (hd0,1)/lib/ld.so.1 /hurd/exec $(exec-task=task-create)

            boot
```
Here it must be noted that the naming convention for the hard-disk is quite different. We normally use a Primary master disk. Its denoted as hd0. A secondary slave will be hd3. The partition is reffered as a slice, denoted by s. Grub uses 0 as the base while this method uses 1, ie, first slice or partion is s1. So hd0,0 is represented as hd0s1.

The root= parameter passed to the kernel is an indication to Mach, the micro-kernel, where the Hurd is situated. Grub need not know where it is. -s is used to enable a single user mode. After installation is complete, we got to remove this.

Now reboot the system. Boot from the Debian GNU/Hurd cd. Do the base-installation after enabiling the swap space and the file-system. No need of partioning again. It seems that some bugs are still there in the parted provided, when run in some systems. Base-install copies the required files to the ext2 FS that was enabled. Now take the CD out and reboot.

Select from the Grub menu, the GNU/Hurd option that we had put.This will boot the system. Now login as root. There is no password and the format is 'login root'. Then run the native-install file.

```sh
./native-install
```

This will install the downloaded packages. The installation is not yet completed. Reboot the system and login again as root and re-run the native-install. Now the most important packages are installed.

NOTE:At this point there is no vim or other editors than 'nano'. Just in case you need one!

Now what you have is a bare system that needs to be configured. There wont be any devices. So...

```sh
    cd /dev

    ./MAKEDEV hd2                         --&gt; hd2 is CDROM
```

You may also add devices if there are any other ext2 or iso9600 FS partions.
Dont try to mount it now. It will not work.

Edit the file /etc/fstab using

```sh
    nano /etc/fstab

    /dev/hd0s6       none    swap        sw            1       1

    /dev/hs2    /cdrom  iso9660fs    ro,noauto      0       0
```

You can always refer your Linux system to find out which is the swap drive or any other you want.

Simply typing the traditional mount /cdrom will work. Though its better to mount it using the following command so that you get a feel of what Hurd is. Make sure you have created the directory /cdrom using the mkdir command.

```sh
settrans -fga /cdrom /hurd/iso9600fs /dev/hd2
```

This will enable the translator iso9600fs which points to `/dev/hd2` device and the translator will be placed at /cdrom. The swap space is enabled by the command

```sh
    swapon -a
```

Run `/cdrom/upgrade/install.sh`. This will install the other packages available in the CD. gui.sh in the same directory will install the GUI. Further installation of the same are in the Cook-book provided in the CD. Also the hurd-install-guide is a good reference at anytime.

One last thing required to be done is changing from single user-mode. Do this by editing the menu.lst file as mentioned earlier. Just remove the -s option. This can be done only in the Linux mode since we didnt install another Grub and the Grub's configuration files are still in the ext3 drive that we cant access from the Hurd (what we need is a ext3 FS translator inside the /hurd/ folder, like the one we have for ext2 and iso9600fs).

HAPPY HACKING...
