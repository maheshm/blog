---
excerpt: "It was started as a KDE project. The Calender in KDE provides facilities
  to add different calenders in it. Praveen and Santhosh already did some work and
  added North Indian Calender to it. It was then understood that the base code itself
  was buggy.\r\n\r"
categories: [page, project]
layout: page
title: Implementation of Malayalam Calender
created: 1268976181
---
It was started as a KDE project. The Calender in KDE provides facilities to add different calenders in it. Praveen and Santhosh already did some work and added North Indian Calender to it. It was then understood that the base code itself was buggy.

Santhosh gave me some Java code that implemented the Malayalam Calender. The code was too big and had implemented many western calenders as well. I started by isolating the Malayalam calender code. I was able to implement it to some extend in C++ and also in Python. The code defined a lot of Astronomical functions. Right now I am creating a library of these functions so that it can be re-used.

The code calculates everything based on the assumption that time at Ujjaini is IST. This has to be reworked and it must be calculated based on the lat and long that the user specifies.

Base codes are available in SMC webpage.

(C++, Python)
