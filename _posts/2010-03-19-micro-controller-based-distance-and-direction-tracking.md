---
excerpt: "This was an existing project. I had got some crude code and some data sheets
  and some electronic items. I was asked to help my cousin by building a robot that
  could guide itself in certain direction without hitting anything. Even he knew that
  was not that easy.\r\n\r\nI read the code and was inspired to correct it. I had
  Atmega32, an LCD module, a digital compass and some digital distance measuring device
  (I forgot its name). Data sheets of all these were available. I started coding from
  the scratch as the code I had got was good for nothing.\r\n\r"
categories: [page, project]
layout: page
title: Micro-controller based distance and direction tracking
created: 1268975905
---
This was an existing project. I had got some crude code and some data sheets and some electronic items. I was asked to help my cousin by building a robot that could guide itself in certain direction without hitting anything. Even he knew that was not that easy.

I read the code and was inspired to correct it. I had Atmega32, an LCD module, a digital compass and some digital distance measuring device (I forgot its name). Data sheets of all these were available. I started coding from the scratch as the code I had got was good for nothing.

Atmega communicated with LCD, compass and that "sonar" device using I2C bus. I liked that method. All had a preset address space in which different services of it was exported.

The code was burned. Compass required some calibration and that was a bit tough. When connected and power was supplied, the LCD module displayed the direction in which it was pointing. The measuring thing was showing garbage values, perhaps it was damaged.

Wanted to add it as a module to Freebird project. But time didnt allow!

(C, embedded system)
