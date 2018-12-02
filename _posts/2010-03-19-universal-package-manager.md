---
excerpt: "I still remember a long fight with Praveen with regards to the usability
  of the method in which GNU/Linux handled packages installation. Sticking repositories,
  lack of interoperability among different flavors, etc were my points while he was
  trying to show the Engineering beauty of the Debian way, apt-get.\r\n\r"
categories: [page, project]
layout: page
title: Universal Package Manager
created: 1268976053
---
I still remember a long fight with Praveen with regards to the usability of the method in which GNU/Linux handled packages installation. Sticking repositories, lack of interoperability among different flavors, etc were my points while he was trying to show the Engineering beauty of the Debian way, apt-get.

Even though this aint anything close to what I wanted, it was a step (cant say forward, but it was really a step). The small code handled .deb, .rpm and the src packages in a primitive way. In deb pre- and post- scripts do not run, and src handling is just kind of a joke, since it worked only when the makefile worked in the standard way: ./configure; make; make install.

The main thing was that it installed everything related to an application in a single directory and provided symbolic links in standard directories where the file would have other wise resided. That was the main and most important script that helped me call it a success.

This meant I just had to delete the directory in which the application was installed to remove application completely from the system. Also it helped in support of different versions of same application to be installed on same system. We can use either of the version by appropriately pointing the files to the files of required version of library. They are all not completely implemented. But they are trivial.

Code is available in GIT Hub. Modifications/contributions are welcome. Cant find the code of the GUI of the same.

(C, Shell script)
