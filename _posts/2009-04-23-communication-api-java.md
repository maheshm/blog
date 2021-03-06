---
categories: [tech-blog]
layout: post
title: Communication API, JAVA
created: 1240438980
tags: [java]
---
Java provides communication API for the programs to communicate via Serial and Parallel ports. The funny part is that there are drivers required for the whole thing to work. RXTX works for providing the drivers. Though the steps described in some pages are quite outdated, they can be helpful. Some links are provided below:

<cite><b>java</b>.sun.com/products/<b>javacomm</b>/</cite>

<cite>www.agaveblue.org/howtos/Comm_How-To.shtml </cite> --&gt; Be careful!!

There may be better docs available though. I'm in a hurry.. :)

UPDATED (20-05-2009)
---

It seems that whatever I land up on is a troublesome thing. The javacomm API doesnt work properly in GNU/Linux. They have stopped caring about CommAPI it seems, as many blogs/reviews suggest. The drivers required are mainly for serial port. And that too doesnt seem to work for me!

Well I had RXTX drivers for my rescue. They are the backend support for the CommAPI. It was the one that finally worked for me.

[www.rxtx.org](http://www.rxtx.org)

They have a good documentation and a wiki. The thing I was stuck was with the javax.comm.properties that must be added to the JRE folder.

Serial port support is ample, though many baudrates are not available. But that is the limitation from CommAPI. Sun provides a program blackbox for both serial as well as parallel port. It can be used as reference as well as test programs.

Support of parallel port is limitted. You can do the most basic things using the SPP mode. Various modes are there, I dont know how they are useful. They say that these things are required for Printer support. Ah! If you are using Parallel port for you Robot and if you are planning to use Java, make sure you ground all the pins from 18 through 25. Some more connections are required that tells the driver that the printer has no error, its not out-of-paper, it is online, its not busy etc. These are looked into by the driver.

That was the biggest realisation for me. My program was not sending any data through parallel port. Then I saw in some web page about the above mentioned connections. That made me realise that Java API also look into all these. Conformation was got when I tried isBusy() (or something similar) API that said that it is actually busy! While the great thing Python, it simply sets data in the parallel port register and we have it in the pins 2 to 9. No worry about out-of-paper or busy!!

Though I was warned that parallel port must be handled with care and that it can harm the mother board, care was taken that it didnt suck much current.
