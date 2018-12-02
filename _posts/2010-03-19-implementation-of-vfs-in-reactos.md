---
excerpt: "The aim of this project was to remove the usual way in which the NT Kernel
  loads a file system and then implement Virtual File System layer in it, similar
  to one found in Linux Kernel. Usual way of NT Kernel to support a FS is by using
  the concept IFS or Installable File System which requires creation of a driver for
  that particular FS.\r\n\r\nFirst I had to study the way in which the Linux Kernel
  handles IO requests, how VFS hides the details regarding the FS from the process
  requesting data residing in that FS.\r\n\r"
categories: [page, project]
layout: page
title: Implementation of VFS in ReactOS
created: 1268976097
---
The aim of this project was to remove the usual way in which the NT Kernel loads a file system and then implement Virtual File System layer in it, similar to one found in Linux Kernel. Usual way of NT Kernel to support a FS is by using the concept IFS or Installable File System which requires creation of a driver for that particular FS.

First I had to study the way in which the Linux Kernel handles IO requests, how VFS hides the details regarding the FS from the process requesting data residing in that FS.

Believe me. NT kernel has a totally different style of handling things. To fit the Linux way into NT was very tough. Now it seems though that the efforts were not that worthy even though the experience is priceless.

This was my academic main project. It was not a complete success. The code never compiled successfully and completely.

I will upload the code soon in the GIT Hub.
